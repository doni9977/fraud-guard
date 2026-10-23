# FraudGuard Use Cases

Use case IDs follow the table in `docs/architecture.md`. Actors are drawn as «actor» boxes, use cases as ovals, associations as plain lines.

```mermaid
---
config:
  layout: elk
---
flowchart LR
    user["«actor»<br/><b>End user</b>"]

    subgraph system["FraudGuard platform"]
        uc1(["UC-01 Submit message for analysis"])
        uc2(["UC-02 View result with explanation"])
        uc7(["UC-07 Receive high-risk warning"])
        uc3(["UC-03 View analysis history"])
        uc4(["UC-04 Provide feedback on classification"])
        uc10(["UC-10 Manage connected sources"])
        uc6(["UC-06 Ingest message from SMS or Telegram"])
        analyze(["Analyze message with ML model"])
        uc5(["UC-05 Access API documentation"])
        uc8(["UC-08 View analytics dashboard"])
        uc9(["UC-09 Monitor system health"])
    end

    admin["«actor»<br/><b>Administrator / analyst</b>"]
    tg["«actor»<br/><b>Telegram bot</b>"]
    sms["«actor»<br/><b>SMS companion app</b><br/>planned"]

    user --- uc1
    user --- uc2
    user --- uc3
    user --- uc4
    user --- uc10
    uc6 --- tg
    uc6 -.- sms

    uc1 -.->|"«include»"| analyze
    uc6 -.->|"«include»"| analyze
    uc7 -.->|"«extend»<br/>P(fraud) ≥ 0.70"| uc1

    uc3 --- admin
    uc4 --- admin
    uc5 --- admin
    uc8 --- admin
    uc9 --- admin

    classDef actor fill:#ffffff,stroke:#374151,color:#111827
    classDef uc fill:#eff6ff,stroke:#1d4ed8,color:#111827
    classDef ucadmin fill:#ecfdf5,stroke:#047857,color:#111827
    classDef ucext fill:#fff7ed,stroke:#c2410c,color:#111827
    classDef internal fill:#f9fafb,stroke:#6b7280,color:#111827
    class user,admin,tg,sms actor
    class uc1,uc2,uc3,uc4,uc7,uc10 uc
    class uc5,uc8,uc9 ucadmin
    class uc6 ucext
    class analyze internal
```

UC-06 is an extension of the core web MVP: Telegram bot first, SMS application as a prototype.
