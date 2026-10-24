# Supervisor Feedback

**Project:** Intelligent Web Platform for Analysis, Detection, and Prevention of Fraudulent Messages Using Machine Learning Methods
**Team:** Marat Danial, Abdikasym Nurbakyt (SE-2406)
**Supervisor:** Konstantin Latuta
**Repository version:** `prob-period-1`

Prepared by the team at the instructor's request. To be confirmed by the supervisor (Konstantin Latuta).

## 1. Reviewed Materials

The following project materials were reviewed:

- project proposal and scope;
- requirements specification;
- literature review;
- comparison of existing systems;
- technology comparison;
- system architecture description and diagrams;
- UX prototype;
- verification plan;
- project-management documentation;
- survey-based evidence of relevance.

## 2. General Feedback

The project topic is relevant because users regularly receive suspicious
messages through SMS, messengers, and e-mail, while many users have
difficulties identifying phishing and social-engineering attempts. The
proposed web-based platform has a clear practical purpose and provides a
reasonable basis for a diploma project.

The project scope and architecture are generally well structured. The
separation between the React frontend, Go backend, Python ML service, and
PostgreSQL database is appropriate for the planned system. The use of a
normalized ingestion API for future SMS and Telegram integrations is also a
suitable design decision.

The architecture diagrams and survey-based evidence of relevance improve the
clarity of the proposal. The web-first approach is appropriate because it
keeps the core message-analysis workflow independent of external platform
permissions.

## 3. Recommendations

The following recommendations should be addressed during the next stage:

1. Keep the web-based message analysis workflow as the guaranteed MVP and do
   not make the project dependent on external platform permissions.
2. Clearly distinguish implemented features, planned features, and
   prototype-only features in the documentation.
3. Prepare a labelled dataset and evaluate a baseline machine-learning model
   using appropriate metrics, including precision, recall, F1-score, and a
   confusion matrix.
4. Do not present unmeasured accuracy, latency, usage, or deployment values as
   actual project results.
5. Preserve privacy by minimising stored message content and documenting the
   message-retention policy.
6. Add tests for invalid input, duplicate external messages, high-risk
   notifications, and failures of dependent services.
7. Keep future features such as mobile applications, browser extensions,
   offline analysis, and automatic blocking outside the core MVP unless
   sufficient development time remains.
8. Continue using GitHub branches, pull requests, reviews, and YouTrack task
   references to demonstrate the development process.

## 4. Required Follow-up Actions

The team should complete the following actions after this review:

- finalise the dataset and document its sources and labelling process;
- implement and evaluate a reproducible ML baseline;
- implement the core backend analysis endpoint;
- connect the frontend to the analysis workflow;
- document the database schema and privacy controls;
- add unit, API, integration, and error-handling tests;
- define the normalized ingestion contract for SMS and Telegram;
- demonstrate external-channel processing with a real adapter or a clearly
  documented test adapter;
- update the verification plan with measured results;
- keep the project status and GitHub history aligned with the actual
  implementation state.

## 5. Review Carried Out by the Team

Every change reached `main` through a pull request reviewed by the other team member. The main findings and the resulting revisions:

| # | Finding | Decision | Change | Reviewed by |
|---|---|---|---|---|
| 1 | Architecture diagrams had overlapping labels; the context diagram showed internal components; the use case diagram did not follow UML notation | Redraw diagrams: platform as one system in the context diagram, UML associations and «include»/«extend» in the use case diagram, editable Mermaid sources | [PR #1](https://github.com/doni9977/fraud-guard/pull/1) | Marat Danial |
| 2 | Several literature references had wrong venue, pages, DOI or authors; too few sources from the last five years | Check every reference against its DOI or publisher page, fix 7 references, add 4 papers from 2023–2025 | [PR #3](https://github.com/doni9977/fraud-guard/pull/3) | Marat Danial |
| 3 | Existing systems comparison used URL checkers for a text-message problem, contained two unverifiable references and showed the planned system as fully implemented | Compare with text-based scam tools, add a source for each claim, mark the proposed system as target | [PR #4](https://github.com/doni9977/fraud-guard/pull/4) | Marat Danial |
| 4 | Public datasets are binary, but the requirements use three risk classes | Derive Safe / Suspicious / Fraudulent from the fraud probability with thresholds tuned on the validation split | [PR #6](https://github.com/doni9977/fraud-guard/pull/6), [PR #7](https://github.com/doni9977/fraud-guard/pull/7) | Marat Danial |
| 5 | Technology comparison presented DistilBERT as part of the selected solution and estimates as measurements | TF-IDF + classical ML as the selected approach, DistilBERT optional; figures marked as estimates | [PR #5](https://github.com/doni9977/fraud-guard/pull/5) | Marat Danial |
| 6 | Endpoint names and use case IDs differed between documents | Use `/api/v1/analyze` and `/api/v1/messages/ingest` everywhere; align the use case table with the diagram | [PR #9](https://github.com/doni9977/fraud-guard/pull/9), [PR #10](https://github.com/doni9977/fraud-guard/pull/10) | Marat Danial |
| 7 | Prototype link was missing | Add the Figma prototype link and describe the key screens; demo values marked as not real data | [PR #10](https://github.com/doni9977/fraud-guard/pull/10) | Marat Danial |
| 8 | Evidence of relevance: share of respondents who find identification difficult is 45%, not half | Recorded as a review comment; to be corrected in the next update | [PR #11](https://github.com/doni9977/fraud-guard/pull/11) | Abdikasym Nurbakyt |

## 6. Status and Limitations

The current work provides a suitable foundation for continuing to the ML
baseline, backend API, and integrated web prototype. At this stage, the
production ML model, complete backend implementation, database integration,
and full external-channel integrations should not be presented as completed
unless they have been implemented and verified.

The survey evidence is based on a small group of 20 respondents and is
descriptive rather than statistically representative. It supports the
relevance of the problem and the proposed feature priorities, but it does not
prove model accuracy or system performance.
