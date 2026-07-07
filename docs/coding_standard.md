# Coding Standard

## Purpose

This document defines the coding conventions used in the project.

The goal is to keep the code readable, consistent, and maintainable across different application components.

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

## Naming Convention

The project uses a combination of type prefixes and role-based naming.

Prefixes are used where they improve readability, online monitoring, and consistency.

---

## Type Naming

### Function Blocks

Function block types use the `FB_` prefix.

```text
FB_AxisModule
FB_PumpModule
FB_SequenceManager
```

Function block instances use the `fb` prefix.

```text
fbAxis
fbPump
fbSequenceManager
```

---

### Structures

Structure types use the `ST_` prefix.

```text
ST_MachineStatus
ST_MachineParameters
ST_OperatorCommands
```

Structure instances use the `st` prefix.

```text
stMachineStatus
stMachineParameters
stOperatorCommands
```

---

### Enumerations

Enumeration types use the `E_` prefix.

```text
E_OperatingMode
E_MachineState
E_AxisCommand
```

Enumeration variables use the `e` prefix.

```text
eOperatingMode
eMachineState
eAxisCommand
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
Command
Status
Parameters
Request
Actual
Target
Fault
```

Examples:

```text
stAxisCommand
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
Parameters
Status
```

Each component shall have a clearly defined owner and responsibility.

---

## Function Block Design

A function block should represent one clear responsibility.

A function block shall not directly modify the internal state of another function block.

Where possible, function blocks should communicate through explicit command and status data.

Typical function block interface:

```text
Inputs
Outputs
Parameters
Status
Diagnostics
```

Not every function block needs every interface element.

Unused signals shall not be added only for symmetry.

---

## State and Command Types

Explicit states and commands should be represented using enumerations.

Preferred:

```text
eMachineState : E_MachineState;
eAxisCommand  : E_AxisCommand;
```

Avoid using plain integers or multiple unrelated booleans to represent complex states.

---

## Error Handling

Faults shall have a clear owner.

A component that detects and owns a fault is responsible for reporting it.

Faults should not reset automatically unless explicitly defined.

Fault reset shall be intentional and traceable.

Fault codes should be represented using an enumeration where practical.

---

## Application Entry Point

The application shall have a single entry point.

Where supported by the target PLC platform, the main program should be named `Application`.

If the platform requires a predefined program name (for example `PLC_PRG`), the main program shall contain only the application entry point.

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
