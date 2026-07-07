# Architecture Decision Records

This folder contains Architecture Decision Records (ADRs) for the project.

An ADR documents a significant architectural decision, including its context, the selected solution, the rationale behind the decision, and its consequences.

ADRs are used only for decisions that:

* have a significant impact on the application architecture,
* involve trade-offs between multiple valid options,
* are difficult or expensive to change later,
* help future readers understand why the system is designed in a particular way.

ADRs are **not** used for implementation details, parameter values, or temporary development notes.

---

## ADR Template

Each ADR should follow the structure below.

```text
# ADR-XXX: Decision Title

## Purpose

Briefly describe why this ADR exists.

## Context

Describe the problem, background, constraints, and available options.

## Decision

Describe the selected architectural decision.

## Rationale

Explain why this solution was chosen over the alternatives.

## Consequences

Describe the expected impact, including benefits, trade-offs, and limitations.
```

---

## Naming Convention

ADR files shall use the following naming convention:

```text
ADR-001-application-responsibilities.md
ADR-002-command-status-communication.md
ADR-003-one-way-data-flow.md
```

Each ADR shall document a single architectural decision.
