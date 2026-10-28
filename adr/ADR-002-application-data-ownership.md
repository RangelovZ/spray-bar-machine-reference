# ADR-002: Application Data Ownership

## Purpose

This ADR defines how application data is owned and shared between application components.

Its purpose is to establish a consistent data exchange model while maintaining low coupling and a single source of truth.

---

## Context

Application components need to exchange commands, parameters, status, and diagnostics.

Direct data exchange through component references introduces unnecessary dependencies and tightly couples the application architecture.

A consistent ownership model is required to keep components independent while allowing them to collaborate.

---

## Decision

`Application` owns the shared application data and acts as the composition root.

Application components exchange data through the structures owned by `Application`.

Managers generate commands.

Modules process commands and update their corresponding status and diagnostics.

Components shall not directly modify the internal state of other components.

---

## Rationale

Keeping shared application data in one location establishes a clear ownership model and supports the Single Source of Truth principle.

Managers remain responsible for application-level decisions.

Modules remain responsible for subsystem execution and status reporting.

`Application` connects the components and defines their execution order without introducing unnecessary direct dependencies.

This keeps application components loosely coupled and makes data flow easier to understand, debug, and maintain.

---

## Consequences

Application components remain loosely coupled.

Data exchange between components becomes explicit and easy to trace.

Application data is centralized while execution logic remains distributed across managers and modules.

The architecture introduces additional data structures, but significantly improves readability, maintainability, and scalability.
