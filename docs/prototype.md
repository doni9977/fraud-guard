# UX Prototype

## 1. Overview
This document describes the user interface design and key user interaction scenarios for the Intelligent Web Platform for Fraud Message Detection.

## 2. Design Principles
- **Simplicity**: Minimal steps to analyze a message
- **Clarity**: Clear, unambiguous presentation of results
- **Accessibility**: WCAG 2.1 AA compliance, responsive design
- **Trust**: Transparent explanation of ML classification

## 3. Key User Scenarios

The web workflow is the guaranteed core scenario. External integrations are planned extensions: the same result is produced whether a message arrives from the web form, the SMS companion application, or the Telegram bot.

### Scenario 1: Analyze a Message
1. User opens the platform homepage
2. User pastes a suspicious message into the text input field
3. User clicks the "Analyze" button
4. System displays a loading indicator
5. System shows the result: classification (Safe / Suspicious / Fraudulent), confidence score, and key indicators

### Scenario 2: View Analysis History
1. User navigates to the "History" page
2. System displays a list of previous analyses with date, snippet, and classification
3. User clicks on an entry to view full details

### Scenario 3: Report Incorrect Classification
1. After viewing a result, user clicks "Report Incorrect"
2. User selects the correct classification
3. System stores the feedback for future model improvement

### Scenario 4: Automatic Analysis from a Connected Channel
1. User explicitly sends or permits a message through the SMS companion application or Telegram bot
2. The adapter submits the message and source metadata to the platform ingestion API
3. The platform analyzes the message using the same ML pipeline as web submissions
4. The platform displays the result in the web dashboard
5. For a high-risk result, the user receives a warning with reasons and safety advice

If a real adapter cannot be completed because of platform permissions or schedule constraints, the same flow is demonstrated with a test adapter using the documented ingestion contract.

## 4. Wireframes

### 4.1 Homepage / Message Input
```
┌────────────────────────────────────────────────┐
│  🛡️ Fraud Message Detector              [■] │
├────────────────────────────────────────────────┤
│                                                │
│  Check if a message is fraudulent              │
│                                                │
│  ┌────────────────────────────────────────┐  │
│  │ Paste your suspicious message here...  │  │
│  │                                        │  │
│  │                                        │  │
│  │                                        │  │
│  └────────────────────────────────────────┘  │
│                                                │
│         [ 🔍 Analyze Message ]                  │
│                                                │
└────────────────────────────────────────────────┘
```

### 4.2 Analysis Result
```
┌────────────────────────────────────────────────┐
│  Analysis Result                               │
├────────────────────────────────────────────────┤
│                                                │
│  ⚠️  SUSPICIOUS  (Confidence: 87%)               │
│  ────────────────────────────────              │
│                                                │
│  Key Indicators:                               │
│  • Urgency language detected ("act now")        │
│  • Request for personal information             │
│  • Suspicious URL pattern                       │
│  • Grammar inconsistencies                      │
│                                                │
│  [ Analyze Another ]  [ Report Incorrect ]     │
│                                                │
└────────────────────────────────────────────────┘
```

### 4.3 History Page
```
┌────────────────────────────────────────────────┐
│  Analysis History                              │
├────────────────────────────────────────────────┤
│  Date        Message Preview     Result        │
│  ──────────  ──────────────  ───────────  │
│  Oct 19      "You won a pri..."  🔴 Fraud     │
│  Oct 18      "Your package..."   🟡 Suspicious│
│  Oct 18      "Meeting at 3..."   🟢 Safe      │
│  Oct 17      "Click here to..."  🔴 Fraud     │
│                                                │
└────────────────────────────────────────────────┘
```

## 5. Figma Prototype
> **Link**: https://text-wispy-10186308.figma.site/

Interactive prototype made in Figma. The main screen covers the key scenarios:
- **Overview:** number of analysed messages, high-risk messages, average confidence and connected sources
- **Quick analysis (UC-01):** message field with a 10,000 character limit, language selection (auto-detect, English, Russian, Kazakh) and the *Analyze message* button
- **Recent analyses (UC-02, UC-03):** latest results with source, short preview, risk label and confidence, and a link to the full history
- **Safety reminder:** short advice shown to the user

The numbers on the screens are demo values for the layout, not real statistics or model results.

### 5.1 Connected Sources
```
┌────────────────────────────────────────────────┐
│ Connected Sources                              │
├────────────────────────────────────────────────┤
│ SMS companion application          Planned     │
│ Telegram bot                       Planned     │
│ Automatic high-risk warnings       Enabled     │
└────────────────────────────────────────────────┘
```

### 5.2 High-Risk Warning
```
┌────────────────────────────────────────────────┐
│ ⚠️ HIGH-RISK MESSAGE DETECTED                 │
│ Source: Telegram bot / SMS                    │
│ Risk: 94% · Financial fraud                   │
│                                               │
│ • Urgent request for payment                  │
│ • Suspicious link detected                    │
│ • Sender impersonation indicators             │
│                                               │
│ Do not open links or share verification codes.│
│                         [View Analysis]       │
└────────────────────────────────────────────────┘
```

## 6. API Interface (for programmatic access)

The examples below show the planned contract. The response values are illustrative, not model output.

### Analyze a message from the web form
```json
POST /api/v1/analyze
Content-Type: application/json

{
  "message": "Congratulations! You've won a $1000 gift card. Click here to claim: http://suspicious-link.example",
  "language": "en",
  "source": "web"
}
```

```json
{
  "id": "a1b2c3d4",
  "classification": "fraudulent",
  "confidence": 0.94,
  "risk_level": "high",
  "indicators": [
    {"type": "reward_or_prize", "title": "Prize or reward", "evidence": "$1000 gift card", "severity": "high"},
    {"type": "suspicious_url", "title": "Suspicious link", "evidence": "http://suspicious-link.example", "severity": "high"},
    {"type": "urgency", "title": "Urgency language", "evidence": "Click here to claim", "severity": "medium"}
  ],
  "warning": {
    "show": true,
    "title": "High-risk message detected",
    "text": "Do not open links or share personal information."
  },
  "source": "web",
  "model_version": "baseline-0.1",
  "processing_time_ms": 120
}
```

### Message from an external channel
```json
POST /api/v1/messages/ingest
Content-Type: application/json

{
  "message_id": "tg-000123",
  "source": "telegram",
  "text": "Your account will be blocked today. Send the code from SMS.",
  "language": "en",
  "received_at": "2026-10-22T12:00:00Z"
}
```

The response has the same fields as `/api/v1/analyze`. Sending the same `source` and `message_id` again returns the existing analysis instead of creating a new one.
