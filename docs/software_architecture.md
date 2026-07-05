# Software Architecture

## Purpose

This document defines the software architecture used throughout the project.

It describes how the application is organized, how information flows through the system, and how software responsibilities are separated.

The architecture is intended to be reusable across different industrial machine applications.

---

## Architectural Principles

The architecture follows the engineering principles defined in `engineering_principles.md`.

The software is designed around the following concepts:

* Separation of Concerns
* One-Way Data Flow
* Single Source of Truth
* Explicit Command/Status Communication
* Independent Software Components

---

## Application Structure

The application is organized into five logical building blocks.

```text
Application
│
├── Interfaces
├── Managers
├── Modules
├── Parameters
└── Status
```

Each building block has a clearly defined responsibility.

### Interfaces

Interfaces isolate the application from external systems.

They translate external information into application data and expose application status to the outside world.

Typical interface categories include:

* Operator Interface
* Hardware Interface
* Drive Interface
* Communication Interface

---

### Managers

Managers are responsible for application behaviour.

They evaluate the current application state, apply application rules, and generate requests for subsystem modules.

Managers do not execute physical actions directly.

---

### Modules

Modules encapsulate reusable subsystem functionality.

Each module owns its internal behaviour, state, diagnostics, and fault handling.

A module is the only component allowed to modify its own internal state.

---

### Parameters

Parameters define configurable application behaviour.

Application logic shall use parameters instead of hardcoded values whenever practical.

---

### Status

Status represents the current state of the application and its subsystems.

Status information is owned by the corresponding application component and may be consumed by other components or external interfaces.

---

## Application Execution Flow

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

Commands represent requests.

Status represents the actual state of a component.

Each component determines how incoming requests are processed and updates its own status accordingly.

---

## Ownership Model

Each application component owns its own internal state.

Components may request actions from other components, but they shall never modify another component's internal state directly.

This architecture establishes a single source of truth for every piece of application data.

---

## Architectural Rules

The following rules apply throughout the application:

* Application logic shall not access hardware directly.
* External systems shall communicate only through interfaces.
* Managers communicate with modules using explicit command/status interfaces.
* Components shall not directly modify the internal state of other components.
* Information shall flow in one direction during each application cycle.
* Every piece of application data shall have a single owner.
