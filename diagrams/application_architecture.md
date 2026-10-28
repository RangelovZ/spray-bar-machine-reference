# Application Architecture

This diagram shows the logical structure of the application and the primary direction of data flow between its components.

```mermaid
flowchart TD
    EXT["External Systems / Hardware<br/>HMI · I/O · Drive · Communication"]

    IF["Interfaces"]
    MGR["Managers<br/>Sequence Manager"]
    DATA["Application Data<br/>Commands · Parameters · Status · Diagnostics"]

    AXIS["Axis Module"]
    PUMP["Pump Module"]
    VALVE["Valve Module"]

    EXT --> IF
    IF --> DATA

    DATA --> MGR
    MGR --> DATA

    DATA --> AXIS
    DATA --> PUMP
    DATA --> VALVE

    AXIS --> DATA
    PUMP --> DATA
    VALVE --> DATA

    DATA --> IF
    IF --> EXT
```

The diagram represents logical responsibilities and data flow rather than direct component-to-component calls.

Managers make application-level decisions, modules execute subsystem behaviour, and interfaces isolate the application from external systems and hardware.
