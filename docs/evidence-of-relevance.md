# Evidence of Relevance

## 1. Purpose

This document records the results of a short user survey conducted to evaluate
the relevance of the project **Development of an Intelligent Web-Based Platform
for Analysis, Detection, and Prevention of Fraudulent Messages**.

The survey was used to compare the initial project assumptions with feedback
from potential users. It informs the scope of the web MVP, the priority of
features, and the design of the explanation and warning screens.

The survey measures user experience and expectations. It does not measure the
accuracy of the machine-learning model and must not be presented as a model
evaluation.

## 2. Method

- **Instrument:** Google Forms questionnaire.
- **Response file:** `Development of an Intelligent Web-Based Platform for Analysis, Detection, and Prevention of Fraudulent Messages (Ответы).xlsx`
- **Number of responses:** 20.
- **Question types:** single-choice, multiple-choice, and one optional
  open-text question.
- **Respondent profile:** mainly young potential users; the questionnaire did
  not collect names or other direct identifiers.

The results are descriptive. Percentages are calculated from 20 responses and
are rounded to the nearest whole number. Multiple-choice questions can have
more than one selected option, so their percentages do not add up to 100%.

## 3. Respondent Age Groups

| Age group | Responses | Share |
|---|---:|---:|
| 18–24 | 9 | 45% |
| 25–34 | 6 | 30% |
| Under 18 | 4 | 20% |
| 35–44 | 1 | 5% |
| **Total** | **20** | **100%** |

The sample is concentrated in the 18–34 age range. Therefore, the results
should be interpreted as feedback from potential users in this demographic,
not as a representative estimate for the whole population.

## 4. Survey Results

### 4.1 Frequency of Suspicious Messages

| Answer | Responses | Share |
|---|---:|---:|
| Every day | 5 | 25% |
| A few times a week | 3 | 15% |
| A few times a month | 3 | 15% |
| Very rarely | 4 | 20% |
| Never | 5 | 25% |

Eleven respondents (55%) reported encountering suspicious or fraudulent
messages at least a few times a month, while one quarter reported receiving
them every day. This supports the relevance of a tool that can provide a
quick check when a user is uncertain.

### 4.2 Channels Where Messages Are Received

This was a multiple-choice question.

| Channel | Respondents selecting it | Share |
|---|---:|---:|
| SMS messages | 12 | 60% |
| Messengers, including WhatsApp and Telegram | 10 | 50% |
| E-mail | 8 | 40% |
| Social networks | 7 | 35% |
| Classified ad websites | 5 | 25% |

SMS was the most frequently selected channel, followed by messengers and
e-mail. These results support the decision to make the web platform the core
system and to define external message ingestion for an SMS companion
application and Telegram bot. WhatsApp appeared in the questionnaire as an
example of a messenger used by respondents; WhatsApp integration is not part
of the current project scope.

### 4.3 Ability to Identify Fraudulent Messages

| Answer | Responses | Share |
|---|---:|---:|
| Very easy, I spot the scam immediately | 5 | 25% |
| Usually easy, but sometimes I have doubts | 6 | 30% |
| Often difficult, I have to double-check | 4 | 20% |
| Very difficult, modern scammers are highly convincing | 5 | 25% |

Half of the respondents described identifying fraudulent messages as often
difficult or very difficult. Another 30% usually manage to identify them but
still experience uncertainty. This supports the need for an explanation of
the warning signs rather than a result that only displays a label.

### 4.4 Previous Experience With Online Scams

| Answer | Responses | Share |
|---|---:|---:|
| Yes, personal losses | 7 | 35% |
| Yes, a loved one was a victim | 4 | 20% |
| No such experience | 6 | 30% |
| Hard to say | 3 | 15% |

Eleven respondents (55%) reported either personal losses or a victim among
their loved ones. This indicates that the problem is familiar to a substantial
part of the sample and supports including clear safety advice with a
high-risk result.

### 4.5 Current Protection Tools

This was a multiple-choice question.

| Tool or approach | Respondents selecting it | Share |
|---|---:|---:|
| Built-in spam protection | 11 | 55% |
| Caller ID applications | 6 | 30% |
| Antivirus software | 5 | 25% |
| No dedicated tool; rely on personal attention | 5 | 25% |

Most respondents rely on built-in protection, while one quarter reported
relying only on their own attention. This suggests a gap between receiving a
message and understanding whether its content is dangerous.

### 4.6 Interest in a Web-Based Checking Service

| Answer | Responses | Share |
|---|---:|---:|
| Yes, it would be very useful | 10 | 50% |
| Probably yes | 4 | 20% |
| Probably not | 5 | 25% |
| No, I do not trust such services | 1 | 5% |

Fourteen respondents (70%) gave a positive or probably positive answer. This
supports implementing a web-based message submission flow as the guaranteed
MVP scenario. The negative and uncertain answers also show why transparency,
privacy controls, and explanations must be part of the product design.

### 4.7 Most Important Platform Features

This was a multiple-choice question.

| Feature | Respondents selecting it | Share |
|---|---:|---:|
| Checking links for phishing | 10 | 50% |
| Analyzing manipulation and social-engineering language | 7 | 35% |
| Browser extension for automatic blocking | 6 | 30% |
| Explaining why a message was flagged | 5 | 25% |
| Telegram bot for quick checks | 5 | 25% |

Link checking was the most frequently selected feature. Text analysis and
explanation were also important. These findings support the current
requirements for risk indicators, explanations, and Telegram ingestion.
Automatic browser blocking is recorded as a possible future extension rather
than a core MVP feature because the current platform is designed to warn
users, not silently delete or block messages.

## 5. Open-Text Suggestions

Seventeen respondents provided optional comments. The suggestions were
qualitatively grouped as follows:

| Theme | Examples from responses | Implication |
|---|---|---|
| More channels and devices | Mobile application; automatic checks in Telegram and other messengers; saved SMS checking | Keep the web platform as the core and document channel adapters as extensions |
| Explanations and education | Highlight triggering words; simple reasons for the warning; educational quizzes | Include human-readable indicators and safety advice |
| Link and content protection | Browser extension; QR-code scanning; spoofed e-mail detection; deepfake links | Record as future scope and avoid expanding the MVP beyond text-message analysis |
| User and community support | Community reporting; family monitoring; weekly phishing digests | Consider as future enhancements after the core workflow is validated |
| Analytics and integration | Regional scam statistics; public API | Keep analytics and API documentation in the planned architecture, subject to implementation capacity |
| Privacy and usability | Offline checking; dark mode | Treat offline operation and visual themes as later usability improvements |

One response mentioned WhatsApp integration. It is treated as user feedback
about a desired future channel, not as a change to the approved project scope.
The current scope includes the web platform, an SMS companion application,
and a Telegram bot.

## 6. How the Evidence Informs the Project

The survey results support the following decisions:

1. **Web-first MVP:** 70% of respondents were positive or probably positive
   about a web-based checking service, so the web submission flow remains the
   guaranteed core scenario.
2. **Explainable results:** 50% of respondents found identification difficult
   or very difficult, and 25% selected explanations as an important feature.
   Results should therefore include reasons, indicators, and safety advice.
3. **Link and social-engineering signals:** Link checking was selected by 50%
   of respondents, while manipulation analysis was selected by 35%. These
   signals should be represented in the ML and explanation requirements.
4. **External channels:** SMS was selected by 60% and messengers by 50%.
   This supports the normalized ingestion contract and planned Telegram/SMS
   adapters.
5. **Privacy and trust:** 30% were probably not interested and 5% did not
   trust such services. The system should communicate what is stored, avoid
   hidden monitoring, and avoid deleting or blocking original messages.
6. **Controlled scope:** Mobile applications, browser extensions, offline
   analysis, family monitoring, and community reporting are recorded as
   possible future work, not silently added to the current MVP.

## 7. Limitations

- The sample contains only 20 responses and was not selected using a
  probability sampling method.
- The sample is weighted toward respondents aged 18–34.
- The questionnaire measures opinions and self-reported experience, not
  observed behavior.
- The survey does not prove that the ML model will achieve a particular
  accuracy, latency, or fraud-detection rate.
- Multiple-choice answers describe feature interest, not a binding product
  requirement.
- No personal names or individual identifiers are included in this report.

## 8. Traceability

| Survey finding | Related project requirement or document |
|---|---|
| Need to submit suspicious text | FR-1, prototype scenarios |
| Need for classification and indicators | FR-2, FR-3, ML pipeline |
| Interest in link and manipulation analysis | FR-2, FR-3, model feature plan |
| SMS and messenger usage | FR-11, system context, data-flow diagram |
| High-risk warning and safety advice | FR-12, risk policy, notification flow |
| Need for history and feedback | FR-4, FR-7, ER diagram |
| Privacy and trust concerns | NFR-6, privacy policy, architecture decisions |

## 9. Conclusion

The survey provides preliminary evidence that fraudulent messages are a
recognised problem for the target group and that a web service for checking
suspicious text is relevant. The results support the current project
direction: a web-first platform with explainable risk classification,
high-risk warnings, privacy-aware history, and planned SMS/Telegram
ingestion. The evidence does not replace technical testing or model
evaluation; those results must be collected separately during implementation.
