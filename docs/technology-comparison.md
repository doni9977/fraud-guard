# Technology Comparison and Technical Decision Record (TDR)

## Project Details
- **Project Title:** Intelligent Web Platform for Analysis, Detection, and Prevention of Fraudulent Messages Using Machine Learning Methods
- **Educational Program:** Software Engineering (SE-2406), School of Software Engineering
- **Team Members:**
  - **Marat Danial Samaluly** — Backend Developer, DevOps Engineer
  - **Abdikasym Nurbakyt Narimanuly** — ML Engineer, Frontend Developer
- **Academic Supervisor:** Konstantin Latuta
- **Document Version:** 1.0.0 (Pre-Defense Submission, October 2024)
- **Status:** Approved / Baselined

---

## Executive Summary

The engineering of an intelligent, high-throughput fraud detection platform necessitates deliberate architectural trade-offs across backend scalability, data persistence, machine learning inference velocity, user interface responsiveness, and cloud operations. This document formalizes the comparative analysis of technology alternatives for key technical decisions across the system's software development lifecycle.

Each technical decision is evaluated against multi-dimensional criteria derived from functional requirements, non-functional performance benchmarks, developmental constraints, operational costs, and team proficiencies.

### Master Decision Matrix

| Domain | Evaluated Alternatives | Selected Solution | Primary Strategic Justification |
| :--- | :--- | :--- | :--- |
| **1. Backend Framework** | Go (Gin), Node.js (Express), Python (FastAPI) | **Go (Gin)** | Sub-millisecond routing latency, minimal memory footprint (<30 MB), native goroutine concurrency, single static binary deployment, and strong team proficiency. |
| **2. Database** | PostgreSQL, MongoDB, MySQL | **PostgreSQL** | Strict ACID transactional integrity for user data and audit trails, powerful semi-structured `JSONB` support for ML feature payloads, and built-in full-text search. |
| **3. ML Framework** | scikit-learn, TensorFlow / Keras, PyTorch | **scikit-learn** *(with Hugging Face Transformers)* | Rapid baseline prototyping, low-resource CPU inference, broad classical ML algorithm suite, seamless transition to Hugging Face for transformer fine-tuning. |
| **4. NLP Approach** | TF-IDF + Classical ML, Pre-trained BERT, Word2Vec + BiLSTM | **TF-IDF + Classical ML** *(Baseline)* $\rightarrow$ **DistilBERT** *(Evaluation)* | Staged evolutionary methodology: immediate explainable token attribution and high-speed baseline, followed by deep contextual semantic fine-tuning. |
| **5. Frontend Framework** | React (TypeScript), Vue.js, Angular | **React (TypeScript)** | Industry-standard declarative component model, strict end-to-end type safety, expansive ecosystem (Tailwind CSS, Lucide, Radix UI), and proven team competence. |
| **6. Backend Hosting** | Render, Railway, AWS EC2 | **Render** | Generous free tier for containerized services and managed PostgreSQL, native GitOps CI/CD integration, automated SSL, and zero server maintenance overhead. |
| **7. Frontend Hosting** | Vercel, Netlify, Cloudflare Pages | **Vercel** | Specialized optimization for React/Vite builds, instantaneous preview deployments on pull requests, edge CDN delivery, and zero-configuration setups. |
| **8. Component Interaction** | REST (HTTP/JSON), gRPC (Protobuf), Message Broker (RabbitMQ) | **RESTful HTTP / JSON** | Decoupled microservice contract between Go backend and Python ML engine, universal debugging capability, and low architectural complexity for Phase 1. |
| **9. Channel Integration** | Direct telecom integration, SMS companion adapter, Telegram Bot API | **Normalized REST ingestion API** | Keeps the web platform independent from channel-specific restrictions while allowing a limited SMS or Telegram prototype and a deterministic test adapter. |

---

## 1. Backend Framework

### 1.1 Context and Problem Statement
The backend service functions as the central gateway of the platform. It is responsible for orchestrating inbound client requests, validating submitted payload schemas, managing user authentication and history records, interfacing with the primary relational database, and proxying analysis payloads to the machine learning inference microservice. The framework must ensure sub-100ms response overhead, strong static type guarantees, high concurrent connection saturation, and minimal memory overhead on constrained cloud instances.

### 1.2 Alternatives Considered
1. **Go with Gin Engine:** A statically typed, compiled systems programming language with an ultra-lightweight, high-performance HTTP web framework utilizing a custom Radix-tree based routing engine.
2. **Node.js with Express.js:** An event-driven, single-threaded asynchronous JavaScript runtime utilizing the ubiquitous, minimalist Express middleware framework.
3. **Python with FastAPI:** A modern, high-performance asynchronous web framework built on Starlette and Pydantic, leveraging Python 3.10+ type hints.

### 1.3 Comparative Evaluation

| Evaluation Criterion | Go (Gin) | Node.js (Express) | Python (FastAPI) |
| :--- | :--- | :--- | :--- |
| **Raw Request Throughput** | **Very High** (~70,000–90,000 req/sec) | **Moderate** (~20,000–30,000 req/sec) | **Moderate to High** (~25,000–40,000 req/sec) |
| **Concurrency Architecture** | Native preemptive Goroutines (M:N scheduler) | Single-threaded event loop with Libuv worker pool | Asynchronous event loop (`asyncio`) |
| **Memory Footprint (Baseline)** | **Very Low** (~15 MB – 30 MB RSS) | **Moderate** (~80 MB – 150 MB RSS) | **Moderate** (~90 MB – 180 MB RSS) |
| **Type Safety & Bug Catching** | **Strong Static Typing** at compile-time | Dynamically typed (requires TypeScript compile step) | Optional type hinting (validated at runtime via Pydantic) |
| **Binary Artifact & Deployment** | Single standalone statically linked binary | Requires Node.js runtime and large `node_modules` | Requires Python interpreter and virtual environment |
| **Cold Start Latency** | **Instantaneous** (<10 ms) | Fast (~100–300 ms) | Moderate (~300–800 ms) |
| **Ecosystem & Tooling** | Robust standard library, `go fmt`, `go test`, `pprof` | Extensive npm ecosystem, rich middleware landscape | High ecosystem synergy with Python ML libraries |
| **Team Familiarity** | **High** (Core competence of Danial Marat) | Moderate | Moderate (utilized on ML layer) |

### 1.4 Justification
**Selected Alternative: Go with the Gin Framework.**

Go was chosen as the core backend language due to its superior execution velocity, minimal CPU and memory consumption, and deterministic concurrency model. Gin’s routing tree provides negligible routing overhead ($0.1\text{x}$ allocated memory compared to standard reflection-based routers), which is critical given the memory limitations of free-tier cloud containers (512 MB RAM limit). 

While Python with FastAPI presents undeniable convenience for single-language consolidation with machine learning code, running API gateway orchestration, authentication, database transactions, and model inference within a single Python runtime invites severe concurrency bottlenecks due to the Global Interpreter Lock (GIL) and elevated memory consumption. Separating concerns by implementing the API server in Go isolates the high-traffic gateway from the resource-intensive ML pipeline. Furthermore, team member Danial Marat possesses hands-on engineering proficiency in Go, enabling rapid scaffolding of idiomatic, maintainable, and type-safe server-side code without steep learning friction.

---

## 2. Primary Database (Data Persistence)

### 2.1 Context and Problem Statement
The platform requires a persistent data store to manage structured user accounts, session tokens, categorized fraud classification reports, submitted message metadata, explainability tokens, and audit logs. The data store must enforce relational integrity for sensitive user records while maintaining flexibility to accommodate semi-structured, variable-length inference metadata (e.g., model version tags, probability distributions, identified suspicious token lists) and offering text search utilities for historical audits.

### 2.2 Alternatives Considered
1. **PostgreSQL:** An advanced, open-source object-relational database management system (ORDBMS) emphasizing standards compliance, extensibility, strict transactional integrity, and native semi-structured (`JSONB`) storage.
2. **MongoDB:** A distributed, schema-less document-oriented NoSQL database storing records in binary JSON-like (BSON) format.
3. **MySQL:** A widely adopted open-source relational database management system renowned for read-heavy transactional performance and simplicity.

### 2.3 Comparative Evaluation

| Evaluation Criterion | PostgreSQL | MongoDB | MySQL |
| :--- | :--- | :--- | :--- |
| **Data Integrity & Transactions** | **Strict ACID** compliance across multi-tables | ACID compliant for single documents; multi-doc transactions add overhead | Strict ACID compliance with InnoDB engine |
| **Schema Strictness vs. Flexibility** | Hybrid: Relational schemas + arbitrary `JSONB` | Completely dynamic / schema-less | Relational schemas; basic JSON support (lacks GIN indexing) |
| **Semi-Structured Querying** | **Exceptional** (`JSONB` with GiST/GIN index support) | **Native** (First-class BSON querying and operators) | Moderate (Functional JSON extractors without specialized GIN) |
| **Full-Text Search Capabilities** | **Native Built-in** (`tsvector`, `tsquery`, stemming, ranking) | Text indexes available, but lacks fine-grained linguistic tuning | Basic full-text search (limited language stemming in standard engines) |
| **Scalability Model** | Vertical scaling, read replicas, declarative partitioning | Horizontal scaling (Native sharding, replica sets) | Vertical scaling, replication clustering |
| **Ecosystem & Cloud Availability** | Supported on every major PaaS/DBaaS (Render, Supabase, Neon) | MongoDB Atlas, self-hosted clusters | Universally available, AWS RDS, PlanetScale |
| **Community & Tooling** | Industry standard for modern engineering, pgAdmin, GORM | Compass, Mongoose, high document community | phpMyAdmin, MySQL Workbench, broad legacy adoption |

### 2.4 Justification
**Selected Alternative: PostgreSQL.**

PostgreSQL represents the optimal hybrid storage solution for the fraud detection platform. While the user management and audit logging requirements are strictly relational—demanding foreign key constraints, unique indexes, and ACID guarantees—the analytical output of ML models varies across versions. For example, a TF-IDF classifier yields n-gram feature importances, whereas a transformer produces attention-weight spans and contextual logits. PostgreSQL resolves this heterogeneity through its binary `JSONB` data type, permitting indexed, queryable semi-structured document storage alongside normalized relational tables.

In contrast, while MongoDB natively excels at storing semi-structured documents, it introduces avoidable complexities in enforcing relational guarantees and transaction boundaries across user histories and auth tables. MySQL, while adequate for tabular data, exhibits inferior semi-structured querying capabilities and lacks Generalized Inverted Index (GIN) support for high-performance JSON querying. Moreover, PostgreSQL’s built-in full-text search (`tsvector` and `tsquery`) allows the platform to support lexical search over historical fraud submissions without requiring the operational overhead of a dedicated Elasticsearch or OpenSearch cluster in Phase 1.

---

## 3. Machine Learning Framework

### 3.1 Context and Problem Statement
The machine learning component is the analytical core of the platform, tasked with categorizing unstructured message bodies into benign, suspicious, or malicious classifications. The framework must support rapid feature engineering, clean pipeline abstraction, reproducible model evaluation, low-latency CPU-based inference, and low deployment friction within memory-constrained cloud environments.

### 3.2 Alternatives Considered
1. **scikit-learn:** A mature, open-source Python library integrating state-of-the-art classical machine learning algorithms, preprocessing utilities, and validation pipelines built upon NumPy, SciPy, and Cython.
2. **TensorFlow / Keras:** An end-to-end open-source machine learning and deep learning ecosystem developed by Google, specializing in graph-based numerical computation, recurrent models, and high-level neural network abstraction.
3. **PyTorch:** An open-source, dynamic tensor computation and deep learning framework developed by Meta AI, dominant in academic natural language processing research and transformer development.

### 3.3 Comparative Evaluation

| Evaluation Criterion | scikit-learn | TensorFlow / Keras | PyTorch |
| :--- | :--- | :--- | :--- |
| **Primary Domain Focus** | Classical ML (Classification, Regression, Clustering) | Deep Learning, Production edge deployment (TFLite) | Deep Learning, NLP, Research, Transformer architectures |
| **Pipeline Abstraction & Prototyping** | **Exceptional** (`Pipeline`, `GridSearchCV`, `ColumnTransformer`) | Moderate (Requires custom layers and Model subclassing) | Moderate (Requires custom training loops and DataLoader setups) |
| **CPU Inference Latency** | **Ultra-Low** (<5 ms for linear/ensemble classifiers) | Moderate (~25–50 ms) | Moderate (~30–60 ms) |
| **Memory Footprint at Runtime** | **Minimal** (~50 MB – 150 MB total memory) | **High** (~400 MB – 800 MB runtime overhead) | **High** (~500 MB – 1000 MB runtime overhead) |
| **Hardware Requirements** | Minimal CPU, negligible RAM, zero GPU dependency | High CPU/RAM; GPU strongly recommended for training | High CPU/RAM; GPU practically mandatory for training |
| **Serialization & Production Portability** | Native `joblib` / `pickle`, standard ONNX conversion | SavedModel, HDF5, TFLite format | TorchScript, State Dict, ONNX export |
| **NLP Transformer Integration** | Complementary with Hugging Face tokenizers | Hugging Face integration (`TFAutoModel`) | First-class citizen in Hugging Face ecosystem (`AutoModelForSequenceClassification`) |

### 3.4 Justification
**Selected Alternative: scikit-learn (as the Primary Engine), augmented with Hugging Face Transformers for BERT evaluation.**

For the development of the fraud message detection platform, scikit-learn offers the most pragmatic, reliable, and mathematically transparent foundation. In academic anti-phishing research, classical machine learning algorithms—specifically Multinomial Naive Bayes, Logistic Regression, Support Vector Classifiers (Linear SVC), and Random Forest Ensembles—consistently demonstrate remarkable performance ($F_1\text{-score} \ge 0.90$) on text classification benchmarks while requiring orders of magnitude less training time and computational overhead than deep neural networks.

Using scikit-learn enables the team to construct fully encapsulated, serializable pipelines combining string preprocessing, n-gram vectorization, and classifier estimators into a single callable artifact via `joblib`. Its ultra-compact memory footprint (<100 MB during serving) ensures that the inference microservice operates comfortably within low-cost or free-tier container limits without experiencing Out-Of-Memory (OOM) termination. For secondary comparative research, Hugging Face Transformers (utilizing a PyTorch execution backend) will be leveraged during offline experimental phases to fine-tune DistilBERT, evaluating whether deep contextual embeddings offer statistically significant improvements over the optimized scikit-learn baseline.

---

## 4. Natural Language Processing (NLP) Approach

### 4.1 Context and Problem Statement
Phishing and fraudulent text messages (e.g., smishing, social engineering, fake lottery notifications, credential harvesting prompts) employ characteristic linguistic markers, including urgent psychological triggers, deceptive hyperlink anchoring, financial terminology, and grammatical anomalies. The analytical modeling approach must accurately detect these semantic signals while remaining computationally viable and, crucially, offering explainable outputs so end users can understand *why* a message was classified as hazardous.

### 4.2 Alternatives Considered
1. **TF-IDF Vectorization with Classical Classifiers:** Converting text into statistical term-frequency inverse-document-frequency n-gram sparse matrices, followed by classification using linear classifiers (Logistic Regression, Support Vector Machines) or Naive Bayes.
2. **Pre-trained Transformer Fine-Tuning (BERT / DistilBERT):** Leveraging deep bidirectional self-attention mechanisms pre-trained on vast linguistic corpora, fine-tuning the classification head on labeled fraud datasets.
3. **Dense Word Embeddings (Word2Vec / GloVe) with Recurrent Neural Networks (BiLSTM):** Mapping words into static distributed continuous vector spaces and processing token sequences through Bidirectional Long Short-Term Memory recurrent layers.

### 4.3 Comparative Evaluation

| Evaluation Criterion | TF-IDF + Classical ML | BERT / DistilBERT Fine-Tuning | Word2Vec + BiLSTM |
| :--- | :--- | :--- | :--- |
| **Contextual Nuance & Semantics** | Bag-of-words / n-grams (lacks word order beyond n-grams) | **Deep Bidirectional Context** (Captures polysemy and complex syntax) | Sequential context (captures sequential dependencies, but static embeddings) |
| **Feature Explainability for Users** | **Direct & Highly Explainable** (Linear coefficients & feature weights) | Complex (Requires auxiliary SHAP/LIME or attention-weight extraction) | Low (Opaque hidden-state vector representations) |
| **Model Size on Disk** | **Compact** (~5 MB – 25 MB serialized) | **Large** (~250 MB – 450 MB model weights) | Moderate (~80 MB – 150 MB embeddings + weights) |
| **Training Duration** | **Minutes** on standard CPU hardware | **Hours** on GPU hardware | **Tens of minutes** on GPU / high-end CPU |
| **Inference Latency (per message)** | **< 5 milliseconds** on single CPU core | **50 – 150 milliseconds** on CPU | **20 – 40 milliseconds** on CPU |
| **Resource Overhead on Cloud** | Extremely light; runs on 256 MB RAM containers | Demanding; prone to OOM on 512 MB free-tier servers | Moderate; requires PyTorch/TF runtime |
| **Typo & Obfuscation Resilience** | High when utilizing character-level n-grams (e.g., 3-to-5-grams) | High via WordPiece / Byte-Pair subword tokenization | Low for out-of-vocabulary (OOV) tokens unless fastText is used |

### 4.4 Justification
**Selected Alternative: Staged Hybrid Approach — Baseline with TF-IDF + Classical Classifiers, followed by DistilBERT Fine-Tuning Evaluation.**

In accordance with rigorous academic methodology, establishing a reproducible, high-performing baseline is mandatory prior to deploying complex deep learning architectures. The TF-IDF representation combined with a linear Support Vector Classifier or Logistic Regression model serves as the primary production engine for Phase 1. 

This decision is motivated by three compelling engineering factors:
1. **Explainability and User Education:** The primary objective of the platform is not merely binary classification, but fraud prevention. Linear models combined with TF-IDF provide direct mathematical interpretability: top feature weights directly indicate the exact unigrams and bigrams (e.g., *"account suspended"*, *"verify immediately"*, *"wire transfer"*) that contributed to the malicious classification score, enabling the frontend to highlight hazardous phrases directly for the user.
2. **Zero-Latency CPU Serving:** The inference process executes in under 5 milliseconds on a single vCPU, eliminating latency bottlenecks and avoiding the high costs of cloud GPU provisioning.
3. **Empirical Baseline Validation:** In subsequent experimental phases, team member Nurbakyt Abdikasym will fine-tune a pre-trained DistilBERT model. If DistilBERT achieves a statistically significant margin in $F_1\text{-score}$ without exceeding container memory ceilings, its ONNX-quantized version will be transitioned to production.

---

## 5. Frontend Framework and Language

### 5.1 Context and Problem Statement
The user-facing client interface is the critical touchpoint through which users submit text snippets, upload message screenshots (for future OCR), inspect risk probability gauges, review highlighted threat phrases, and explore educational security recommendations. The frontend framework must deliver a responsive Single Page Application (SPA), ensure rigorous compile-time type safety for complex API payload contracts, and allow rapid prototyping using reusable, modern UI component libraries.

### 5.2 Alternatives Considered
1. **React with TypeScript:** A declarative, component-based view library developed by Meta, paired with TypeScript for strict compile-time type checking and IDE refactoring support.
2. **Vue.js (v3 Composition API) with TypeScript:** A progressive, lightweight JavaScript framework featuring single-file components, fine-grained reactivity, and built-in routing/state management primitives.
3. **Angular:** An enterprise-grade, batteries-included application framework engineered by Google, written in TypeScript and enforcing strict Model-View-Controller (MVC) architectural patterns.

### 5.3 Comparative Evaluation

| Evaluation Criterion | React (TypeScript) | Vue.js (TypeScript) | Angular |
| :--- | :--- | :--- | :--- |
| **Component Architecture** | Declarative JSX / TSX functional components with Hooks | Single File Components (`.vue`) with Composition API | Opinionated Modules, Components, Services, and Dependency Injection |
| **TypeScript Support & Ergonomics** | **Native & Comprehensive** (TSX treats JSX as first-class expressions) | Good in Vue 3, but template expressions historically looser | Built on TypeScript from inception; highly dogmatic |
| **UI Library Ecosystem** | **Vast & Unrivaled** (Tailwind CSS, Radix UI, Lucide Icons, Shadcn UI) | Moderate (Vuetify, PrimeVue, Quasar) | High for enterprise (Angular Material), but less agile |
| **Learning Curve & Agility** | **Low to Moderate**; straightforward component composition | **Low**; gentle ramp-up and clear documentation | **High**; steep learning curve (RxJS streams, zones, decorators) |
| **Community & Industry Adoption** | **Global Leader** (#1 in Web Developer surveys) | Strong open-source following | Heavy adoption in traditional enterprise banking |
| **State Management Flexibility** | Unopinionated (Zustand, Redux Toolkit, React Query) | Pinia (official and elegant) | NgRx / RxJS (powerful but verbose) |
| **Team Familiarity** | **High** (Core competence of Nurbakyt Abdikasym) | Low to Moderate | Low |

### 5.4 Justification
**Selected Alternative: React with TypeScript.**

React combined with TypeScript was chosen due to its unparalleled component ecosystem, exceptional developer ergonomics, and team proficiency. TypeScript is non-negotiable for this project: by defining centralized Data Transfer Object (DTO) schemas that mirror the Go backend’s REST responses (such as `AnalysisResult`, `RiskScore`, `TokenHighlight`, and `ModelMetrics`), the team eliminates runtime type mismatches and unhandled null-reference exceptions.

Furthermore, the React ecosystem provides an exceptional suite of modern, lightweight UI design tools. The combination of Vite (for sub-second Hot Module Replacement), Tailwind CSS (for expressive utility-first styling), and Lucide React (for crisp cybersecurity iconography) allows Nurbakyt Abdikasym to build a polished, accessible, and responsive user interface within tight pre-defense deadlines. Angular was dismissed due to excessive boilerplate and steep architectural overhead, while Vue.js—despite its elegance—lacks the immense variety of ready-to-use modern component primitives available in the React ecosystem.

---

## 6. Backend Cloud Hosting Platform

### 6.1 Context and Problem Statement
The backend infrastructure must host the compiled Go REST API alongside the Python machine learning inference service and provide a fully managed PostgreSQL database instance. As an academic diploma project, the hosting solution must balance zero-cost or minimal operational expenditure with production-like reliability, continuous Git deployment, automated SSL certificate generation, and low configuration overhead.

### 6.2 Alternatives Considered
1. **Render:** A unified Cloud Application Platform (PaaS) providing automated zero-downtime deployments from GitHub, free web service tiers, managed PostgreSQL databases, and integrated SSL.
2. **Railway:** An infrastructure-as-a-service platform offering dynamic container orchestration, instant repository linking, and granular usage-based billing credits.
3. **Amazon Web Services (AWS EC2):** Unmanaged raw virtual machine instances (IaaS) offering total infrastructural control within the industry’s largest cloud ecosystem.

### 6.3 Comparative Evaluation

| Evaluation Criterion | Render | Railway | AWS EC2 |
| :--- | :--- | :--- | :--- |
| **Free / Student Tier Viability** | **Generous Free Tier** (Web services + managed PostgreSQL) | Limited free monthly execution credit ($5 trial) | 12-month free tier (t2.micro / t3.micro), then paid |
| **Ease of Deployment & GitOps** | **Push-to-Deploy from GitHub** (Zero manual ops) | Automatic GitHub repository builds | Requires manual SSH configuration, Docker setups, or Ansible |
| **Managed Database Availability** | **Native Managed PostgreSQL** with automated connection strings | Managed PostgreSQL plugins | Self-hosted on EC2 or managed via AWS RDS (cost-prohibitive) |
| **SSL / TLS Termination** | **Automated & Instantaneous** (Let's Encrypt certificates) | Automated SSL | Manual setup using Certbot / Nginx reverse proxies |
| **Maintenance & Administrative Burden** | **Zero Server Administration** (Fully managed PaaS) | Low maintenance | **High** (OS patching, security groups, firewall management) |
| **Docker Container Support** | Native support via `Dockerfile` or native Go/Python runtimes | Native Nixpacks and `Dockerfile` | Full unrestricted Docker / Docker Compose engine |
| **Predictability of Cost** | **Completely predictable**; no accidental bill shock | Variable consumption-based billing | High risk of billing surprises if limits are breached |

### 6.4 Justification
**Selected Alternative: Render.**

Render was selected as the primary backend and database cloud provider because it provides the most frictionless developer experience and the lowest operational risk for an academic project. Render provides free web service instances capable of running containerized applications, coupled with a free managed PostgreSQL instance. Its native GitHub integration automatically triggers builds and zero-downtime redeployments upon every merge into the `main` branch, perfectly supporting the Continuous Integration guidelines mandated by the university committee.

In contrast, while AWS EC2 offers complete infrastructural sovereignty, it incurs substantial operational overhead: configuring virtual private clouds (VPCs), hardening Linux kernel parameters, configuring Nginx reverse proxies, renewing Let’s Encrypt SSL certificates, and managing security groups divert critical engineering time away from core software engineering objectives. Furthermore, unmanaged cloud environments risk unexpected billing liabilities. Railway was considered a strong runner-up, but its transition toward a time-bounded credit model makes Render the safer, zero-cost operational choice for the duration of the academic year.

---

## 7. Frontend Cloud Hosting Platform

### 7.1 Context and Problem Statement
The client-side single-page application must be distributed globally with low network latency, instant cache invalidation, automated build validation, and high availability. The hosting platform must seamlessly integrate with modern bundlers (Vite/React) and provide pull-request preview environments so both team members and the academic supervisor can test UI revisions prior to production merging.

### 7.2 Alternatives Considered
1. **Vercel:** A specialized cloud platform optimized for frontend frameworks and static web applications, featuring an integrated global Edge Network, automatic SSL, and preview deployments.
2. **Netlify:** An established Jamstack and serverless hosting platform offering continuous Git-based deployments, asset optimization, and edge routing.
3. **Cloudflare Pages:** A fast, secure Jamstack hosting platform running directly atop Cloudflare’s expansive global anycast network.

### 7.3 Comparative Evaluation

| Evaluation Criterion | Vercel | Netlify | Cloudflare Pages |
| :--- | :--- | :--- | :--- |
| **React / Modern SPA Optimization** | **Industry Benchmark** (Tailored presets for Vite, Next.js, CRA) | Excellent SPA support | Very good static file serving |
| **Preview Deployments per Pull Request** | **Instant & Flawless** (Generates unique staging URL per PR) | Excellent preview builds | Good branch previews |
| **Global Edge Network & Latency** | Ultra-low latency via global CDN edge points | High performance global CDN | **Exceptional** (Runs directly on Cloudflare Anycast edge) |
| **Free Tier Bandwidth & Build Limits** | Generous (100 GB bandwidth, 6,000 build minutes/month) | Generous (100 GB bandwidth, 300 build minutes/month) | Unlimited free bandwidth, 500 builds/month |
| **Build Velocity & Caching** | **Blazing Fast** (Proprietary Turborepo/Vite caching) | Fast build runners | Moderate build times |
| **Setup & Configuration Overhead** | **Near Zero** (Auto-detects Vite and sets build output) | Minimal configuration (`netlify.toml`) | Minimal configuration (`wrangler.toml`) |
| **Developer Ergonomics** | Unmatched developer experience and intuitive dashboard | Intuitive developer interface | More utility-focused dashboard |

### 7.4 Justification
**Selected Alternative: Vercel.**

Vercel is selected as the dedicated frontend deployment target. As the industry leader in modern frontend hosting, Vercel provides out-of-the-box build detection for Vite-powered React applications, automatically configuring static build directories (`dist`), caching headers, and edge compression (Brotli/Gzip) without requiring bespoke configuration scripts.

The defining factor in choosing Vercel is its preview deployment pipeline: whenever a feature branch or Pull Request is published on GitHub, Vercel immediately synthesizes an isolated staging deployment with an immutable preview URL. This functionality satisfies the university's rigorous pre-defense requirements, enabling academic supervisor Konstantin Latuta and peer reviewer Danial Marat to inspect, test, and provide feedback on frontend features before they are merged into the production trunk.

---

## 8. Inter-Service Communication (Component Interaction)

### 8.1 Context and Problem Statement
The system architecture separates the high-performance Go API gateway from the Python machine learning inference engine. A standardized inter-service communication protocol is required to transport raw message text from Go to Python and return structured classification results (risk category, confidence floats, and explainability tokens). The communication mechanism must be low-latency, resilient to transient failures, straightforward to debug, and lightweight to implement.

### 8.2 Alternatives Considered
1. **Synchronous RESTful HTTP / JSON:** A decoupled, stateless request-response paradigm using standard HTTP/1.1 or HTTP/2 transport and JSON payload serialization.
2. **gRPC with Protocol Buffers:** A high-performance, open-source universal Remote Procedure Call (RPC) framework utilizing binary serialization (Protobuf) over persistent HTTP/2 multiplexed streams.
3. **Asynchronous Message Broker (RabbitMQ / Redis PubSub):** A decoupled event-driven architecture where messages are enqueued into an intermediary message broker and processed asynchronously by worker pools.

### 8.3 Comparative Evaluation

| Evaluation Criterion | REST (HTTP / JSON) | gRPC (Protocol Buffers) | Asynchronous Message Broker |
| :--- | :--- | :--- | :--- |
| **Serialization Overhead** | Moderate (Textual JSON parsing) | **Ultra-Low** (Binary serialization via Protobuf) | Moderate (JSON or MessagePack payloads) |
| **Latency & Network Overhead** | Low (~5–15 ms local loopback) | **Sub-millisecond** loopback latency | Adds broker network hop latency |
| **Implementation Complexity** | **Very Low** (Standard Go `net/http` & Python `FastAPI`) | Moderate (Requires `.proto` definition and code generation) | **High** (Requires broker deployment, consumer dead-letters, ack handling) |
| **Debugging & Human Inspection** | **Trivial** (Human-readable JSON via Postman or cURL) | Complex (Requires specialized tools like `grpcurl` and schema files) | Complex (Requires inspecting broker management UI and queues) |
| **Architectural Decoupling** | High (Stateless contract over HTTP) | High (Strict schema contract via `.proto`) | **Maximum** (Temporal and spatial decoupling) |
| **Infrastructure Overhead** | Zero extra infrastructure (Direct point-to-point) | Zero extra infrastructure (Direct point-to-point) | Requires hosting an external RabbitMQ or Redis instance |

### 8.4 Justification
**Selected Alternative: Synchronous RESTful HTTP / JSON.**

For Phase 1 of the diploma platform, Synchronous REST over HTTP/JSON is selected as the primary inter-service communication protocol. Given that end users expect immediate, real-time feedback upon submitting a suspicious message (target end-to-end response time under 500 ms), an asynchronous broker pattern introduces unnecessary architectural complexity, message queuing delay, and extra infrastructure dependencies (e.g., managing a Redis or RabbitMQ container within strict cloud hosting budgets).

Between REST and gRPC, while gRPC offers superior binary serialization efficiency, RESTful HTTP/JSON provides clear developer velocity benefits:
1. **Universal Inspectability:** Inbound and outbound payloads can be intercepted, logged, and validated using standard developer tools, cURL, and Postman, greatly accelerating integration debugging between Danial (Go backend) and Nurbakyt (Python ML service).
2. **Standardized Pydantic/Go Structs:** Go’s `encoding/json` and Python’s `pydantic` natively parse identical JSON contracts with zero code-generation overhead.
3. **Extensibility Path:** If future load testing demonstrates that JSON serialization becomes a measurable CPU bottleneck under sustained concurrency, the interfaces are strictly encapsulated behind a Go `InferenceClient` interface, allowing a seamless migration to gRPC in Phase 2.

---

## 9. Comprehensive Architectural Synthesis

The selected technologies coalesce into a coherent, decoupled, and resilient three-tier microservice architecture:

```
[ End User Browser / Client ]
            │
            ▼ HTTPS / Edge CDN
  [ Vercel CDN ] ──> React + TypeScript Single Page Application
                            │
                            ▼ HTTPS REST API (JSON)
            [ Render PaaS: Go (Gin) API Gateway ]
               │                            │
               │ Relational / JSONB         │ Internal HTTP REST
               ▼                            ▼
      [ PostgreSQL DB ]            [ Python (FastAPI / scikit-learn) ]
      (User auth, audit,           (TF-IDF Vectorizer + Model Inference)
       historical reports)
```

1. **Client Tier:** The user interacts with an ultra-responsive, mobile-ready **React + TypeScript** web application hosted on **Vercel's global CDN**.
2. **Gateway Tier:** Inbound API calls terminate at the compiled **Go (Gin)** backend service hosted on **Render**, which handles rate limiting, authentication, session security, and database persistence.
3. **Persistence Tier:** Structured records and flexible model output artifacts are persisted in a managed **PostgreSQL** instance with full ACID guarantees.
4. **Inference Tier:** When a message requires evaluation, the Go gateway dispatches an internal REST request to the **Python inference service**, which executes the **scikit-learn** pipeline (TF-IDF vectorization and classifier scoring), returning risk ratings and explainability tokens in milliseconds.

4. **Channel Integration Tier:** A normalized REST ingestion endpoint accepts messages from the planned SMS companion application, Telegram bot, or local test adapter. The web interface remains independent and is the guaranteed MVP path.

This technology stack satisfies the core functional requirements, leaves a controlled extension path for SMS and Telegram integrations, adheres to university pre-defense quality standards, eliminates unnecessary operational expenses, and aligns directly with the established competencies of the development team.
