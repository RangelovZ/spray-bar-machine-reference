# Automatic Operation

This diagram summarizes the normal automatic production sequence and the controlled stop path.

```mermaid
flowchart TD
    START["Start Command"] --> CHECK["Check Operating Conditions"]
    CHECK --> HOMED{"Axis Homed?"}

    HOMED -- "No" --> HOME["Perform Homing"]
    HOME --> MOVE["Move to Start Position"]

    HOMED -- "Yes" --> MOVE

    MOVE --> PUMP["Start Pump"]
    PUMP --> AIR["Open Air Valve"]
    AIR --> CYCLE["Start Cycling"]
    CYCLE --> RUN["Start Position ↔ Final Position"]

    RUN --> STOP{"Stop / Mode Change /<br/>Process Interruption / Fault"}
    STOP --> AXSTOP["Stop Axis Motion"]
    AXSTOP --> PUMPOFF["Stop Pump"]
    PUMPOFF --> BLOW["Keep Air Valve Open"]
    BLOW --> DELAY["Configured Air-Blow Delay"]
    DELAY --> AIRCLOSE["Close Air Valve"]
    AIRCLOSE --> END["Stopped / Follow-up Action"]
```

The normal production path starts only after the required operating conditions are satisfied and the axis has a valid homing reference.

The final action after a controlled stop depends on the condition that initiated the stop.
