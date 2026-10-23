# Comparative Analysis of Existing Anti-Phishing and Scam Detection Systems

## Abstract

Digital communication channels—including short message services (SMS), electronic mail, and Telegram—have increasingly become primary attack vectors for social engineering, smishing, and phishing campaigns. Defending users against these deceptive tactics requires robust automated verification systems. This document provides an academic and technical comparative evaluation of five systems: Google Safe Browsing, Google Messages Scam Detection, Kaspersky Anti-Phishing, Bitdefender Scamio, and Apache SpamAssassin. Two of them (Google Messages and Scamio) analyse message text directly; the other three represent URL reputation, commercial endpoint protection and open-source email filtering. Each system is evaluated across ten rigorous criteria encompassing algorithmic capability, deployment accessibility, multilingual adaptability (specifically within the Central Asian context, including Kazakh and Russian languages), and user interpretability. The analysis shows that tools which analyse text are closed, have no public API and do not document support for Kazakh or Russian, while open tools are limited to email or URLs. Based on these findings, we present a structured justification for the proposed intelligent anti-scam web platform.

---

## 1. Introduction and Problem Definition

Social engineering attacks have evolved substantially from crude mass-broadcast spam emails to targeted, psychologically manipulative text communications across diverse media. Perpetrators leverage urgency, impersonation of financial institutions or government authorities, and emotional triggers to extract credentials or coerce fraudulent transactions. 

Traditional cybersecurity mechanisms predominantly rely on two paradigms:
1. **Network- and domain-level telemetry:** Maintaining blacklists, reputation scores, and domain registration metadata (e.g., WHOIS records, SSL issuance).
2. **Deterministic rule engines:** Matching rigid regular expressions or static signatures within specific protocol boundaries (e.g., RFC 5322 email headers).

While effective against established infrastructure and known malicious hosts, these paradigms exhibit acute vulnerabilities when faced with modern attack strategies:
- Zero-day scam campaigns utilizing freshly generated, unlisted links or linkless fraudulent messages (e.g., social engineering requesting money transfers or one-time codes).
- Cross-channel diversification spanning SMS and Telegram, where protocol headers are absent and only natural language text is available.
- Linguistic localization exploiting regional languages, such as Kazakh and colloquial Russian, which are underrepresented in global threat intelligence databases.

To address these vulnerabilities, security systems must synthesize modern Natural Language Processing (NLP), multi-source message normalization, real-time inference, and human-interpretable reasoning. The purpose of this study is to systematically evaluate existing commercial and open-source tools to establish an evidence-based foundation for our proposed solution.

---

## 2. Evaluation Criteria

The systems under review are evaluated across ten multi-dimensional criteria reflecting technical performance, operational accessibility, and regional applicability:

| ID | Criterion | Definition and Academic Significance |
|:---|:---|:---|
| **C1** | **Text Message Analysis (SMS / Email Body)** | Ability to perform deep natural language and semantic analysis on raw unstructured message text, rather than relying exclusively on embedded URLs or domain strings. |
| **C2** | **Use of Machine Learning (ML)** | Incorporation of statistical learning, neural networks, or NLP transformers for automated feature extraction, pattern generalization, and zero-day threat detection. |
| **C3** | **Open-Source Availability** | Permissive access to the underlying codebase and model architecture, enabling independent auditability, academic validation, and continuous community development. |
| **C4** | **Multilingual Support (including Kazakh/Russian)** | Native linguistic handling and model comprehension of multilingual texts, with specific competence in regional languages (Kazakh, Russian, and English) and transliterated dialects. |
| **C5** | **Free to Use** | Unrestricted, non-commercial or public accessibility without licensing fees, paywalls, or restrictive enterprise contracts for foundational security functionality. |
| **C6** | **API for Integration** | Availability of standard programming interfaces (e.g., RESTful HTTP endpoints) allowing seamless integration into third-party security pipelines and applications. |
| **C7** | **Real-Time Analysis Capability** | Low-latency inference and threat classification (processing completed within milliseconds to seconds) suitable for synchronous user-facing interactions. |
| **C8** | **Explainability of Results** | Generation of transparent, interpretable diagnostic feedback (e.g., highlighting manipulative phrases, psychological cues, and threat risk factors) rather than opaque black-box verdicts. |
| **C9** | **User-Friendly Web Interface** | Provision of an intuitive, accessible graphic interface that allows non-technical individuals to submit suspicious content and readily comprehend the safety evaluation. |
| **C10** | **Support for Multiple Message Types** | Cross-platform versatility capable of accepting, parsing, and normalizing diverse communication modalities (SMS snippets, rich email bodies, and instant messaging conversations). |

### Evaluation Scale
- ✅ **Full support:** The feature is natively implemented, thoroughly documented, and meets industry standards.
- ⚠️ **Partial support:** The feature is limited in scope, constrained by protocol dependencies, available only in enterprise tiers, or requires complex third-party customization.
- ❌ **No support:** The capability is entirely absent or falls outside the fundamental scope of the system.

---

## 3. Comprehensive Evaluation of Existing Systems

### 3.1. Google Safe Browsing

#### System Overview
Google Safe Browsing is an industry-standard threat intelligence service developed by Google that enables client applications to check URLs against continuously updated lists of suspected phishing, social engineering, and malware-hosting web resources. It powers the security warnings in browsers such as Google Chrome, Mozilla Firefox and Apple Safari [1].

#### Capabilities and Strengths
- **Global Telemetry and Speed:** Backed by Google’s web-scale crawling and indexing infrastructure, providing instantaneous hash-based client lookups with minimal latency and high availability.
- **Enterprise-Grade Infrastructure:** Offers robust, well-documented APIs (Lookup API v4 and Update API) optimized for high throughput, mobile bandwidth constraints, and local caching.
- **High URL Accuracy:** Demonstrates extremely low false-positive rates for established malicious domains and verified malware distribution points.

#### Limitations
- **Exclusively URL-Centric:** Completely incapable of parsing or evaluating plain-text messages lacking embedded hyperlinks. Text-only social engineering scams (e.g., bank transfer requests, family impersonation) fall outside its operational purview.
- **Proprietary Black-Box Engine:** The internal classification pipelines, heuristics, and crawler detection models are closed-source.
- **Coarse Diagnostic Feedback:** Output is restricted to broad threat categories (e.g., `MALWARE`, `SOCIAL_ENGINEERING`, `UNWANTED_SOFTWARE`) with zero contextual explanation of why a specific target was flagged.

#### Criteria Assessment
- **C1: Text message analysis:** ❌ No support. Operates strictly on Uniform Resource Identifiers (URIs).
- **C2: Use of Machine Learning:** ✅ Full support. Employs advanced machine learning models internally within Google's crawl and detection pipelines.
- **C3: Open-source availability:** ❌ No support. Proprietary closed-source infrastructure.
- **C4: Multilingual support:** ⚠️ Partial support. While landing page content is analyzed globally, message text comprehension in regional languages (Kazakh/Russian) is not offered as an input service.
- **C5: Free to use:** ⚠️ Partial support. The Safe Browsing API is free for non-commercial use only; commercial use requires the paid Web Risk API [1].
- **C6: API for integration:** ✅ Full support. REST and RPC APIs (v4 and v5) [1].
- **C7: Real-time analysis:** ✅ Full support. Lookups use locally synchronised hash prefixes, so most checks need no network request.
- **C8: Explainability of results:** ⚠️ Partial support. Returns only broad threat labels; no granular reasoning or linguistic feature attribution is provided.
- **C9: User-friendly web interface:** ⚠️ Partial support. Provides a simple transparency report search bar for site owners, but lacks an interactive diagnostic portal for general message inspection.
- **C10: Support for multiple message types:** ❌ No support. Strictly limited to URL endpoints; cannot parse SMS, email headers, or messenger chats.

---

### 3.2. Google Messages Scam Detection

#### System Overview
Scam Detection is a feature of the Google Messages application on Android, announced in March 2025 [2]. On-device AI models check SMS, MMS and RCS conversations with non-contacts for patterns typical of conversational scams (for example, a harmless first message that later turns into a request for money or personal data) and show a warning in the chat.

#### Capabilities and Strengths
- **Text-based detection:** Works on the message content itself, so it can flag scams that contain no link at all.
- **On-device processing:** According to Google, message processing stays on the phone; data is shared only when the user decides to report the conversation [2].
- **Integrated warning:** The user sees the warning inside the conversation and can dismiss it or report and block the sender.

#### Limitations
- **Limited availability:** At launch the feature covered English-language users in the United States, the United Kingdom and Canada [2]. Russian and Kazakh are not mentioned.
- **Single channel:** Only works for messages received in Google Messages on Android; text from Telegram, email or iOS cannot be checked.
- **No explanation or API:** Public sources describe a warning and actions, but no breakdown of why the message was flagged and no interface for other applications.

#### Criteria Assessment
- **C1: Text message analysis:** ✅ Full support. Analyses the text of SMS/RCS conversations.
- **C2: Use of Machine Learning:** ✅ Full support. On-device AI models [2].
- **C3: Open-source availability:** ❌ No support. Proprietary.
- **C4: Multilingual support:** ❌ No support. English only at launch; no Russian or Kazakh.
- **C5: Free to use:** ✅ Full support. Free for users of the application.
- **C6: API for integration:** ❌ No support. No public API is documented.
- **C7: Real-time analysis:** ✅ Full support. Warnings appear while the conversation is happening.
- **C8: Explainability of results:** ⚠️ Partial support. A warning is shown; detailed reasons are not documented.
- **C9: User-friendly web interface:** ❌ No support. Mobile application only, no web interface for checking an arbitrary message.
- **C10: Support for multiple message types:** ⚠️ Partial support. SMS, MMS and RCS only.

---

### 3.3. Kaspersky Anti-Phishing

#### System Overview
Kaspersky Anti-Phishing is a proprietary security technology integrated into Kaspersky’s commercial endpoint protection, mail gateway, and mobile security suites. It combines local heuristics with cloud-based intelligence from the Kaspersky Security Network (KSN) [3].

#### Capabilities and Strengths
- **Cloud threat intelligence:** KSN collects data from users' devices and uses machine learning to react quickly to new threats [3].
- **Tested phishing protection:** In the 2025 AV-Comparatives anti-phishing test Kaspersky Premium detected 93% of phishing URLs and received the "Approved" certificate [6].
- **Wide coverage:** Protects web browsing and email in desktop, mobile and mail-gateway products.

#### Limitations
- **Closed Commercial Ecosystem:** Requires commercial software licensing; proprietary algorithms and source code are strictly guarded.
- **Black-Box Enterprise Output:** Decisions are opaque to end-users, presenting binary alerts ("Threat Blocked") without educational explanations of the deceptive tactics employed.
- **No Standalone Open Web Portal:** Does not offer a free, open-access web portal where non-subscribers can freely paste arbitrary text snippets or instant messaging threads for immediate analysis.

#### Criteria Assessment
- **C1: Text message analysis:** ⚠️ Partial support. Analyzes text within dedicated email/SMS filter modules of commercial suites, but not as an open standalone text analyzer.
- **C2: Use of Machine Learning:** ✅ Full support. KSN uses machine learning on cloud telemetry [3].
- **C3: Open-source availability:** ❌ No support. Fully proprietary commercial intellectual property.
- **C4: Multilingual support:** ⚠️ Partial support. Products are localised into Russian and many other languages; public sources do not describe text analysis for Kazakh.
- **C5: Free to use:** ❌ No support. Enterprise and consumer software requires paid subscription licensing.
- **C6: API for integration:** ⚠️ Partial support. Threat Intelligence Portal APIs exist, but access is restricted to paid enterprise tiers under specialized agreements.
- **C7: Real-time analysis:** ✅ Full support. High-speed local heuristics combined with cloud telemetry deliver real-time verdicts.
- **C8: Explainability of results:** ⚠️ Partial support. Alerts categorise the attack type, but offer no semantic deconstruction or educational explanation for user literacy.
- **C9: User-friendly web interface:** ⚠️ Partial support. Management is conducted via desktop client or enterprise administrator consoles; no public web playground for ad-hoc user text verification.
- **C10: Support for multiple message types:** ⚠️ Partial support. Covers web traffic and email inside installed products; an arbitrary copied messenger text cannot be checked on its own.

---

### 3.4. Bitdefender Scamio

#### System Overview
Scamio is a free AI chatbot by Bitdefender for checking suspicious content [4]. The user pastes a text, email or social media message, sends a link or QR code, uploads a screenshot or simply describes the situation, and Scamio answers whether it looks like a scam and gives advice. It is available as a web chat and in WhatsApp, Facebook Messenger and Discord.

#### Capabilities and Strengths
- **Closest to our use case:** Accepts free text from any channel, not only URLs.
- **Several entry points:** Web chat and popular messengers.
- **Advice for the user:** The answer contains a verdict and tips on what to do next.

#### Limitations
- **Closed system:** The models, rules and Bitdefender's scam database are proprietary; the decision process cannot be audited [4].
- **Account required:** The user has to sign in with a Bitdefender account before the first check [4].
- **Unclear language coverage:** The product page does not state which languages are supported; Kazakh is not mentioned.
- **No public API:** No interface for integration into other applications is described.

#### Criteria Assessment
- **C1: Text message analysis:** ✅ Full support. Accepts any text, links, QR codes and screenshots.
- **C2: Use of Machine Learning:** ✅ Full support. Described as AI-powered; also uses rules and a database of known scams [4].
- **C3: Open-source availability:** ❌ No support. Proprietary.
- **C4: Multilingual support:** ⚠️ Partial support. Language support is not documented; no information about Kazakh.
- **C5: Free to use:** ✅ Full support. Free, but requires a Bitdefender account.
- **C6: API for integration:** ❌ No support. No public API.
- **C7: Real-time analysis:** ✅ Full support. Answers within a chat session.
- **C8: Explainability of results:** ⚠️ Partial support. Gives a verdict and tips; the level of detail of the reasons is not documented.
- **C9: User-friendly web interface:** ✅ Full support. Chat interface in the browser.
- **C10: Support for multiple message types:** ✅ Full support. Text, email, social media messages, links, QR codes and screenshots.

---

### 3.5. Apache SpamAssassin

#### System Overview
Apache SpamAssassin is a mature, highly extensible open-source anti-spam platform released under the Apache License 2.0. Widely integrated into Unix mail transfer agents (MTAs) such as Postfix, Exim, and Sendmail, it processes incoming email using heuristic rules, DNS-based blocklists and Bayesian filtering [5].

#### Capabilities and Strengths
- **Granular Heuristic Rule Engine:** Evaluates RFC 5322 email headers, MIME structures, and message body text against thousands of community and customized heuristic rules.
- **Self-Hostable and Fully Open-Source:** Grants complete access to source code, configuration matrices, and score-weighting algorithms, free from licensing costs or vendor lock-in.
- **Extensible Architecture:** Integrates Bayesian statistical token learning (`sa-learn`), collaborative networks (Razor, Pyzor), and DNS blocklists (Spamhaus, URIBL).

#### Limitations
- **Rigid Email Protocol Coupling:** Designed strictly for RFC email messages with headers; cannot directly ingest, parse, or evaluate unstructured SMS or instant messenger text snippets.
- **Absence of Modern Deep Learning:** Relies on Naive Bayes bag-of-words and static regex matching. It struggles to capture complex semantic context, sentence embeddings, or nuanced paraphrase variations typical of modern smishing.
- **Lack of Native Web GUI:** Operates as a server-side command-line daemon (`spamd`/`spamc`). It possesses no out-of-the-box web interface for casual end-users.

#### Criteria Assessment
- **C1: Text message analysis:** ⚠️ Partial support. Deeply analyzes email bodies, but lacks out-of-the-box capability for raw SMS or social messaging dialogue.
- **C2: Use of Machine Learning:** ⚠️ Partial support. Uses traditional Naive Bayes token filtering; does not utilize modern deep learning or contextual transformer architectures.
- **C3: Open-source availability:** ✅ Full support. Distributed under the permissive Apache License 2.0.
- **C4: Multilingual support:** ⚠️ Partial support. Supports UTF-8 encodings and custom rules, but default rule collections are predominantly English-focused; lacks native linguistic parsing for Kazakh.
- **C5: Free to use:** ✅ Full support. Completely free and open-source software.
- **C6: API for integration:** ✅ Full support. Programmatic integration via the `spamd` network daemon protocol, Perl bindings, and Unix pipes.
- **C7: Real-time analysis:** ✅ Full support. Performs near-instantaneous pipeline processing for incoming email traffic on mail servers.
- **C8: Explainability of results:** ⚠️ Partial support. Output details every triggered rule (e.g., `BAYES_99`, `HTML_MESSAGE`, `URIBL_BLACK`), but these technical tags are cryptic and uninterpretable for non-technical users.
- **C9: User-friendly web interface:** ❌ No support. Native distribution provides command-line tooling only; requires third-party administrative frontends.
- **C10: Support for multiple message types:** ⚠️ Partial support. Strictly structured around email message formats; incompatible with SMS or instant messaging payloads.

---

## 4. Comprehensive Comparative Matrix

The matrix summarises the assessment of the five existing systems. The last column shows the **target** of the proposed platform: it is not implemented yet, and each value will be checked against the requirements and the verification plan.

| Evaluation Criteria | Google Safe Browsing | Google Messages Scam Detection | Kaspersky Anti-Phishing | Bitdefender Scamio | Apache SpamAssassin | **Proposed Solution (target)** |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| **1. Text Message Analysis (SMS / Email Body)** | ❌ | ✅ | ⚠️ | ✅ | ⚠️ | ✅ |
| **2. Use of Machine Learning** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **3. Open-Source Availability** | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| **4. Multilingual Support (incl. Kazakh/Russian)** | ⚠️ | ❌ | ⚠️ | ⚠️ | ⚠️ | ⚠️ |
| **5. Free to Use** | ⚠️ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **6. API for Integration** | ✅ | ❌ | ⚠️ | ❌ | ✅ | ✅ |
| **7. Real-Time Analysis Capability** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **8. Explainability of Results** | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ✅ |
| **9. User-Friendly Web Interface** | ⚠️ | ❌ | ⚠️ | ✅ | ❌ | ✅ |
| **10. Support for Multiple Message Types** | ❌ | ⚠️ | ⚠️ | ✅ | ⚠️ | ⚠️ |

For the proposed solution, C4 is partial because English is the baseline and Russian/Kazakh are planned as FR-9 (Could). C10 is partial because the web form and the Telegram bot are planned first, while the SMS application is a prototype.

### Summary Scorecard

| System | ✅ Full Support | ⚠️ Partial Support | ❌ No Support | Main limitation for our task |
|:---|:---:|:---:|:---:|:---|
| **Google Safe Browsing** | 3 | 4 | 3 | Checks URLs only |
| **Google Messages Scam Detection** | 4 | 2 | 4 | One app, English only, no API |
| **Kaspersky Anti-Phishing** | 2 | 6 | 2 | Paid and closed, no standalone text check |
| **Bitdefender Scamio** | 6 | 2 | 2 | Closed, no API, account required |
| **Apache SpamAssassin** | 4 | 5 | 1 | Email only, no interface for end users |

Scamio is the closest existing system to our idea. The difference of the proposed platform is not the ability to check text as such, but the combination of an open and reproducible model, a public API for channel adapters, an explanation built from the model's features, and a focus on Kazakhstan.

---

## 5. Comparative Gap Analysis

```
+-----------------------------------------------------------------------------+
|                             EXISTING APPROACHES                             |
+-----------------------------------------------------------------------------+
|  URL reputation          | Closed text-scam assistants | Email filters / AV  |
|  (Safe Browsing)         | (Google Messages, Scamio)   | (SpamAssassin,      |
|                          |                             |  Kaspersky)         |
|  - URLs only             | - No public API             | - Email or installed|
|  - Miss linkless scams   | - Model cannot be audited   |   product only      |
|                          | - Kazakh not documented     | - Technical output  |
+-----------------------------------------------------------------------------+
                                      │
                                      ▼
+-----------------------------------------------------------------------------+
|                                THE GAP                                      |
+-----------------------------------------------------------------------------+
|   No reviewed tool combines:                                                |
|   1. Analysis of message text from different channels                       |
|   2. An open, reproducible model with a public API                          |
|   3. An explanation that an ordinary user can understand                    |
|   4. Attention to the Kazakhstan context (Russian and Kazakh messages)      |
+-----------------------------------------------------------------------------+
```

### 5.1. Scams without links
URL reputation services cannot help when a message has no link at all. Typical examples are:
- **Direct money requests:** urgent messages asking to transfer money by phone number or QR code under the pretext of an erroneous transfer, a legal threat or a family emergency.
- **Verification code harvesting:** messages that make the recipient share a one-time SMS code.
- **Moving the conversation elsewhere:** requests to continue the conversation by phone or in a private messenger chat.

Text-based tools such as Google Messages Scam Detection and Scamio address this problem, which confirms that analysing message text is the right direction.

### 5.2. Regional and language context
None of the reviewed tools documents support for Kazakh, and Google Messages Scam Detection was launched for English only [2]. Scam messages in Kazakhstan are often written in Russian, in Kazakh or in a mix of both, and impersonate local banks, delivery services or government services. These observations are preliminary assumptions and are checked in the relevance study (user survey); the first version of the model is trained on English data, and Russian and Kazakh support is planned as FR-9.

### 5.3. Black-box results
Most tools return a verdict or a warning without reasons. SpamAssassin does list the triggered rules (`BAYES_99`, `URIBL_BLACK`), but these names mean nothing to an ordinary user. A user who understands *why* a message is dangerous is better prepared for the next attempt.

### 5.4. Openness and integration
The text-scam assistants are closed products without a public API, so their result cannot be used by other applications and their quality cannot be checked independently. SpamAssassin is open but requires server administration and works only with email.

---

## 6. Justification for the Proposed Solution

The proposed platform addresses these gaps within the scope defined in the project proposal.

### 6.1. Core Components

```
+-------------------------------------------------------------------------------+
|  1. Input                                                                     |
|     - Web form (core MVP)                                                     |
|     - Ingestion API for the Telegram bot and the SMS application (planned)    |
+-------------------------------------------------------------------------------+
                                      │
                                      ▼
+-------------------------------------------------------------------------------+
|  2. ML service (Python, FastAPI)                                              |
|     - Text normalisation (URLs, emails, phone numbers)                        |
|     - TF-IDF features + classical classifier (Logistic Regression baseline)   |
|     - Rule-based indicators: urgency, links, money and code requests          |
+-------------------------------------------------------------------------------+
                                      │
                                      ▼
+-------------------------------------------------------------------------------+
|  3. Explanation and risk policy (Go backend)                                  |
|     - Risk level: safe / suspicious / fraudulent                              |
|     - Top features and indicators shown to the user                           |
|     - Plain-language warning and safety advice                                |
+-------------------------------------------------------------------------------+
                                      │
                                      ▼
+-------------------------------------------------------------------------------+
|  4. Delivery                                                                  |
|     - React web interface                                                     |
|     - Documented REST API (OpenAPI)                                           |
|     - Public repository with code, data description and evaluation results    |
+-------------------------------------------------------------------------------+
```

### 6.2. Differences from the Reviewed Systems

#### 1. Text from different channels
Unlike URL services and email filters, the platform accepts plain message text from the web form and, through one ingestion API, from the Telegram bot and the SMS application. The same model is used for every channel.

#### 2. Open and reproducible model
Unlike Google Messages and Scamio, the model, the training script and the evaluation results are kept in the public repository, so the quality can be checked and compared with baselines (see the verification plan).

#### 3. Explanation for ordinary users
A linear model on TF-IDF features shows which words and phrases influenced the decision. Together with rule-based indicators (urgency, suspicious link, request for money or a code) the user gets a short explanation and advice instead of a bare verdict.

#### 4. Public API
Unlike the closed text-scam assistants, the platform has a documented REST API, which is used by the channel adapters and can be used by other applications.

#### 5. Kazakhstan context
The first model is trained on English data. Russian and Kazakh support is a planned extension (FR-9) and is evaluated separately for each language.

---

## 7. Conclusion

URL reputation services (Google Safe Browsing), commercial protection (Kaspersky), email filters (Apache SpamAssassin) and closed text-scam assistants (Google Messages Scam Detection, Bitdefender Scamio) each cover part of the problem. None of them offers an open, explainable text analysis with a public API that can be used from the web and from messaging channels. The proposed platform targets exactly this combination; whether the targets in Section 4 are reached will be shown by the evaluation described in the verification plan.

---

## References

1. Google. (n.d.). *Safe Browsing APIs*. Google for Developers. Retrieved October 23, 2026, from https://developers.google.com/safe-browsing
2. Google. (2025, March 4). *New AI-powered scam detection features to help protect you on Android*. The Keyword. https://blog.google/security/new-ai-powered-scam-detection-features/
3. Kaspersky. (n.d.). *Kaspersky Security Network*. Retrieved October 23, 2026, from https://www.kaspersky.com/enterprise-security/wiki-section/products/kaspersky-security-network
4. Bitdefender. (n.d.). *Bitdefender Scamio*. Retrieved October 23, 2026, from https://www.bitdefender.com/consumer/scamio
5. The Apache Software Foundation. (n.d.). *Apache SpamAssassin*. Retrieved October 23, 2026, from https://spamassassin.apache.org/
6. Kaspersky. (2025, September 4). *Kaspersky Premium earns Approved certificate from AV-Comparatives for high anti-phishing results* [Press release]. https://www.kaspersky.com/about/press-releases/kaspersky-premium-earns-approved-certificate-from-av-comparatives-for-high-anti-phishing-results
