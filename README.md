# Spray Bar Machine Reference

Reference architecture for designing modular industrial machine software using IEC 61131-3 Structured Text.

## Project Purpose

This repository documents the architecture, engineering decisions, and development process behind a modular spray bar machine implemented using IEC 61131-3 Structured Text.

Its purpose is to capture the design process from requirements through software architecture to implementation, while serving as a reusable reference for developing maintainable industrial machine software.

Although based on a commercial project, the repository intentionally focuses on generalized architecture, engineering principles, and reusable software design rather than customer-specific implementation.

## Design Philosophy

The project is built around a small set of engineering principles:

* Reliability Through Simplicity
* Separation of Concerns
* Single Source of Truth
* One-Way Data Flow
* Explicit State Management

These principles guide every architectural and implementation decision throughout the project.

## Architecture Overview

The application is organized into five main building blocks:

```text
Application
│
├── Interfaces
├── Managers
├── Modules
├── Parameters
└── Status
```

The architecture clearly separates application logic from machine subsystems and external interfaces.

Managers are responsible for application-level decisions and workflow.

Modules encapsulate subsystem behavior and own their internal state.

Interfaces isolate the application from the HMI, hardware, and external communication.

## Repository Structure

```text
docs/
    machine_specification.md
    software_architecture.md
    engineering_principles.md
    coding_standard.md
    adr/

examples/
diagrams/
images/
```

* **docs** – Project documentation and architecture decision records.
* **examples** – Simplified reference implementations and code snippets.
* **diagrams** – Architecture and design diagrams.
* **images** – Images used throughout the documentation.

## Intentionally Omitted

This repository does **not** include:

* Production PLC source code
* Customer-specific functionality
* Electrical schematics
* HMI project files
* Drive configuration
* Commissioning parameters

The goal is to share engineering practices and software architecture without exposing proprietary implementation details.

## Roadmap

### Current

* ✔ Machine Specification
* ✔ Software Architecture
* ✔ Engineering Principles
* ✔ Coding Standard

### Planned

* Application Data Model
* State Machine Design
* Reference Structured Text Skeletons
* Architecture Diagrams
* Example Design Patterns
* Architecture Decision Records (ADRs)
