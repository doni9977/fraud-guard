# FraudGuard System Context

The platform is shown as one system. Only people and external systems around it are shown; internal parts are in the component diagram.

```mermaid
flowchart LR
    user["«person»<br/><b>End user</b><br/>received a suspicious message"]
    admin["«person»<br/><b>Administrator / analyst</b><br/>reviews feedback and analytics"]
    platform["<b>FraudGuard</b><br/>intelligent web platform<br/><br/>analyses messages with its own ML model,<br/>returns risk level, explanation and warning"]
    telegram["«external system»<br/><b>Telegram Bot API</b>"]
    sms["«external system»<br/><b>SMS companion application</b><br/>planned"]
    hosting["«external system»<br/><b>Cloud hosting</b><br/>Vercel, Render, managed PostgreSQL"]

    user -->|"submits message, gets result and warning<br/>HTTPS"| platform
    platform ---|"views analytics, reviews feedback<br/>HTTPS"| admin
    user -->|"forwards message to the bot"| telegram
    telegram <-->|"webhook update / reply with warning"| platform
    sms -.->|"normalized payload, API key"| platform
    platform -.->|"deployed on"| hosting

    classDef system fill:#dbeafe,stroke:#1d4ed8,stroke-width:2px,color:#111827
    classDef person fill:#ffffff,stroke:#374151,color:#111827
    classDef external fill:#f3f4f6,stroke:#6b7280,color:#111827
    classDef planned fill:#f3f4f6,stroke:#6b7280,stroke-dasharray:5 4,color:#111827
    class platform system
    class user,admin person
    class telegram,hosting external
    class sms planned
```

Dashed elements are planned extensions, not part of the core web MVP.
