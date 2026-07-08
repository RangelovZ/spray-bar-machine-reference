# ADR-003: Application Component Execution Model

## Purpose

This ADR defines how application components are executed during each application cycle.

Its purpose is to establish a consistent execution model for managers, modules, and future application components.

---

## Context

Application components perform different responsibilities, but they should follow a consistent execution structure.

Without a common execution model, each component may organize its logic differently, making the application harder to read, debug, test, and maintain.

A simple and repeatable execution model is required.

---

## Decision

Each application component shall follow a three-phase execution model:

```text
Read Inputs
        ↓
Execute
        ↓
Update Outputs
```

`Read Inputs` collects commands, parameters, status inputs, or other data required by the component.

`Execute` performs the component logic, including decisions, state machines, timers, and internal processing.

`Update Outputs` writes component status, diagnostics, or generated commands.

Internal implementation details remain private to each component.

---

## Rationale

A common execution model makes components easier to understand and maintain.

The same structure can be applied to managers, modules, and future application components.

The model is simple enough for small components while still supporting complex state machines.

It also aligns with the application-level execution flow:

```text
Read Interfaces
        ↓
Update Managers
        ↓
Update Modules
        ↓
Write Interfaces
```

---

## Consequences

Application components follow a predictable internal structure.

Debugging and code review become easier because each component follows the same execution pattern.

Components remain free to implement their internal logic according to their responsibility.

The model introduces a small amount of structural discipline, but avoids unnecessary implementation complexity.
