# Machine Specification

## Purpose

This document defines the functional behavior of the Spray Bar machine.

It describes what the machine does from an operational perspective without specifying implementation details.

The specification is independent of the software architecture, PLC platform, HMI, or communication technology.

---

## Machine Overview

The Spray Bar machine performs a continuous back-and-forth movement over the configured working area while applying a fluid-air mixture through a spray system.

The machine consists of:

* One horizontal driven axis
* One pump
* One air valve

The axis position is configurable through machine parameters.

The pump and air valve operate only during automatic production.

---

## Operating Modes

The machine supports three operating modes:

* Automatic
* Manual
* Maintenance

Only one operating mode may be active at any time.

Operating mode changes are allowed only while the machine is stopped.

### Automatic

Automatic mode is intended for standard production.

The machine continuously moves between the configured start and final positions while the spray system is active.

### Manual

Manual mode allows the operator to perform individual machine functions for setup and adjustment.

Automatic sequences are disabled.

### Maintenance

Maintenance mode is intended for servicing, diagnostics, and setup activities.

Its available functions may differ from Manual mode depending on project requirements.

---

## Machine States

The machine operates in one of the following states:

* Stopped
* Running
* Fault

### Stopped

No machine operation is active.

### Running

The machine performs the behavior associated with the selected operating mode.

### Fault

Machine operation is inhibited until the active fault has been cleared and acknowledged.

---

## Operator Commands

The machine provides the following operator commands:

* Start
* Stop
* Homing Request

Stop shall have immediate priority over all other commands.

---

## Automatic Operation

Automatic operation requires:

* Successful homing
* No active faults

The automatic sequence performs the following behaviour:

Already at Start Position

↓

Activate Pump

↓

Open Air Valve

↓

Move continuously between:

Start Position ↔ Final Position

↓

Stop Command or Fault

↓

Stop Axis

↓

Close Air Valve

↓

Stop Pump

The sequence continues until a Stop command or an active fault occurs.

---

## Homing

The homing sequence establishes the machine reference position.

The sequence performs the following operations:

```text
Move towards mechanical stop

↓

Detect mechanical stop

↓

Set reference position

↓

Move to configured Start Position

↓

Homing completed
```

The machine is ready for automatic operation only after the homing sequence has completed successfully.

---

## General Operating Rules

* Only one operating mode may be active at a time.
* Automatic operation requires successful homing.
* Pump and air valve operate only during automatic production.
* Stop immediately terminates machine operation.
* Active faults inhibit machine operation.
