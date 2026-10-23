# FraudGuard Entity Relationship Diagram

```mermaid
erDiagram
    USERS ||--o{ ANALYSES : creates
    USERS ||--o{ FEEDBACK : submits
    USERS ||--o{ CHANNEL_CONNECTIONS : owns
    ANALYSES ||--o{ FEEDBACK : receives

    USERS {
        uuid id PK
        string email
        string password_hash
        string role
        datetime created_at
    }

    ANALYSES {
        uuid id PK
        uuid user_id FK
        string source
        string external_msg_id UK
        string classification
        string risk_level
        float confidence
        json indicators
        string message_hash
        string redacted_preview
        int processing_time_ms
        string model_version
        datetime created_at
    }

    FEEDBACK {
        uuid id PK
        uuid analysis_id FK
        uuid user_id FK
        string corrected_label
        string comment
        datetime created_at
    }

    CHANNEL_CONNECTIONS {
        uuid id PK
        uuid user_id FK
        string channel_type
        string external_id
        string status
        datetime created_at
    }
```

`external_msg_id` is filled for SMS and Telegram messages and is unique, so a repeated delivery of the same `message_id` does not create a second analysis. Full message text is not stored, only `message_hash` and an optional short `redacted_preview`.
