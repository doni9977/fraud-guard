# Intelligent Web Platform for Fraud Message Detection

[![Academic Status](https://img.shields.io/badge/Status-Analysis_%26_Design_(Pre--defense_1)-blue.svg)](#current-status--milestones)
[![University Group](https://img.shields.io/badge/Group-SE--2406-emerald.svg)](#project-team--academic-supervision)
[![Go Version](https://img.shields.io/badge/Go-1.21+-00ADD8?logo=go&logoColor=white)](#technologies--tools)
[![Python Version](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](#technologies--tools)
[![React](https://img.shields.io/badge/Frontend-React_%2B_TypeScript-61DAFB?logo=react&logoColor=black)](#technologies--tools)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-4169E1?logo=postgresql&logoColor=white)](#technologies--tools)

---

## 1. Executive Summary

**Intelligent Web Platform for Fraud Message Detection** is a diploma project aimed at providing automated, machine learning-driven protection against malicious message campaigns. The web platform accepts messages manually through its interface and is designed to receive messages from two optional external channels: an SMS companion application and a Telegram bot. It returns an ML-powered classification verdict, an interpretable confidence score, and an actionable warning.

By decoupling the high-throughput API gateway from the underlying NLP classification engine, the platform delivers near-instant response times while serving end-users, system administrators, and security analysts through an accessible, responsive web interface.

---

## 2. Problem Statement

In recent years, the frequency and sophistication of cyber threats—notably **phishing, smishing (SMS phishing), and social engineering scams**—have escalated dramatically. Fraudulent actors continuously exploit communication channels (instant messengers, SMS gateways, corporate email) using deception techniques that evade basic keyword filters.

Key challenges include:
- **Lack of accessible end-user verification tools**: Everyday internet users receive suspicious messages daily without a simple, immediate way to assess their threat level before clicking malicious links or sharing sensitive data.
- **Cognitive burden and human error**: Social engineering attacks exploit urgency, fear, and curiosity, making manual threat detection difficult even for experienced users.
- **Disjointed enterprise reporting**: Organizations and IT support desks often lack centralized, rapid-triage mechanisms to inspect flagged communication snippets submitted by employees.

This project addresses these issues by developing an accessible, reliable, and intelligent text analysis platform that delivers rapid, objective risk evaluations.

---

## 3. Target Audience

| User Group | Description & Value Proposition |
| :--- | :--- |
| **General Internet Users** | Individuals receiving unsolicited, alarming, or suspicious SMS messages, emails, and messenger texts who need immediate verification without technical overhead. |
| **IT Support & Helpdesk Staff** | First-line technical teams in institutions who require a standardized utility to rapidly evaluate reported messages from end-users. |
| **Organizations & SMEs** | Small-to-medium businesses lacking dedicated cybersecurity teams that seek lightweight, reliable tooling to improve employee awareness and reduce phishing incidence. |

---

## 4. Project Scope

To ensure high model precision and timely engineering execution, strict project boundaries have been established:

### In-Scope
- **Text-based Message Analysis**: Evaluation of textual content entered through the web interface or received through the planned SMS and Telegram integrations.
- **Binary & Multi-class Fraud Classification**: Identifying spam, phishing, financial fraud, impersonation, and legitimate messages (ham).
- **Explainability & Confidence Scoring**: Providing confidence percentages and indicator signals (e.g., suspicious keywords, deceptive urgency markers, link anomalies).
- **Interactive Web Interface**: A responsive web portal for submitting content and inspecting historical submission insights.
- **RESTful API**: Standardized backend interfaces for submitting analysis requests and fetching classifications.
- **External Channel Adapters**: A normalized ingestion contract for an SMS companion application and a Telegram bot. At least one adapter may be implemented as a working prototype subject to platform and schedule constraints.
- **Risk Notifications**: User-facing warnings for high-risk messages; the system does not delete or block messages.

### Out-of-Scope
- **Non-textual Media Analysis**: Deep learning analysis of images, audio recordings, video files, or raw document attachments (e.g., PDF/DOCX macro analysis).
- **Passive Real-time Network Monitoring**: Deep packet inspection (DPI), direct telco network tapping, or carrier-level background interception.
- **Unauthorized Message Interception**: Hidden monitoring, telecom-level interception, or reading private messages without explicit user consent.

---

## 5. Expected Outcomes & Performance Targets

Upon completion of the diploma project, the platform will deliver:
1. **High Classification Accuracy**: An operational machine learning model achieving a balanced **F1-score $\ge$ 0.85** on benchmark test datasets containing real-world fraud and legitimate messages.
2. **Sub-second Response Times**: Low-latency classification pipeline ensuring users receive verdicts within seconds of submission.
3. **Transparent Reasoning**: User-facing indicators highlighting the rationale behind flagged texts to raise digital literacy.
4. **End-to-End Deployed Architecture**: Fully cloud-hosted, reproducible services orchestrated via automated continuous integration and continuous deployment (CI/CD).

---

## 6. Technologies & Tools

The system follows a modular microservice-inspired architecture, separating client interaction, core business logic, and machine learning inference:

```
[ User Browser / Client ]    [ SMS App / Telegram Bot ]
           │                         │
           ▼                         ▼
[ React + TypeScript SPA ]    [ Channel Adapters ]
           │                         │
           └──────── HTTPS / REST ────┘
                         ▼
[ Go API Backend (Gin) ] ─────── [ PostgreSQL Database ]
  (Hosted on Render)
           │
           │ Internal HTTP / JSON
           ▼
[ Python ML Service (scikit-learn / NLP) ]
```

| Layer | Technology | Rationale & Responsibilities |
| :--- | :--- | :--- |
| **Frontend** | **React**, **TypeScript** | Type-safe, component-driven UI delivering responsive interaction, real-time validation, and rich visual verdict indicators. Deployed on **Vercel**. |
| **Backend API** | **Go (Gin framework)** | High-concurrency, memory-efficient RESTful API gateway managing authentication, rate limiting, persistence, and service routing. Deployed on **Render** / **Railway**. |
| **Machine Learning** | **Python (scikit-learn, NLP libraries)** | Specialized service responsible for text preprocessing, tokenization, feature extraction (TF-IDF/embeddings), model inference, and confidence score generation. |
| **Database** | **PostgreSQL** | Relational data store for audit logs, user submissions, feedback annotations, and application configuration. |
| **CI / CD** | **GitHub Actions** | Automated testing, linting, image building, and continuous deployment workflows. |

---

## 7. Current Status & Milestones

- **Current Milestone**: **Pre-defense 1 — Analysis & Design Phase**
- **Activities Underway**:
  - Domain research, literature review, and dataset curation (SMS/email fraud corpora).
  - Software requirements specification (SRS) and architecture modeling.
  - UI/UX wireframing and API contract definition.
  - Baseline model experimentation and preprocessing pipeline design.
  - Design of an extensible message-ingestion layer for the web, SMS, and Telegram channels.

---

## 8. Repository Structure Overview

```text
├── config/
│   └── .env.example          # Template environment variable configuration
├── diagrams/
│   ├── source/               # Editable Mermaid diagram sources
│   └── exports/              # Generated diagram exports
├── docs/                     # Diploma documentation, requirements specs, and academic reports
├── src/
│   ├── backend/              # Go RESTful API service (Gin framework, database migrations)
│   ├── frontend/             # React + TypeScript single-page web application
│   └── ml/                   # Python NLP models, preprocessing pipelines, and inference service
├── tests/                    # Integration, end-to-end, and performance test suites
├── .gitignore                # Comprehensive Git ignore rules for Go, Python, Node.js, and OS files
└── README.md                 # Project documentation and repository entry point
```

---

## 9. Project Team & Academic Supervision

### Student Developers (Group SE-2406)
- **Marat Danial** — *Backend Developer & DevOps Engineer*
  - **Responsibilities**: Go backend architecture, database modeling & optimization, authentication, CI/CD pipelines, and cloud deployment (Render/Railway).
- **Abdikasym Nurbakyt** — *ML Engineer & Frontend Developer*
  - **Responsibilities**: Data collection, NLP feature engineering, model training & evaluation (Python), and user interface development (React + TypeScript, Vercel).

### Academic Supervisor
- **Konstantin Latuta** — *Project Supervisor & Academic Advisor*

---

## 10. Getting Started & Setup Guide

> [!NOTE]
> The project is currently in the **Analysis and Design Phase**. Detailed installation, execution, and testing guides will be provided here as runnable service code is integrated into the repository.

### Prerequisites
- Go 1.21+
- Python 3.10+
- Node.js 18+ & npm/yarn
- PostgreSQL 15+

### Instructions
The implementation is currently at the analysis and design stage. Run commands will be added when the backend, frontend, and ML services are integrated.

---

## 11. Links & Documentation

- **Project Documentation**: Available in [`/docs`](./docs)
- **Architecture Diagrams**: Available in [`/diagrams`](./diagrams)
- **Issue & Task Tracking (YouTrack)**: [fg-diploma.youtrack.cloud/projects/DIP](https://fg-diploma.youtrack.cloud/projects/DIP)

### Architecture Diagrams (Pre-Defense 1 Deliverable)

The platform architecture is formally modeled across six diagrams. Editable Mermaid sources are stored in [`diagrams/source`](./diagrams/source) and can be exported to PNG or SVG when presentation files are needed:

| # | Diagram | Description | Source and exports | Requirements |
|---|---|---|---|---|
| 1 | **System Context Diagram** | Platform as one system, people and external systems around it | [Source](./diagrams/source/system-context.md) · [SVG](./diagrams/exports/system-context.svg) · [PNG](./diagrams/exports/system-context.png) | FR-1, FR-5, FR-11, FR-12, NFR-5 |
| 2 | **Use Case Diagram** | Actors and use cases UC-01–UC-10 | [Source](./diagrams/source/use-case.md) · [SVG](./diagrams/exports/use-case.svg) · [PNG](./diagrams/exports/use-case.png) | FR-1–FR-8, FR-11, FR-12 |
| 3 | **Component Diagram** | Frontend, Go, Python, and PostgreSQL components | [Source](./diagrams/source/component-diagram.md) · [SVG](./diagrams/exports/component-diagram.svg) · [PNG](./diagrams/exports/component-diagram.png) | FR-1–FR-7, FR-11, FR-12, NFR-4 |
| 4 | **Data Flow Diagram** | Web and external channel message processing | [Source](./diagrams/source/data-flow.md) · [SVG](./diagrams/exports/data-flow.svg) · [PNG](./diagrams/exports/data-flow.png) | FR-1, FR-2, FR-3, FR-4, FR-11 |
| 5 | **Sequence Diagram** | Request-response flow across services | [Source](./diagrams/source/sequence-diagram.md) · [SVG](./diagrams/exports/sequence-diagram.svg) · [PNG](./diagrams/exports/sequence-diagram.png) | FR-1, FR-2, FR-3, FR-4, NFR-1 |
| 6 | **ER Diagram** | PostgreSQL entities, relationships, and privacy fields | [Source](./diagrams/source/er-diagram.md) · [SVG](./diagrams/exports/er-diagram.svg) · [PNG](./diagrams/exports/er-diagram.png) | FR-2, FR-4, FR-6, FR-7, FR-11, NFR-6 |
