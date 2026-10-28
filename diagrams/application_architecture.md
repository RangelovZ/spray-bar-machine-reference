# Application Architecture

This diagram shows the application composition and the execution relationship between the main software components.

```mermaid
flowchart TB
    EXT["External Systems / Hardware<br/>HMI · I/O · Drive · Communication"]

    subgraph APP["PROGRAM Application"]
        direction TB

        IF["Interfaces"]
        MGR["Sequence Manager"]

        subgraph MOD[" "]
            direction LR
            MODTITLE["Modules"]
            VALVE["Valve Module"]
            AXIS["Axis Module"]
            PUMP["Pump Module"]
        end

        style MODTITLE fill:transparent,stroke:transparent

        DATA["Shared Application Data<br/>Commands · Parameters · Status · Diagnostics"]

        IF --> MGR
        MGR --> AXIS
        MGR --> PUMP
        MGR --> VALVE

        IF --- DATA
        MGR --- DATA
        AXIS --- DATA
        PUMP --- DATA
        VALVE --- DATA
    end

    EXT <--> IF
```

`Application` acts as the composition root. It owns the shared application data, instantiates the main components, and defines their execution order.

The primary execution flow is:

```text
Read Interfaces
        ↓
Sequence Manager
        ↓
Axis / Pump / Valve Modules
        ↓
Write Interfaces
```

Commands, parameters, status, and diagnostics are shared application data used to exchange information between these components. They are not separate executable components.
