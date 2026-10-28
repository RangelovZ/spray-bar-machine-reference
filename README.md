# Spray Bar Machine Reference

Reference architecture for designing modular industrial machine software using IEC 61131-3 Structured Text.

## Project Purpose

This repository presents a reference architecture for a modular industrial spray bar machine implemented using IEC 61131-3 Structured Text.

The goal is to demonstrate how functional requirements can be translated into a maintainable PLC software architecture based on clear responsibilities, explicit data ownership, deterministic execution, and reusable software components.

Although the reference is based on a real industrial machine, the concepts are intentionally generalized so they can be applied to other machine-control applications.

## Design Philosophy

The project is built around a small set of engineering principles:

* Reliability Through Simplicity
* Separation of Concerns
* Single Source of Truth
* One-Way Data Flow
* Explicit State Management

These principles guide every architectural and implementation decision throughout the project.

A more detailed description is available in [Design Principles](docs/design_principles.md).

## Architecture Overview

The application is organized into four main building blocks:

```text
Application
│
├── Interfaces
├── Managers
├── Modules
└── Data
```

Each building block has a clearly defined responsibility.

Interfaces isolate the application from external systems such as the HMI, hardware I/O, drives, and communication interfaces.

Managers implement application-level decisions and coordinate machine behaviour.

Modules encapsulate subsystem functionality and own their internal state, diagnostics, and fault handling.

Data defines the information exchanged between application components and is organized into commands, parameters, status, and diagnostics.

```text
Data
│
├── Commands
├── Parameters
├── Status
└── Diagnostics
```

The architecture keeps application-level decisions separate from subsystem execution and maintains explicit ownership of application data.

## Repository Structure

```text
.
├── README.md
│
├── docs/
│   ├── machine_specification.md
│   ├── software_architecture.md
│   ├── design_principles.md
│   └── coding_standard.md
│
├── adr/
│   ├── README.md
│   ├── ADR-001-application-composition.md
│   ├── ADR-002-application-data-ownership.md
│   ├── ADR-003-application-component-execution-model.md
│   └── ADR-004-axis-homing-responsibility.md
│
├── diagrams/
│   └── README.md
│
├── examples/
│   └── README.md
│
└── images/
    └── README.md
```

The repository is organized around documentation rather than production source code.

* **docs/** – Functional specification, software architecture, design principles, and coding conventions.
* **adr/** – Architecture Decision Records documenting significant design decisions and their rationale.
* **diagrams/** – Architecture, state-machine, and sequence diagrams used to visualize the design.
* **examples/** – Simplified IEC 61131-3 Structured Text examples illustrating selected architectural patterns.
* **images/** – Supporting images used by the documentation.

## Intentionally Omitted

This repository does **not** include:

* Production PLC source code
* Customer-specific functionality
* Electrical schematics
* HMI project files
* Drive configuration
* Commissioning parameters
* Network configuration, credentials, or access settings
* Safety-rated control implementation

The repository focuses on software architecture, engineering principles, and generalized design decisions that can be reused across industrial machine projects.
