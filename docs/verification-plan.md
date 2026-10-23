# Verification and Evaluation Plan

## 1. Introduction
This document outlines the testing strategy, evaluation metrics, and acceptance criteria for the Intelligent Web Platform for Fraud Message Detection.

## 2. ML Model Evaluation

### 2.1 Dataset
- Training/validation/test split: 70/15/15, stratified by class, fixed random seed (42) so results can be reproduced
- Cross-validation: 5-fold stratified on the training part
- Dataset sources: public datasets (UCI SMS Spam Collection, a public smishing dataset), supplemented with manually collected and anonymised samples. Each source, its licence and class balance are recorded in `src/ml/data/README.md`
- The test split is used only once, for the final comparison of models

### 2.2 Labels and the three risk classes
Public datasets are mostly binary (spam/fraud vs. legitimate). The model is therefore trained as a binary classifier that outputs the fraud probability *P*. The three classes from FR-2 are derived from *P*:
- *P* < 0.30 → Safe
- 0.30 ≤ *P* < 0.70 → Suspicious
- *P* ≥ 0.70 → Fraudulent

The thresholds are tuned on the validation split and then fixed before the test evaluation. Binary metrics (fraud vs. legitimate) are reported on the test split; in addition, a small manually labelled set with all three classes is used to check that the Suspicious band is meaningful.

### 2.3 Metrics
| Metric | Description | Target |
|---|---|---|
| Accuracy | Overall correct predictions | ≥ 85% |
| Precision | True positives / (True positives + False positives) | ≥ 85% |
| Recall | True positives / (True positives + False negatives) | ≥ 80% |
| F1-Score | Harmonic mean of Precision and Recall (macro average) | ≥ 0.85 |
| AUC-ROC | Area under ROC curve | ≥ 0.90 |
| False Positive Rate | Legitimate messages classified as fraud | ≤ 5% |

### 2.4 Baseline Model
Two baselines are used for comparison:
1. **Keyword rules** — a simple filter that flags a message when it contains typical fraud words or a link. It represents the rule-based approach of existing spam filters.
2. **Naive Bayes with TF-IDF features** — the simplest ML baseline.

The selected model must outperform both baselines in F1-score on the same test split.

### 2.5 Models to Evaluate
| Model | Type | Rationale |
|---|---|---|
| Naive Bayes + TF-IDF | Baseline | Simple, fast, interpretable |
| SVM + TF-IDF | Classical ML | Strong for text classification |
| Random Forest + TF-IDF | Classical ML | Handles non-linear patterns |
| Logistic Regression + TF-IDF | Classical ML | Probabilistic output |
| BERT (fine-tuned) | Deep Learning | State-of-the-art NLP (if resources allow) |

### 2.6 Evaluation Procedure
1. Train each model on the training set
2. Validate using 5-fold cross-validation
3. Evaluate on held-out test set
4. Compare confusion matrices
5. Analyze errors (false positives and false negatives)
6. Select best model based on F1-score and inference time

## 3. API Performance Testing

| Metric | Tool | Target |
|---|---|---|
| Response time (p50) | k6 / wrk | ≤ 500ms |
| Response time (p95) | k6 / wrk | ≤ 2000ms |
| Throughput | k6 | ≥ 50 req/sec |
| Error rate | k6 | ≤ 1% under normal load |
| Concurrent users | k6 | ≥ 100 |

Load tests run against the deployed staging environment (Render free tier for backend and ML service), with the ML service already warmed up so cold start is not counted. Cold start time is measured and reported separately.

### 3.1 Test Scenarios
- Normal load: 10 concurrent users, 5 minutes
- Peak load: 100 concurrent users, 2 minutes
- Stress test: Gradually increase to 500 users

## 4. Frontend/UX Testing

### 4.1 Usability Testing
- 5 test participants
- Task-based evaluation:
  1. Submit a message for analysis
  2. Interpret the result
  3. View analysis history
- Metrics: Task completion rate (target: 100%), Time on task, System Usability Scale (SUS) score (target: ≥ 70)

### 4.2 Cross-browser Testing
- Chrome, Firefox, Safari, Edge (latest versions)
- Mobile responsive: iOS Safari, Android Chrome

## 5. Integration Testing
- End-to-end test: Submit message through UI → API → ML Service → Display result
- Channel ingestion test: SMS/Telegram adapter or test adapter → ingestion API → ML Service → risk policy → warning → web history
- Contract test: validate source, timestamp, message identifier, and text fields in normalized channel payloads
- Idempotency test: repeated delivery of one external message does not create duplicate analyses or notifications
- API contract testing
- Database integrity checks

## 6. Security Testing
- OWASP Top 10 checklist
- SQL injection testing
- XSS testing
- Rate limiting verification
- Authentication/authorization testing

## 7. Acceptance Criteria Summary
| Component | Criterion | Status |
|---|---|---|
| ML Model | Macro F1-score ≥ 0.85 on the test split; better than both baselines | Pending |
| API | p95 latency ≤ 2s, ≥100 concurrent users | Pending |
| Frontend | SUS score ≥ 70, 100% task completion | Pending |
| Integration | All E2E scenarios pass | Pending |
| Channel extension | At least the documented adapter contract and test adapter flow pass; real SMS/Telegram adapter is reported separately | Pending |
| Notifications | High-risk messages produce a clear warning; original messages are never deleted or blocked | Pending |
| Security | 0 critical/high vulnerabilities | Pending |
