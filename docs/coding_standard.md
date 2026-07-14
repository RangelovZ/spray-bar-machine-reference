# Coding Standard

## Purpose

This document defines the coding conventions used in the project.

The goal is to keep the code readable, consistent, deterministic, and maintainable across different application components.

---

## General Rules

* Prefer readability over compact code.
* Prefer explicit logic over implicit behaviour.
* Avoid duplicated logic.
* Avoid magic numbers.
* Keep each component focused on one responsibility.
* Use meaningful names that describe intent.
* Keep interfaces small and well-defined.

---

## Application Entry Point

The application shall have a single entry point.

Where supported by the target PLC platform, the main program should be named `Application`.

If the platform requires a predefined program name, such as `PLC_PRG`, the main program shall contain only the application entry point.

Preferred structure:

```text
PROGRAM Application
        ↓
FB_Application
```

Fallback structure:

```text
PROGRAM PLC_PRG
        ↓
FB_Application
```

The main program shall not contain application logic.

Its only responsibility is to instantiate and execute `FB_Application`.

---

## Component Interface Direction

Application components shall use deterministic data exchange through explicit inputs and outputs.

Shared application data shall follow ownership rules:

| Data        | Owner            | Interface Direction       |
| ----------- | ---------------- | ------------------------- |
| Commands    | Manager          | `VAR_INPUT` for consumers |
| Parameters  | Application      | `VAR_INPUT` for consumers |
| Status      | Owning component | `VAR_OUTPUT` from owner   |
| Diagnostics | Owning component | `VAR_OUTPUT` from owner   |

Application components shall only modify data they own.

Data owned by other components shall be treated as read-only.

`VAR_IN_OUT` should be avoided for application components unless shared ownership is explicitly required and documented.

Typical module interface:

```text
VAR_INPUT
    stCommands
    stParameters
END_VAR

VAR_OUTPUT
    stStatus
    stDiagnostics
END_VAR
```

Typical manager interface:

```text
VAR_INPUT
    stCommands
    stParameters
    stModuleStatus
    stDiagnostics
END_VAR

VAR_OUTPUT
    stStatus
    stModuleCommands
END_VAR
```

---

## Naming Convention

The project uses a combination of type prefixes and role-based naming.

Prefixes are used where they improve readability, online monitoring, and consistency.

---

## Type Naming

### Function Blocks

Function block types use the `FB_` prefix.

```text
FB_Application
FB_AxisModule
FB_PumpModule
FB_SequenceManager
```

Function block instances use the `fb` prefix.

```text
fbApplication
fbAxis
fbPump
fbSequenceManager
```

---

### Structures

Structure types use the `ST_` prefix.

```text
ST_Commands
ST_Parameters
ST_Status
ST_Diagnostics
ST_AxisCommands
ST_AxisStatus
```

Structure instances use the `st` prefix.

```text
stCommands
stParameters
stStatus
stDiagnostics
stAxisCommands
stAxisStatus
```

---

### Enumerations

Enumeration types use the `E_` prefix.

```text
E_OperatingMode
E_MachineState
E_AxisCommand
E_AxisState
```

Enumeration variables use the `e` prefix.

```text
eOperatingMode
eMachineState
eAxisCommand
eAxisState
```

Enumeration values use uppercase names.

```text
AUTOMATIC
MANUAL
MAINTENANCE
STOPPED
RUNNING
FAULT
```
### Enumeration Values

Enumeration values should use explicit numeric assignments when they represent application states, commands, operating modes, alarms, faults, or other values that may be exchanged with external systems or persisted over time.

Command, fault, and warning enumerations should reserve value `0` for `NONE` whenever applicable.

State enumerations should use the first valid operational state as value `0` rather than introducing an artificial `NONE` state.

Examples:

```text
E_AxisCommand
    NONE            := 0
    HOME            := 1
    MOVE_ABSOLUTE   := 2
    START_CYCLING   := 3
    STOP            := 4
```

```text
E_AxisState
    NOT_HOMED       := 0
    HOMING          := 1
    IDLE            := 2
    POSITIONING     := 3
    CYCLING         := 4
    STOPPING        := 5
    FAULT           := 6
```

---

## Variable Prefixes

Basic variables use lightweight prefixes.

```text
x   BOOL
r   REAL / LREAL
i   INT
ui  UINT
s   STRING
t   TIME
dt  DATE_AND_TIME
```

Examples:

```text
xEnable
xReady
xBusy
xDone
xFault

rStartPosition
rMachineWidth
rCurrentPosition
rTargetPosition

iRetryCount
uiAlarmCount

sRecipeName
tHomingTimeout
dtLastService
```

---

## Role-Based Naming

Role-based names may be used when they better describe intent than a data type alone.

Common role names include:

```text
Commands
Parameters
Status
Diagnostics
Request
Actual
Target
Fault
```

Examples:

```text
stAxisCommands
stAxisStatus
stMachineParameters

xStartRequest
xStopRequest
xResetRequest

rActualPosition
rTargetPosition
```

---

## Software Organization

Application code should be organized around responsibilities.

The main component categories are:

```text
Interfaces
Managers
Modules
Data
```

Each component shall have a clearly defined owner and responsibility.

---

## Function Block Design

A function block should represent one clear responsibility.

A function block shall not directly modify the internal state of another function block.

Function blocks should exchange data through explicit input and output structures.

Typical function block data groups:

```text
Commands
Parameters
Status
Diagnostics
```

Not every function block needs every data group.

Unused structures or signals shall not be added only for symmetry.

---

## State and Command Types

Explicit states and commands should be represented using enumerations.

Preferred:

```text
eMachineState : E_MachineState;
eAxisCommand  : E_AxisCommand;
```

Avoid using plain integers or multiple unrelated booleans to represent complex states.

Commands represent requests.

States represent current component condition.

---

## Error Handling

Faults shall have a clear owner.

A component that detects and owns a fault is responsible for reporting it.

Faults should not reset automatically unless explicitly defined.

Fault reset shall be intentional and traceable.

Fault codes should be represented using an enumeration where practical.

---

## Comments

Comments should explain why something is done, not simply repeat what the code already says.

Avoid comments like:

```text
// Turn pump on
```

Prefer comments that explain intent:

```text
// Pump is enabled only during automatic production.
```

Good naming and clear structure should reduce the need for explanatory comments.

---

## Formatting

Use consistent indentation and spacing throughout the project.

Keep conditional logic readable.

Avoid deeply nested logic where a small state machine or helper function would be clearer.

Use blank lines to separate logical sections of code.

---

### PLCopen Motion Function Blocks

PLCopen motion function blocks shall be called directly inside the state that owns them.

Avoid unnecessary wrapper methods around PLCopen function blocks unless they provide significant code reuse.

Prefer explicit `Execute := TRUE/FALSE` logic, making the execution flow easy to understand during online debugging and commissioning.

For this project, readability and maintainability take precedence over excessive abstraction.

Each PLCopen function block instance shall belong to a single state or sub-state.

The ownership of a PLCopen function block must be obvious from the state machine implementation.
