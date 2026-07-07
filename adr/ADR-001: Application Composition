# ADR-001: Application Composition

## Purpose

This ADR defines how the application is organized into logical building blocks.

Its purpose is to establish a consistent application structure that supports readability, maintainability, scalability, and component reuse.

---

## Context

Industrial PLC applications often evolve into large monolithic programs where application logic, hardware interaction, subsystem functionality, and application data become tightly coupled.

Such architectures are difficult to understand, extend, test, and maintain.

A consistent application structure is required to clearly separate responsibilities and provide a common foundation for future machine projects.

---

## Decision

The application shall be organized into four primary building blocks:

* Interfaces
* Managers
* Modules
* Data

Each building block has a clearly defined responsibility.

Interfaces isolate the application from external systems.

Managers implement application-level decisions and coordinate application execution.

Modules implement subsystem functionality and own their internal state.

Data represents the information used and exchanged by application components.

---

## Rationale

Separating the application into logical building blocks improves software organization and reduces coupling between components.

Interfaces isolate hardware-specific implementation details.

Managers coordinate application execution without implementing subsystem functionality.

Modules encapsulate reusable subsystem logic and maintain ownership of their internal state.

Data provides a consistent information model shared across the application while maintaining a single source of truth.

---

## Consequences

The application follows a predictable and consistent structure across all machine projects.

Responsibilities remain clearly separated, making the software easier to understand, maintain, and extend.

The architecture encourages reusable modules and minimizes dependencies between application components.

The approach introduces additional software components compared to a monolithic implementation, but the improvement in maintainability and scalability outweighs the additional implementation effort.
