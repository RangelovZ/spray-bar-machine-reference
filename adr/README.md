# Architecture Decision Records

This folder contains Architecture Decision Records (ADRs) for the project.

An ADR documents an important architectural decision, including the context, selected decision, rationale, and consequences.

ADRs are used only for decisions that:

* have a significant impact on the application architecture,
* involve trade-offs between multiple valid options,
* are difficult or expensive to change later,
* help future readers understand why the system is designed in a certain way.

ADRs are not used for minor implementation details, parameter values, or temporary development notes.

## ADR Format

Each ADR should follow this structure:

```text
# ADR-XXX: Decision Title

## Status

Proposed | Accepted | Superseded

## Context

Describe the problem, background, and constraints.

## Decision

Describe the selected decision.

## Rationale

Explain why this decision was selected.

## Consequences

Describe the benefits, trade-offs, and limitations of this decision.
```

## Naming Convention

ADR files should use the following format:

```text
ADR-001-decision-title.md
ADR-002-decision-title.md
ADR-003-decision-title.md
```

Example:

```text
ADR-001-application-responsibilities.md
```
