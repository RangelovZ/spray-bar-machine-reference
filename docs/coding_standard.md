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

The main program acts as the composition root of the application. It instantiates the main application components, owns shared application data, and defines their execution order.

Application logic should remain inside the corresponding managers and modules rather than being implemented directly in the main program.

---

## Component Interface Direction

Application components shall exchange data through explicit inputs and outputs.

Shared application data follows clear ownership rules:

| Data        | Owner            | Interface Direction       |
| ----------- | ---------------- | ------------------------- |
| Commands    | Manager          | `VAR_INPUT` for consumers |
| Parameters  | Application      | `VAR_INPUT` for consumers |
| Status      | Owning component | `VAR_OUTPUT` from owner   |
| Diagnostics | Owning component | `VAR_OUTPUT` from owner   |

Components shall modify only the data they own.

`VAR_IN_OUT` should be avoided for normal application data exchange.

An exception is infrastructure references required by the target platform or motion-control library, such as a SoftMotion axis reference. In such cases, `VAR_IN_OUT` does not imply shared ownership of application state.

---

## Naming Convention

The project uses type prefixes and role-based naming where they improve readability, consistency, and online monitoring.

### Function Blocks

Function block types use the `FB_` prefix.

```text
FB_AxisModule
FB_PumpModule
FB_ValveModule
FB_SequenceManager
```

Function block instances use the `fb` prefix.

```text
fbAxis
fbPump
fbValve
fbSequenceManager
```

### Structures

Structure types use the `ST_` prefix.

```text
ST_Commands
ST_Parameters
ST_Status
ST_AxisCommands
ST_AxisStatus
```

Structure instances use the `st` prefix.

```text
stCommands
stParameters
stStatus
stAxisCommands
stAxisStatus
```

### Enumerations

Enumeration types use the `E_` prefix and enumeration variables use the `e` prefix.

```text
E_OperatingMode
E_MachineState
E_AxisCommand
E_AxisState

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

Explicit numeric assignments should be used when enumeration values may be exchanged with external systems or persisted over time.

Command, fault, and warning enumerations should reserve value `0` for `NONE` where applicable.

State enumerations should use the first valid operational state as value `0` rather than introducing an artificial `NONE` state.

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
xFault

rStartPosition
rTargetPosition

iRetryCount
uiAlarmCount

sRecipeName
tHomingTimeout
dtLastService
```

---

## Role-Based Naming

Role-based names should describe the purpose of application data.

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

Application code is organized around clear responsibilities:

```text
Interfaces
Managers
Modules
Data
```

Each component should have one clearly defined responsibility.

Function blocks should encapsulate one subsystem or application concern and exchange data through explicit command, parameter, status, and diagnostic structures where applicable.

Components shall not directly modify the internal state of other components.

Unused structures or signals should not be introduced only for symmetry.

---

## State and Command Types

Explicit states and commands should be represented using enumerations.

```text
eMachineState : E_MachineState;
eAxisCommand  : E_AxisCommand;
```

Avoid using plain integers or multiple unrelated booleans to represent complex machine states.

Commands represent requested actions.

States represent the current condition of a component.

---

## Error Handling

Faults shall have a clearly defined owner.

The component that detects a fault is responsible for reporting it.

Faults should not reset automatically unless explicitly defined.

Fault reset shall be intentional and traceable.

Fault codes should use enumerations where practical.

---

## Comments

Comments should explain intent or reasoning rather than repeat what the code already states.

Prefer:

```text
// Keep the air valve open during the configured blow-off delay after stopping.
```

over:

```text
// Open air valve.
```

Clear naming and structure should reduce the need for excessive comments.

---

## Formatting

Use consistent indentation and spacing throughout the project.

Keep conditional logic readable and avoid unnecessary nesting.

Use blank lines to separate logical sections of code.

Prefer explicit and easy-to-debug logic over compact expressions.

---

## PLCopen Motion Function Blocks

PLCopen motion function blocks should be called directly from the state or sub-state that owns the motion operation.

Prefer explicit `Execute := TRUE/FALSE` control so that command execution is easy to follow during online debugging and commissioning.

Avoid unnecessary wrapper layers unless they provide clear reuse or architectural value.

Each PLCopen function block instance should have clear ownership within the motion state machine.
