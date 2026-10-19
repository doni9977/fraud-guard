# Project Proposal

## Project Title
**Intelligent Web Platform for Analysis, Detection, and Prevention of Fraudulent Messages Using Machine Learning Methods**

---

## Team Members

| Name | Role | Core Responsibilities |
| :--- | :--- | :--- |
| **Marat Danial Samaluly** | Backend Developer, DevOps Engineer | Server-side architecture & development (Go), database schema design & management, RESTful API architecture, CI/CD pipeline automation, cloud deployment (Render / Railway), system performance & load testing, application security |
| **Abdikasym Nurbakyt Narimanuly** | ML Engineer, Frontend Developer | Data collection, cleaning & preprocessing, machine learning model training & evaluation (Python / NLP), client-side development (React / TypeScript), UI/UX design, frontend deployment (Vercel) |

---

## Supervisor
**Konstantin Latuta**

---

## Problem Statement
The proliferation of digital communication channels has led to an unprecedented increase in fraudulent messages across electronic communications, including phishing emails, smishing (SMS phishing), and sophisticated social engineering schemes distributed through instant messengers. According to data published by the Anti-Phishing Working Group (APWG), phishing and social engineering attacks have reached record highs in recent years, inflicting substantial financial and personal data losses on individuals and organizations alike.

Ordinary users frequently lack the requisite cybersecurity literacy and technical background to reliably recognize subtle indicators of social engineering, deceptive links, spoofed headers, and manipulative language patterns. Concurrently, existing enterprise-grade fraud detection solutions are predominantly embedded into proprietary, high-cost corporate security suites or email gateway appliances, rendering them inaccessible to everyday individuals and small businesses. 

Consequently, there is an urgent demand for an accessible, transparent, and user-centric web platform that empowers individuals to swiftly evaluate suspicious messages and comprehend potential fraud vectors using state-of-the-art machine learning and natural language processing (NLP) techniques.

---

## Target Users
The platform is designed to serve a broad spectrum of users with varying degrees of technical proficiency:
- **General Internet Users:** Everyday individuals seeking rapid verification of suspicious SMS messages, unsolicited emails, or instant messenger prompts before interacting with links or disclosing confidential data.
- **IT Support Personnel and Helpdesk Operators:** First-line support professionals who require an auxiliary diagnostic tool to evaluate potential threats reported by end-users.
- **Small and Medium-Sized Enterprises (SMEs):** Organizations operating without dedicated cybersecurity or Security Operations Center (SOC) teams that need lightweight security verification capabilities.
- **Educational Institutions:** Instructors and academic communities utilizing real-world detection tools to facilitate digital literacy, cybersecurity awareness, and threat demonstration.

---

## Aim
The primary aim of this diploma project is to design, develop, and deploy an intelligent web platform that harnesses machine learning methods to automatically parse, analyze, and detect fraudulent text messages, providing users with an explainable, accurate, and actionable evaluation of message safety. The web platform is the core system. It exposes an extensible ingestion API for a companion SMS application and a Telegram bot; implementation of one or both adapters is planned subject to platform access and project time, while the core web workflow remains independently usable.

---

## Objectives
To fulfill the stated aim, the project encompasses the following academic and technical objectives:
1. **Literature & State-of-the-Art Review:** Conduct a comprehensive review of contemporary fraud detection methodologies, natural language processing architectures, and existing anti-phishing platforms.
2. **Data Acquisition and Preparation:** Gather, curate, normalize, and label a balanced corpus of legitimate, suspicious, and fraudulent text messages across email, SMS, and messenger modalities.
3. **Model Development and Optimization:** Train, tune, and benchmark multiple ML/NLP classification models (e.g., TF-IDF baseline with classifiers, transformer-based representations) to optimize predictive accuracy and generalization.
4. **Backend Architecture & API Implementation:** Design and implement a high-throughput, secure RESTful API backend utilizing Go, responsible for request lifecycle management, database transactions, and integration pipelines.
5. **Frontend Interface Engineering:** Construct a modern, intuitive, responsive user interface utilizing React and TypeScript to facilitate frictionless user submissions and insightful visual feedback.
6. **Microservice / ML Model Integration:** Establish low-latency communication mechanisms between the Go backend service and the Python-based machine learning inference engine.
7. **Cloud Infrastructure Deployment:** Configure automated Continuous Integration and Continuous Deployment (CI/CD) pipelines and deploy backend and frontend services onto scalable cloud infrastructure (Render / Railway and Vercel).
8. **Empirical Evaluation and Validation:** Systematically evaluate system efficacy using standard statistical metrics ($Precision$, $Recall$, $F_1\text{-score}$, $ROC\text{-AUC}$) alongside usability assessments.
9. **Channel Integration Design:** Define a common message-ingestion contract and prepare prototype integration paths for an SMS application and a Telegram bot, including automatic risk warnings.

---

## Expected Outcomes
The key deliverables and outcomes expected upon project completion include:
- **Trained Machine Learning Model:** A robust text classification pipeline capable of categorizing incoming texts into safe, suspicious, or fraudulent categories with a target macro $F_1\text{-score} \ge 0.85$.
- **Production-Ready RESTful API:** A performant Go backend exposing well-documented endpoints for asynchronous message processing, analysis history, and administrative analytics.
- **Responsive Web Application:** An accessible, user-friendly client application providing detailed risk breakdowns, confidence scores, and visual threat explanations.
- **Integration-Ready Ingestion API:** A documented contract for web submissions and external SMS or Telegram adapters. At least one adapter may be delivered as a working prototype if platform constraints permit.
- **Prevention Workflow:** Risk-based warnings and safety recommendations; the system does not silently delete or block messages.
- **Cloud-Deployed System:** A fully operational, globally accessible platform hosted on resilient cloud environments with automated build and deployment pipelines.
- **Comprehensive Academic & Technical Documentation:** Rigorous documentation including the complete diploma thesis, architectural blueprints, dataset characterization, and benchmarking results.

---

## Project Scope

### In Scope
- **Text Modality Analysis:** Processing and evaluation of text entered in the web interface or received through an SMS application and Telegram bot.
- **Multi-Level Classification:** Categorization of messages across structured risk tiers (Safe, Suspicious, Fraudulent) accompanied by confidence metrics.
- **Primary Language Focus:** Robust handling of English-language communications as the primary baseline, designed with architectural modularity to support multi-language tokenization (e.g., Russian and Kazakh) in subsequent iterations.
- **Web-Based Interface:** An intuitive, single-page application (SPA) optimized for desktop and mobile browser viewports.
- **Programmatic REST API:** Standardized JSON endpoints facilitating authenticated and public API-based queries.
- **Cloud Infrastructure:** Free and low-cost containerized cloud hosting environments ensuring reproducibility and high uptime.

### Out of Scope
- **Non-Text Multimodal Artifacts:** Analysis of attached image files, OCR-based poster parsing, audio transcripts, or embedded video streams.
- **Real-Time Stream Interception:** Continuous background packet sniffing or live socket tapping into telecommunications carrier channels.
- **Full-Scale Mobile Product:** A production mobile security suite with broad device support is outside the current release. A limited SMS companion prototype may be used as an integration client.
- **Telecom-Level Integration:** Direct access to operator SMS gateways, carrier infrastructure, or hidden interception is outside the project.
- **Unauthorized Private-Chat Monitoring:** The Telegram bot only processes messages explicitly sent or forwarded by the user; it does not read private chats in the background.
