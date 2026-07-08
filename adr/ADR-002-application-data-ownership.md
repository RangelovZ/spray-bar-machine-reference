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

`FB_Application` owns all application data.

Application components exchange data exclusively through the application data owned by `FB_Application`.

Managers write commands.

Modules read commands, execute their functionality, and update their status and diagnostics.

Components shall not reference or call each other directly.

---

## Rationale

Keeping application data in a single location establishes a clear ownership model and supports the Single Source of Truth principle.

Managers remain responsible for making decisions.

Modules remain responsible for executing subsystem functionality.

`FB_Application` becomes the composition root that connects components without introducing direct dependencies between them.

This architecture keeps application components independent and simplifies testing, maintenance, and future extension.

---

## Consequences

Application components remain loosely coupled.

Data exchange between components becomes explicit and easy to trace.

Application data is centralized while execution logic remains distributed across managers and modules.

The architecture introduces additional data structures, but significantly improves readability, maintainability, and scalability.
