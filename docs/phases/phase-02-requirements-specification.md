---
phase: 2
title: Requirements specification
status: validated
last_updated: 2026-11-21
---

# Phase 2: Requirements specification

Priority key: **M** = Must, **S** = Should, **C** = Could, **Deferred** = later improvement.
Requirement IDs (`FR-xx`, `NFR-xx`) are stable. Use them in tickets, tests and commit messages.

## 2.1 Decisions recorded
- French and English are both supported (FR-07).
- Follow-up questions are supported (FR-06).
- All questions are stored, with answers, ratings and latency, to detect frequent questions and compute statistics (FR-20 to FR-23).
- Traffic on COLEPS is high: the system must be stateless, horizontally scalable and fast. Initial capacity target is 100 concurrent conversations, to be confirmed by benchmark.
- A human support interface handles out-of-scope questions.
- Answers never depend on the identity or role of the asker.
- Reuse of human answers by the bot is **deferred** (FR-18).
- Delivery of a support answer to the user (FR-19): the user retrieves it in the chat with a reference number. Optional email is an add-on. Live agent handoff is future work.

## 2.2 Functional requirements

### A. Conversation (end users)
| ID | User story | Acceptance criteria | Priority |
|---|---|---|---|
| FR-01 | As a user, I type a question and get an answer. | Free text up to 500 characters, answer shown in the chat window. | M |
| FR-02 | As a user, I get answers based only on official documents. | Each claim is supported by a retrieved passage. | M |
| FR-03 | As a user, I see where the answer comes from. | Document, article or section, page, version label. Citations point to the original French text. | M |
| FR-04 | As a user, I am told when the documents do not contain the answer. | The bot says so and does not invent anything. | M |
| FR-05 | As a user, I see the answer appear progressively. | Streaming starts as soon as generation begins. | M |
| FR-06 | As a user, I ask follow-up questions that depend on earlier ones. | Last N turns kept per session (N to be set). Follow-ups such as "and for foreign companies?" are resolved correctly (tested on a follow-up evaluation set). | M |
| FR-07 | As a user, I use French or English. | The answer comes in the language of the question. Retrieval works across languages over the French corpus. Interface labels are bilingual. | M |
| FR-08 | As a user, I am asked to clarify when my question is ambiguous. | One clarifying question, or a stated assumption. Never based on who the user is. | S |
| FR-09 | As a user, I open the chat from a floating button at the bottom-right. | Works on desktop and mobile. | M |
| FR-10 | As a user, I start a new conversation. | A reset button clears the session context. | M |
| FR-11 | As a user, I rate an answer. | Thumbs up or down, linked to that question and answer. | M |
| FR-12 | As a user, I see example questions when opening the chat. | A few suggestions, drawn later from the most frequent questions. | C |

### B. Out-of-scope handling and human support
| ID | User story | Acceptance criteria | Priority |
|---|---|---|---|
| FR-13 | As a user, when the bot cannot answer, I can send my question to human support. | The bot offers the option. A reference number is shown. | M |
| FR-14 | As a user, I can optionally leave a contact so support can reply. | Optional field, validated format, stored apart from the question. | S |
| FR-15 | As a support agent, I log in to a console listing unanswered questions. | Authenticated access, filters by status, language, date. | M |
| FR-16 | As a support agent, I answer a question and mark it resolved. | Status workflow: new, in progress, answered, closed. | M |
| FR-17 | As a support agent, I see the conversation context and what the bot retrieved. | Full thread displayed with the retrieved passages. | S |
| FR-18 | *(Deferred)* Turn a support answer into a reusable validated answer. | Later improvement. Not part of the current scope. | Deferred |
| FR-19 | As a user, I get the support answer back. | I enter my reference number in the chat and see the answer. Optional email notification as an add-on. | S |

### C. Data, analytics and improvement
| ID | User story | Acceptance criteria | Priority |
|---|---|---|---|
| FR-20 | As the owner, I want every question, answer, language, rating and latency stored. | Stored per interaction with a session identifier. No account needed. | M |
| FR-21 | Personal data is masked before storage. | Emails, phone numbers and similar patterns masked automatically. Retention period configurable. | M |
| FR-22 | As an administrator, I see the most frequent questions. | Semantically similar questions grouped, across both languages. | M |
| FR-23 | As an administrator, I see ratings per question group and over time. | Statistics: volume, satisfaction rate, unanswered rate, top topics, languages. | M |
| FR-24 | As an administrator, I can export the data. | CSV or JSON export. | S |
| FR-25 | Frequent questions are answered faster. | A semantic cache serves validated, well-rated answers without calling the LLM. Entries expire when the source document changes. | S |

### D. Administration and robustness
| ID | User story | Acceptance criteria | Priority |
|---|---|---|---|
| FR-26 | As an administrator, I add or update documents without touching code. | An ingestion command or screen processes documents and reports errors. | M |
| FR-27 | Updating a document causes no downtime and no mix of versions. | Old and new versions never appear together. See the versioning mechanism in Phase 3, section 4.4. | S |
| FR-28 | The bot resists misuse. | Off-topic requests and prompt injection attempts are refused (tested). | M |
| FR-29 | Roles for staff. | At least two roles: administrator and support agent. | M |

## 2.3 Non-functional requirements
| ID | Area | Requirement | Target |
|---|---|---|---|
| NFR-01 | Quality | Correct answers on the evaluation set | >= 85%, measured separately for French and English |
| NFR-02 | Quality | Answers supported by sources | >= 95% |
| NFR-03 | Quality | Out-of-scope questions refused or escalated | >= 90% |
| NFR-04 | Quality | Follow-up questions resolved correctly | >= 85% on a dedicated set |
| NFR-05 | Performance | Time to first token (p95), normal load | < 2 s |
| NFR-06 | Performance | Retrieval stage (query embedding, vector search, rerank), p95 | < 300 ms, vector search itself < 50 ms |
| NFR-07 | Performance | Complete answer (p95) | < 8 s |
| NFR-08 | Capacity | Concurrent conversations on one reference server | Initial target 100, confirmed by benchmark. The real maximum is measured and reported. |
| NFR-09 | Scalability | Growth path | Stateless application, horizontal scaling by adding instances behind a load balancer. LLM inference scales independently. |
| NFR-10 | Overload | Behavior beyond capacity | Bounded queue, clear "busy, please retry" message, no crash, cached answers still served. |
| NFR-11 | Availability | Chatbot failure does not affect the host page | The widget fails silently with a friendly message. |
| NFR-12 | Resilience | LLM or retrieval timeout | Clear error message within 15 s, no infinite spinner. |
| NFR-13 | Security | Transport and secrets | TLS, no secrets in code or images. |
| NFR-14 | Security | Abuse protection | Rate limiting, input size limits, validation. |
| NFR-15 | Security | Staff access | Authentication, role-based access, audit trail of staff actions. |
| NFR-16 | Privacy | Stored data | PII masking, retention limit, notice in the widget, access limited to staff. |
| NFR-17 | Portability | Installation | One command on a clean Linux machine, with offline option. |
| NFR-18 | Modularity | Replaceable parts | LLM, embeddings, vector store and cache each swappable by configuration. |
| NFR-19 | Maintainability | Test coverage of core logic | >= 70% |
| NFR-20 | Observability | Logs and metrics | Structured logs, latency and error metrics per pipeline stage (embed, retrieve, rerank, generate), queue length, cache hit rate. |
| NFR-21 | Usability | Widget | Asynchronous loading, keyboard accessible, readable on mobile. |
| NFR-22 | Compatibility | Browsers | Current versions of Chrome, Firefox, Edge, Safari. |
| NFR-23 | Freshness | Document change visible in answers | Within minutes of ingestion. |

## 2.4 MoSCoW summary
- **Must:** FR-01 to FR-07, FR-09 to FR-11, FR-13, FR-15, FR-16, FR-20 to FR-23, FR-26, FR-28, FR-29.
- **Should:** FR-08, FR-14, FR-17, FR-19, FR-24, FR-25, FR-27.
- **Could:** FR-12.
- **Deferred:** FR-18.

If time runs short, cut from the Should list first. FR-25 (semantic cache) is the first Should item to promote, because it addresses the high-traffic concern directly.

## 2.5 Open parameters
- N, the number of conversation turns kept for follow-ups.
- Retention period for stored interactions.
- Final capacity target after benchmark spike S5.
