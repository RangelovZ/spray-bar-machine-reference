# Machine Specification

## Purpose

This document defines the functional behavior of the Spray Bar machine.

It describes what the machine does from an operational perspective without specifying implementation details.

The specification is independent of the software architecture, PLC platform, HMI, or communication technology.

---

## Machine Overview

The Spray Bar machine performs a controlled back-and-forth movement over a configurable working area while applying a fluid-air mixture through a spray system.

The machine consists of:

* One horizontal driven axis
* One pump
* One air valve

The working area is defined by configurable start and final positions.

During automatic production, the axis continuously moves between these positions while the spray system is active.

The machine also supports manual operation, maintenance positioning, homing, process monitoring, and controlled handling of stop and fault conditions.

---

## Operating Modes

The machine supports three operating modes:

* Automatic
* Manual
* Maintenance

Only one operating mode may be active at any time.

If a mode change is requested while machine operation is active, the current operation is stopped in a controlled manner before the requested mode becomes active.

### Automatic

Automatic mode is intended for standard production.

When started, the machine verifies that the required operating conditions are satisfied.

If the axis is not homed, homing is performed before production starts.

The axis then moves to the configured Start Position, activates the spray system, and continuously moves between the Start Position and Final Position.

Automatic operation continues until a Stop command, process interruption, mode change, or fault condition occurs.

### Manual

Manual mode allows the operator to control individual machine functions for setup, testing, and adjustment.

Available functions include:

* Homing request
* Axis jogging
* Manual pump control
* Manual air valve control

Automatic production sequences are disabled in Manual mode.

### Maintenance

Maintenance mode is intended for servicing and machine access.

When Maintenance mode is entered, the axis performs a new homing sequence and then moves to the configured Maintenance Position.

After the maintenance position has been reached, the machine indicates that it is ready for maintenance activity.

---

## Machine States

The machine operates in one of the following states:

* Stopped
* Starting
* Running
* Stopping
* Fault

### Stopped

No automatic machine operation is active.

The machine is in a stable condition and may accept a new Start command or operating mode request.

### Starting

The machine is preparing for automatic operation.

Depending on the current condition, this may include:

* Checking operating conditions
* Performing homing
* Moving the axis to the configured Start Position
* Starting the pump
* Opening the air valve

### Running

The machine is performing normal automatic production.

The axis continuously moves between the configured Start Position and Final Position while the spray system is active.

### Stopping

The machine is performing a controlled stop.

Depending on the reason for stopping, this may include stopping axis motion, stopping the pump, and maintaining airflow for the configured air-blow delay.

### Fault

Machine operation is inhibited because an active fault condition exists.

The fault must be cleared and acknowledged before normal operation can resume.

---

## Operator Commands

The machine provides the following operator commands:

* Start
* Stop
* Reset
* Homing Request
* Jog Left
* Jog Right
* Manual Pump Control
* Manual Air Valve Control
* Operating Mode Request

### Start

Starts automatic operation when the required operating conditions are satisfied.

### Stop

Requests a controlled stop of the current machine operation.

Stop has priority over normal production commands.

### Reset

Acknowledges recoverable fault conditions and resets the corresponding machine or drive fault state when the underlying fault has been cleared.

### Homing Request

Requests a new axis homing sequence.

The command is primarily used during Manual operation.

### Jog Left / Jog Right

Commands manual axis movement in the selected direction.

Jogging is available only when allowed by the active operating mode and current machine condition.

### Manual Pump Control

Allows the pump to be switched on or off during Manual operation.

### Manual Air Valve Control

Allows the air valve to be opened or closed during Manual operation.

### Operating Mode Request

Requests a change between Automatic, Manual, and Maintenance operating modes.

If the machine is currently active, the requested mode becomes effective only after the current operation has been stopped in a controlled manner.

---

## Automatic Operation

Automatic operation requires:

* No active machine fault
* Required operating conditions to be satisfied
* Axis homing before production movement begins

When a Start command is accepted, the machine performs the following sequence:

```text
Check Operating Conditions
        ↓
Axis Homed?
   No ───────→ Perform Homing
        ↓
Move to Start Position
        ↓
Start Pump
        ↓
Open Air Valve
        ↓
Start Cycling
        ↓
Start Position ↔ Final Position
```

During normal production, the axis continuously moves between the configured Start Position and Final Position while the pump and air valve remain active.

If a Stop command or another condition requiring a controlled stop occurs, the machine performs the following sequence:

```text
Stop Axis Motion
        ↓
Stop Pump
        ↓
Keep Air Valve Open
        ↓
Configured Air-Blow Delay
        ↓
Close Air Valve
        ↓
Stopped
```

The exact continuation after a controlled stop depends on the reason that initiated it. For example, a mode change, process interruption, or fault may require a different subsequent action.

---

## Homing

The homing sequence establishes the mechanical reference position of the axis.

Homing is performed against a defined mechanical stop. The axis approaches the stop at the configured homing speed until the mechanical stop condition is detected.

The sequence performs the following operations:

```text
Move Towards Mechanical Stop
        ↓
Detect Mechanical Stop
        ↓
Stop Axis Motion
        ↓
Set Axis Position to Zero
        ↓
Release Mechanical Stop
        ↓
Homing Completed
```

The release movement moves the axis away from the mechanical stop after the reference position has been established.

Homing is considered complete only after the release movement has finished successfully.

The homing sequence establishes the axis reference only. It does not determine the subsequent machine position.

After homing is completed, the active operating mode determines the next action:

```text
Automatic   → Move to Start Position
Maintenance → Move to Maintenance Position
Manual      → Remain available for manual operation
```

Automatic production is allowed only when the axis has a valid homing reference.

---

## Paper Break Handling

The machine monitors the paper-present condition during automatic production.

If a paper break is detected while the axis is cycling, the spray process is interrupted in a controlled manner.

The machine performs the following sequence:

```text
Paper Break Detected
        ↓
Stop Pump
        ↓
Keep Air Valve Open
        ↓
Stop Axis Cycling
        ↓
Return Axis to Start Position
```

The axis always completes the return to the configured Start Position, even if the paper-present signal becomes valid again during the return movement.

Once the Start Position has been reached:

```text
Paper Present?
   Yes ───────→ Resume Automatic Production
   No  ───────→ Remain in Paper Break Wait State
```

While the machine remains in the Paper Break Wait state, automatic cycling is inhibited until the paper-present condition is restored.

When the condition becomes valid again, automatic production resumes from the Start Position.

---

## General Operating Rules

* Only one operating mode may be active at a time.
* Automatic operation requires a valid axis homing reference.
* Active faults inhibit automatic machine operation.
* A Stop request initiates a controlled stop of the active machine operation.
* Operating mode changes during active operation are completed only after the machine has reached a controlled stopped condition.
* The pump and air valve are controlled automatically during production and may be operated individually in Manual mode.
* The axis reference is invalidated when a new homing sequence is required.
* Machine operation may resume only when the conditions required by the active operating mode are satisfied.
