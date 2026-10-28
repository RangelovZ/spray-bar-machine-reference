# Homing Responsibility

This diagram shows the separation of responsibility between the Sequence Manager and the Axis Module during homing.

```mermaid
flowchart TD
    SM1["Sequence Manager<br/>Requests Homing"] --> AXIS["Axis Module"]

    AXIS --> STOPPER["Move Towards Mechanical Stop"]
    STOPPER --> DETECT["Detect Mechanical Stop"]
    DETECT --> HALT["Stop Axis Motion"]
    HALT --> ZERO["Set Axis Position to Zero"]
    ZERO --> RELEASE["Release Mechanical Stop"]
    RELEASE --> DONE["Homing Completed"]

    DONE --> SM2["Sequence Manager<br/>Selects Next Action"]

    SM2 --> AUTO["Automatic<br/>Move to Start Position"]
    SM2 --> MAN["Manual<br/>Remain Available"]
    SM2 --> MAINT["Maintenance<br/>Move to Maintenance Position"]
```

The Axis Module owns the homing procedure itself.

The Sequence Manager owns the machine-level decision that follows successful homing.
