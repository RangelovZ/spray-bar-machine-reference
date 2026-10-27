# Software Architecture

## Purpose

This document defines the software architecture used to implement the functional behaviour defined in the Machine Specification.

It describes how the application is organized, how information flows through the system, and how software components interact.

The architecture is intended to be reusable across different industrial machine applications.

---

## Architectural Principles

The architecture follows the principles defined in [Design Principles](design_principles.md).

Its foundation is based on the following concepts:

* Separation of Concerns
* One-Way Data Flow
* Single Source of Truth
* Explicit Command/Status Communication
* Independent Software Components

---

## Architecture Overview

The application is organized into four primary building blocks:

```text
Application
│
├── Interfaces
├── Managers
├── Modules
└── Data
```

Each building block has a clearly defined responsibility.

**Interfaces** isolate the application from external systems and hardware.

**Managers** implement application-level decisions and coordinate machine behaviour.

**Modules** execute subsystem-specific functionality and are responsible for their state, diagnostics, and fault handling.

**Data** defines the information exchanged between application components.

The architecture separates machine-level decision making from subsystem execution.

A typical interaction follows this direction:

```text
External System / Hardware
        ↓
     Interfaces
        ↓
      Managers
        ↓
      Commands
        ↓
      Modules
        ↓
       Status
        ↓
     Interfaces
```

This structure keeps component responsibilities explicit and minimizes direct dependencies between application components.

---

## Application Structure

### Interfaces

Interfaces isolate the application from external systems and hardware.

They handle communication with the HMI, physical I/O, drives, and other external systems without containing machine-sequence logic.

---

### Managers

Managers implement application-level decisions and coordinate machine behaviour.

They evaluate commands, operating conditions, and module status, then generate requests for subsystem modules.

Managers do not execute hardware actions directly.

In this reference implementation, the Sequence Manager coordinates the machine workflow while subsystem execution remains inside the modules.

---

### Modules

Modules implement subsystem-specific functionality.

Each module is responsible for its subsystem behaviour, state, diagnostics, and fault handling, and processes commands received from managers.

Typical modules in this reference implementation include:

* Axis Module
* Pump Module
* Valve Module

---

### Data

Data defines the information exchanged between application components.

```text
Data
│
├── Commands
├── Parameters
├── Status
└── Diagnostics
```

Commands request actions from application components.

Parameters define configurable application behaviour.

Status represents the current state of the application and its subsystems.

Diagnostics represents faults, warnings, and service-related information.

Each data item has a clearly defined owner.

---

## Execution Model

The application follows a deterministic execution sequence during every application cycle.

```text
Read Interfaces
        ↓
Update Managers
        ↓
Update Modules
        ↓
Write Interfaces
```

The execution order establishes a predictable and traceable flow of information throughout the application.

---

## Interaction Model

Application components communicate through explicit command and status interfaces.

```text
Manager
    ↓
Command
    ↓
Module
    ↓
Status
```

Commands represent requested actions, while status represents the current condition of the component.

Modules remain responsible for deciding how requests are executed and for updating their own state.

---

## Architectural Rules

The following rules apply throughout the application:

* Application logic shall not access hardware directly.
* External systems shall communicate through interfaces.
* Managers and modules communicate through explicit command and status data.
* Managers shall not modify module state directly.
* Modules own their functionality, state, and fault handling.
* Information shall follow the defined one-way execution flow.
* Every piece of application data shall have a clearly defined owner.
