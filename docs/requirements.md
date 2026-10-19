# Requirements Specification

## 1. Introduction

### 1.1 Purpose of the Platform
The **Intelligent Web Platform for Analysis, Detection, and Prevention of Fraudulent Messages Using Machine Learning Methods** is an academic and applied software engineering project designed to address the increasing threat of digital communication fraud. With phishing, smishing (SMS phishing), and social engineering schemes escalating across instant messengers and electronic mail, individuals and organizations face critical security vulnerabilities. 

The primary purpose of the platform is to provide an accessible, explainable, and high-throughput automated classification system. It accepts text through the web interface and through a normalized ingestion API intended for an SMS companion application and a Telegram bot. The platform evaluates risk levels (Safe, Suspicious, Fraudulent), calculates confidence metrics, visualizes indicators, and provides an actionable warning for high-risk messages. External adapters are an extension of the core web MVP and may be delivered as prototypes subject to platform access and project time.

### 1.2 Scope of this Document
This Software Requirements Specification (SRS) defines the complete baseline of functional and non-functional requirements for the platform. It establishes the technical criteria against which the system will be designed, implemented, evaluated, and verified. The scope encompasses:
- End-user web interactions and submission interfaces.
- Machine learning inference and model explainability pipelines.
- RESTful API architectures and security protocols.
- Performance, availability, scalability, privacy, and maintainability standards.
- Verification strategies and issue traceability.

### 1.3 Target Audience and Document Conventions
This document is authored for academic supervisors, software engineers, machine learning specialists, quality assurance personnel, and system evaluators. In accordance with standard systems engineering conventions:
- **Must** denotes mandatory, non-negotiable capabilities essential for the core minimal viable product (MVP).
- **Should** denotes high-priority features that significantly enrich platform utility and should be included unless strictly constrained by project timelines.
- **Could** denotes desirable, complementary enhancements that may be incorporated if developmental resources permit.

---

## 2. Functional Requirements

The functional requirements define the specific operational behaviors, automated services, user interactions, and data transformations provided by the platform.

### 2.1 Functional Requirements Matrix

| ID | Requirement | Description | Priority | Acceptance Criteria |
| :--- | :--- | :--- | :---: | :--- |
| **FR-1** | Message Submission | User can paste or type a text message and submit it for analysis via the user interface. | **Must** | 1. Text input field accepts between 1 and 10,000 characters.<br>2. Submit button triggers client-side validation and backend dispatch.<br>3. Input sanitization prevents injection attacks while preserving linguistic markers. |
| **FR-2** | ML-Based Classification | System classifies submitted message using a trained machine learning model into risk categories. | **Must** | 1. Classifies message into one of three predefined classes: *Safe*, *Suspicious*, or *Fraudulent*.<br>2. Computes and returns a confidence score normalized between 0% and 100%.<br>3. End-to-end inference execution completes within 3 seconds. |
| **FR-3** | Result Explanation | System provides an interpretable explanation detailing why a message was assigned a specific classification. | **Must** | 1. Highlights at least 3 key indicators or features (e.g., urgency cues, credential harvesting links, manipulative linguistic patterns).<br>2. Presents human-readable rationales alongside visual risk severity indicators. |
| **FR-4** | Analysis History | User can view their previous message analyses within the application interface. | **Should** | 1. Retains and displays the last 50 analyses for the active session or authenticated user profile.<br>2. Displays timestamp, risk classification, confidence score, and truncated snippet for each record. |
| **FR-5** | REST API | Public API endpoint for programmatic message submission and automated analysis. | **Should** | 1. Standardized JSON REST endpoint (`/api/v1/analyze`) accepting payload schema.<br>2. Comprehensive OpenAPI / Swagger 3.0 specification available.<br>3. API key authentication supported with valid credential verification. |
| **FR-6** | User Registration & Authentication | Users can create accounts and authenticate securely to preserve longitudinal analysis history. | **Could** | 1. Email-based registration with password hashing (bcrypt / Argon2).<br>2. JSON Web Token (JWT) based session management and authorization.<br>3. Secure password recovery and profile management workflows. |
| **FR-7** | Feedback Mechanism | Users can report incorrect or contested classifications to support active learning. | **Could** | 1. Interface contains an interactive "Report Misclassification" or rating widget.<br>2. Discrepancy reports and optional corrected labels are securely recorded in the database for continuous model re-training. |
| **FR-8** | Dashboard | Overview of aggregated platform analytics and fraud detection telemetry. | **Could** | 1. Administrative or analytical dashboard visualizing total analyses conducted.<br>2. Real-time metric displays: fraud detection percentage rate, average confidence scores, and historical temporal trends. |
| **FR-9** | Multi-language Input | Accept and accurately classify messages submitted in English, Russian, and Kazakh. | **Should** | 1. Robust multilingual tokenization and feature extraction handling English, Russian (Cyrillic), and Kazakh (Cyrillic and Latin script variants).<br>2. Consistent classification performance across all three supported languages. |
| **FR-10** | Batch Analysis | Upload and process multiple messages simultaneously via file batching. | **Could** | 1. File uploader supports CSV and plain TXT file formats containing multiple text entries.<br>2. System executes bulk inference and provides downloadable structured reports (CSV/JSON). |
| **FR-11** | External Message Ingestion | Receive normalized messages from an SMS application or Telegram bot through a protected API contract. | **Should** | 1. Payload includes message text, source channel, received time, and optional sender metadata.<br>2. The same ML pipeline is used for web and external-channel messages.<br>3. Duplicate delivery is handled idempotently. |
| **FR-12** | Risk Notification | Warn the user when an analyzed message is classified as high risk. | **Should** | 1. Web results display risk level, reasons, and safety advice.<br>2. An implemented adapter can return the warning to its channel.<br>3. The system does not delete or block the original message. |

---

## 3. Non-Functional Requirements

Non-functional requirements describe quality attributes, performance constraints, architectural criteria, and security guardrails governing the platform.

### 3.1 Non-Functional Requirements Matrix

| ID | Requirement | Category | Acceptance Criteria |
| :--- | :--- | :---: | :--- |
| **NFR-1** | Latency & Response Time | Performance | 1. Server-side processing and ML inference round-trip time $\le$ 2.0 seconds at the 95th percentile under standard operating conditions.<br>2. Time-to-first-byte (TTFB) for static UI assets $\le$ 300 ms. |
| **NFR-2** | High Availability | Availability | 1. Platform system uptime achieves $\ge$ 99.0% availability during standard operational periods.<br>2. Automated health check endpoints (`/healthz`, `/readyz`) enable orchestrator restart and failover. |
| **NFR-3** | Concurrency & Scalability | Scalability | 1. The backend service sustains $\ge$ 100 simultaneous concurrent users without request dropping or connection timeouts.<br>2. Stateless architecture supports horizontal scaling across containerized instances. |
| **NFR-4** | API Rate Limiting | Security | 1. Enforces rate limiting of maximum 100 requests per minute per IP address.<br>2. Returns HTTP status code `429 Too Many Requests` with informative `Retry-After` headers upon threshold breach. |
| **NFR-5** | Transport Layer Security | Security | 1. Mandatory HTTPS enforcement across all endpoints utilizing TLS 1.3 (with TLS 1.2 fallback).<br>2. Automated HTTP to HTTPS redirection and strict HTTP Strict Transport Security (HSTS) headers. |
| **NFR-6** | Data Privacy & Minimized Retention | Privacy | 1. Raw message text is processed only for analysis and is not retained as a permanent record by default.<br>2. History stores only necessary metadata, classification, explanation indicators, source, and an optional short redacted preview or hash; private transcripts and unnecessary PII are not stored.<br>3. External adapters require explicit user permission or action. |
| **NFR-7** | Responsive Web Design | Usability | 1. User interface adapts fluidly across mobile viewports (minimum width 320px), tablet devices, and desktop viewports (up to 4K resolution).<br>2. Touch targets adhere to minimum dimension guidelines ($\ge$ 48x48 px). |
| **NFR-8** | Accessibility Compliance | Usability | 1. Client web application complies with Web Content Accessibility Guidelines (WCAG) 2.1 Level AA standards.<br>2. Semantic HTML5 elements, ARIA attributes, full keyboard navigability, and sufficient color contrast ratios ($\ge 4.5:1$). |
| **NFR-9** | Automated CI/CD Pipeline | Maintainability | 1. Continuous Integration (CI) pipeline executes automated linting, type-checking, and unit test suites on every pull request.<br>2. Continuous Deployment (CD) automates builds and zero-downtime rollouts to staging and production cloud environments upon branch merge. |
| **NFR-10** | Code Quality & Test Coverage | Maintainability | 1. Overall codebase unit and integration test coverage maintains $\ge$ 70%.<br>2. Static analysis linters (e.g., `golangci-lint` for Go, `ESLint` / `TypeScript` compiler for frontend, `flake8` / `pytest` for Python) execute without fatal warnings. |

---

## 4. Requirement Priorities (MoSCoW Prioritization)

The platform requirements are structured according to the industry-standard **MoSCoW** (Must have, Should have, Could have, Won't have) prioritization framework. This categorization balances academic evaluation objectives with practical project execution milestones.

```
       ┌─────────────────────────────────────────────────────────┐
       │             MoSCoW REQUIREMENTS TAXONOMY                │
       └─────────────────────────────────────────────────────────┘
            │                                       │
            ▼                                       ▼
  ┌───────────────────┐                   ┌───────────────────┐
  │     MUST HAVE     │                   │    SHOULD HAVE    │
  │ • FR-1: Submit    │                   │ • FR-4: History   │
  │ • FR-2: Classify  │                   │ • FR-5: REST API  │
  │ • FR-3: Explain   │                   │ • FR-9: Multiling │
  │ • NFR-1 to NFR-10 │                   │                   │
  └───────────────────┘                   └───────────────────┘
            │                                       │
            ▼                                       ▼
  ┌───────────────────┐                   ┌───────────────────┐
  │    COULD HAVE     │                   │    WON'T HAVE     │
  │ • FR-6: Auth/User │                   │ • Multimodal OCR  │
  │ • FR-7: Feedback  │                   │ • Telecom Sniffer │
  │ • FR-8: Dashboard │                   │ • Native iOS/Andr │
  │ • FR-10: Batch    │                   │ • Hidden Monitoring│
  └───────────────────┘                   └───────────────────┘
```

### 4.1 Must Have (Core MVP Foundation)
The system cannot be considered minimally viable or defensible as a diploma deliverable without these capabilities:
- **FR-1 (Message Submission):** The entry point enabling users to provide raw text for threat assessment.
- **FR-2 (ML-Based Classification):** The core intelligence engine delivering ternary classification (*Safe*, *Suspicious*, *Fraudulent*) with confidence scores within bounded latency.
- **FR-3 (Result Explanation):** The critical differentiator offering interpretable transparency by surfacing at least 3 decisive linguistic/structural indicators.
- **NFR-1 through NFR-10:** Universal non-functional baselines (sub-2s latency, 99% uptime, 100 concurrent users, rate limiting, HTTPS, privacy-preserving minimized retention of message bodies, responsiveness, WCAG 2.1 AA accessibility, automated CI/CD, and $\ge$ 70% test coverage).

### 4.2 Should Have (High-Value Extensions)
Substantial enhancements critical to fulfilling the full academic and practical scope:
- **FR-4 (Analysis History):** Empowers users to inspect recent evaluations, compare risk assessments, and track suspicious communications over time.
- **FR-5 (REST API):** Enables third-party integration, external script queries, and programmatic testing with OpenAPI specification and API key enforcement.
- **FR-9 (Multi-language Input):** Broadens the platform's utility in the target demographic by accepting English, Russian, and Kazakh languages.

### 4.3 Could Have (Desirable Enhancements)
Features that expand user experience and system utility if sprint velocity permits:
- **FR-6 (User Registration / Authentication):** Grants long-term personalized account state and cross-device history persistence via JWT.
- **FR-7 (Feedback Mechanism):** Closes the continuous improvement loop by collecting user verification labels to augment dataset quality.
- **FR-8 (Dashboard):** Visualizes system-wide throughput, threat distribution metrics, and model performance trends.
- **FR-10 (Batch Analysis):** Accommodates bulk CSV/TXT processing for institutional helpdesk or research evaluations.
- **FR-11/FR-12 (Channel Extension):** Provides a common ingestion contract and risk-warning workflow for the planned SMS and Telegram adapters when implementation time and platform access permit.

### 4.4 Won't Have (Explicit Out-of-Scope Items for Current Release)
To prevent scope creep and maintain development focus:
- Binary file, image attachment, or audio stream OCR/parsing.
- Full-scale native mobile products requiring broad device support and a complete background security suite.
- Telecom-level SS7 or carrier-grade network packet interception.
- Hidden monitoring of private chats or messages without explicit user action and consent.

---

## 5. Traceability

### 5.1 Project Management Traceability Model
To guarantee that academic goals and software engineering requirements are rigorously implemented, tested, and audited, every functional requirement (FR-1 through FR-12) and non-functional requirement (NFR-1 through NFR-10) is mapped to actionable work items within the team's project management system (**YouTrack**).

```
┌─────────────────────────────────┐
│     Requirements Document       │
│      (FR-1..12, NFR-1..10)      │
└────────────────┬────────────────┘
                 │ Traced via Issue Keys
                 ▼
┌─────────────────────────────────┐
│         YouTrack Tasks          │
│    (User Stories / Epics)       │
└────────────────┬────────────────┘
                 │ Implemented via Branches & Commits
                 ▼
┌─────────────────────────────────┐
│     Source Code & Artifacts     │
│   (Backend, ML Model, Frontend) │
└────────────────┬────────────────┘
                 │ Verified via Automated Test Suites
                 ▼
┌─────────────────────────────────┐
│    Verification & Acceptance    │
│  (Unit, Integration, CI Audits) │
└─────────────────────────────────┘
```

### 5.2 Traceability Mapping Matrix

| Requirement ID | YouTrack Epic / Workstream | Target Architectural Component | Verification Method |
| :--- | :--- | :--- | :--- |
| **FR-1** | `CORE-UI-INPUT` | React Frontend (`MessageInput` component) | E2E Browser Testing, Form Validation Tests |
| **FR-2** | `ML-INFERENCE-ENGINE` | Python ML Service (`fastapi`/`onnx`/`sklearn`) | Unit Benchmarks, Classification Metric Evaluation |
| **FR-3** | `ML-EXPLAINABILITY` | Explainability Module (LIME / SHAP / Rule-based) | Output Inspection, Feature Attribution Coverage |
| **FR-4** | `DATA-STORE-HIST` | Go Backend & PostgreSQL Schema | Repository Integration Tests, Query Profiling |
| **FR-5** | `API-GATEWAY-PUB` | Go HTTP Router (`chi` / `gin`) & OpenAPI Spec | Automated OpenAPI Contract Tests, Postman Collection |
| **FR-6** | `AUTH-SECURITY` | Go Auth Middleware & JWT Services | Security Pen-Testing, Token Expiry Unit Tests |
| **FR-7** | `FEEDBACK-LOOP` | Feedback Controller & Annotation Database | Endpoint Integration Tests, Form Validation |
| **FR-8** | `ANALYTICS-DASH` | Frontend Charting (`Chart.js`/`Recharts`) & API | Component Visual Tests, Aggregation Query Tests |
| **FR-9** | `ML-NLP-MULTILANG` | Multilingual Tokenizer & ML Pipeline | Trilingual Precision/Recall Test Suites |
| **FR-10**| `BATCH-PROCESS` | Async Worker Pool & Multipart Form Parser | Bulk Load Ingestion Tests, Format Validation |
| **FR-11**| `CHANNEL-INGEST` | Go Ingestion API and Channel Adapters | Adapter Contract and Idempotency Tests |
| **FR-12**| `RISK-NOTIFY` | Risk Policy and Notification UI | High-Risk Warning Integration Tests |
| **NFR-1** | `PERF-OPTIMIZE` | Go Backend & Inference Service Gateway | Load Testing (`k6` / `Vegeta`) under simulated loads |
| **NFR-2** | `INFRA-SRE` | Cloud Orchestration (Render / Railway / Vercel) | Uptime Monitoring (`BetterStack` / `UptimeRobot`) |
| **NFR-3** | `SCALE-CONCURR` | Go Goroutines & Concurrency Handlers | Stress Testing with 100+ simulated users |
| **NFR-4** | `SEC-RATELIMIT` | Rate Limiter Middleware (`tollbooth` / Redis) | Rate Limiter Unit Tests, Automated Flooding Tests |
| **NFR-5** | `SEC-TLS` | Edge Routing & TLS Configuration | SSL Labs Evaluation, Automated Header Inspections |
| **NFR-6** | `PRIV-ZERO-RET` | In-Memory Processing & Scrubbing Pipeline | Memory Profiling, Database Persistence Auditing |
| **NFR-7** | `UI-RESPONSIVE` | Tailwind CSS Responsive Breakpoint Rules | Cross-device Viewport Emulation, Lighthouse Audits |
| **NFR-8** | `UI-A11Y` | Accessible UI Design System | Automated Axe-Core Audits, Keyboard Navigation QA |
| **NFR-9** | `DEVOPS-CICD` | GitHub Actions Workflows | Automated CI Pipeline Execution on Pull Requests |
| **NFR-10**| `QA-COVERAGE` | Test Runner & Coverage Reporters | Automated Codecov / Coveralls Enforcement ($\ge 70\%$) |
