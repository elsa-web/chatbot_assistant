---
phase: 3
title: Analysis
status: validated (diagrams pending validation; class lists kept outside this file)
last_updated: 2026-11-21
---

# Phase 3: Analysis

The system is built as **12 modules**. Work is organized by module. Diagrams follow these rules:
- Use case diagram: one global diagram.
- Class diagrams: one per module, showing only that module's own classes. A class from another module appears only as a link, never redrawn.
- Sequence diagrams: by scenario. A scenario crossing several modules is drawn once, with the modules as participants.

## 1. Modules

| ID | Module | Purpose |
|---|---|---|
| M01 | Chat Widget (frontend) | Embeddable chat interface on COLEPS |
| M02 | Chat API | Single entry point for end-user traffic |
| M03 | RAG Orchestrator | Turns a question into an answer |
| M04 | Retrieval | Finds the most relevant passages fast |
| M05 | LLM Gateway | Isolates the language model behind one interface |
| M06 | Knowledge Ingestion | Turns official documents into searchable knowledge |
| M07 | Semantic Cache | Serves frequent questions without calling the LLM |
| M08 | Feedback and Analytics | Stores interactions and learns from them |
| M09 | Human Support | Handles questions the bot cannot answer |
| M10 | Identity and Access | Protects staff-facing functions |
| M11 | Staff Console (frontend) | Web interface for support agents and administrators |
| M12 | Platform (cross-cutting) | Configuration, logging, metrics, deployment, backups |

### M01 Chat Widget
- Floating button, open and close, mobile and desktop.
- Message input, streamed answer display, source citations.
- New conversation, example questions, thumbs up and down.
- Interface language (FR/EN).
- "Send to support" action, reference number display, later lookup by reference.
- Fails silently without affecting the host page.
- Covers: FR-01, 05, 07 (labels), 09, 10, 11, 12, 13, 14, 19.

### M02 Chat API
- Session creation and conversation history (last N turns).
- Streaming endpoint.
- Rate limiting, input validation and size limits.
- Overload control (bounded queue, "busy" response).
- Forwards each question to M03 and each interaction to M08.
- Serves feedback submissions and support status lookups.
- Covers: FR-01, 05, 06 (history), 10, NFR-10, 14.

### M03 RAG Orchestrator
- Language detection (FR/EN).
- Follow-up resolution (rewrite a context-dependent question into a standalone one).
- Input guardrails (off-topic, prompt injection).
- Cache check (M07), then retrieval (M04), then generation (M05).
- Grounding check: is the answer supported by the retrieved passages?
- Citation assembly.
- Decision: answered, needs clarification, or out of scope (triggers the escalation offer).
- Covers: FR-02, 03, 04, 06, 07, 08, 28.

### M04 Retrieval
- Query embedding (multilingual, so English questions match French text).
- Vector search, with optional keyword (hybrid) search for article numbers and exact terms.
- Reranking.
- Mandatory filter on active document versions; optional filters on document type.
- Latency measurement per stage.
- Covers: NFR-06, cross-lingual retrieval.

### M05 LLM Gateway
- Uniform interface for hosted APIs and local models (swap by configuration).
- Streaming, timeouts, retries, fallback model.
- Concurrency limits and request queue towards the model.
- Token and latency metrics.
- Covers: NFR-08, 09, 10, 12, 18.

### M06 Knowledge Ingestion (offline pipeline)
- Parse PDF and Word, with structure-aware chunking (articles, sections, tables).
- OCR for scanned documents.
- Metadata extraction (document, article, page, version).
- Embedding and indexing into the knowledge store.
- Versioning with validated, atomic switch (section 4.4).
- Error report and command-line interface.
- Triggers invalidation of the cache (M07).
- Covers: FR-26, 27, NFR-23.

### M07 Semantic Cache (Should)
- Store validated answers keyed by question meaning.
- Similarity lookup with a safe threshold, per language.
- Expiry and invalidation when documents change.
- Hit rate metrics.
- Covers: FR-25.

### M08 Feedback and Analytics
- Store interactions (question, answer, language, rating, latency, session).
- PII masking before storage, retention policy.
- Rating capture.
- Grouping of similar questions (frequent question detection).
- Statistics: volume, satisfaction, unanswered rate, top topics, languages.
- Data export.
- Covers: FR-11 (storage), 20, 21, 22, 23, 24.

### M09 Human Support
- Create an escalation ticket with a reference number.
- Optional contact data stored apart from the question.
- Status workflow: new, in progress, answered, closed.
- Assignment, conversation context and retrieved passages shown to the agent.
- Answer retrieval by reference number (optional email notification).
- Covers: FR-13, 14, 15, 16, 17, 19.

### M10 Identity and Access
- Staff authentication.
- Roles: administrator and support agent.
- Permission checks.
- Audit trail of staff actions.
- Covers: FR-29, NFR-15.

### M11 Staff Console
- Support: ticket list, filters, ticket detail, reply, status change.
- Admin: document upload and ingestion status, user management, statistics dashboard, export, configuration.
- Covers: UI part of FR-15, 16, 17, 23, 24, 26.

### M12 Platform
- Configuration management and secrets.
- Structured logging, metrics and tracing.
- Health checks.
- Deployment packaging (containers), reverse proxy, TLS.
- Backups.
- Covers: NFR-13, 17, 20, 11.

### Module relations
- **Online question path:** M01 -> M02 -> M03 -> (M07, M04, M05); M02 -> M08 records the interaction.
- **Escalation path:** M03 -> M02 -> M09, with M10 and M11 on the staff side.
- **Offline path:** M06 writes the knowledge store that M04 reads. The contract is the chunk and metadata schema (section 4.3).
- **Dependency rules:** frontends talk only to APIs. M04 and M05 never call M03. M08 and M09 never depend on the answer engine.
- **Staff API:** M06, M08, M09 and M10 expose staff endpoints. Proposal: one staff API gateway so authentication and auditing sit in one place (to settle in Phase 5).

### Suggested development order
M12 (skeleton) -> M06 -> M04 -> M05 -> M03 -> M02 -> M01 -> M08 -> M09 + M10 -> M11 -> M07.

## 2. Main workflows
| ID | Workflow | Modules | Diagram status |
|---|---|---|---|
| W1 | Ask a question (new or follow-up) | M01, M02, M03, M07, M04, M05, M08 | Drawn, pending validation |
| W2 | Escalate to human support, staff reply, user retrieves | M01, M02, M03, M09, M10, M11 | Drawn, pending validation |
| W3 | Ingest or update documents | M11 (or CLI), M06, M04 store, M07 | To draw |
| W4 | Give feedback on an answer | M01, M02, M08 | To draw |
| W5 | Review statistics and frequent questions | M11, M10, M08 | To draw |
| W6 | Staff login and role check | M11, M10 | Folded into W2 and W5 |
| W7 | Manage staff accounts | M11, M10 | Short sequence, later |
| W8 | Data retention purge | M08 (scheduled) | Activity note, no diagram |

## 3. Failure and edge cases
| Situation | Expected behavior | Owner |
|---|---|---|
| Question unrelated to procurement | Polite refusal, no LLM generation | M03 |
| Procurement question not covered by the documents | "Not in the documents" plus escalation offer | M03 |
| Ambiguous question | One clarifying question or a stated assumption | M03 |
| Follow-up with a pronoun ("and for foreign companies?") | Rewritten into a standalone question before retrieval | M03 |
| Mixed-language question | Language of the main sentence wins | M03 |
| Retrieved passages weak or contradictory | Low confidence leads to out of scope, or a hedged answer with citations | M03 |
| Cache hit on a document that has since changed | Entry invalidated at ingestion, never served | M07, M06 |
| LLM timeout or provider error | Retry, then fallback model, then a clear error within 15 s | M05, M02 |
| Overload (queue full) | "Busy, please retry"; cached answers still served | M02 |
| Vector store unavailable | Error message, escalation offered, alert raised | M04, M12 |
| Very long, empty or special-character input | Rejected or truncated with a message | M02 |
| User closes the browser mid-stream | Generation cancelled, interaction stored as incomplete | M02, M05 |
| User pastes personal data | Masked before storage | M08 |
| Support ticket never answered | Visible in the "new" queue with age, flagged when overdue | M09 |
| Reference number unknown or guessed | Generic "not found", rate-limited | M02, M09 |
| Ingestion of a corrupt or scanned file | Reported in the ingestion report, previous version stays live | M06 |
| Ingestion fails midway | Atomic switch: no partial version is ever served | M06 |

## 4. Data analysis

### 4.1 Source inventory (to complete once the document set is catalogued)
For each document record: title, issuer, `document_type` (law, decree, order or circular, manual, guide), file format (text PDF, scanned PDF, Word), language, pages, version and date, authority level, whether it is the legally authoritative text.

The corpus is large and varied and covers the different actors of public procurement. There is **one shared corpus**; answers do not depend on the asker.

Note: the COLEPS presentation document explains the *recours* procedure descriptively. The official legal text is the source of truth. The presentation can be used for context and for test-question ideas.

### 4.2 Expected data challenges
| Challenge | Impact | Response |
|---|---|---|
| Scanned PDFs | No text without OCR, with errors | Early text-layer test, OCR where needed |
| Legal structure (titles, chapters, articles, paragraphs) | Chunks cutting an article in half lose meaning | Structure-aware chunking, article identity in metadata |
| Cross-references ("see article X") | A passage may depend on another | Store references in metadata, optionally fetch linked articles |
| Tables (thresholds, deadlines, fees) | Flattened tables become unreadable | Table-aware extraction, headers repeated |
| Numbers and deadlines | A wrong figure is the most harmful error | Retrieval tests on numeric questions, mandatory citations |
| Acronyms (DAO, CPM, ARMP, ANTIC, PPM, RCCM, NIU, CNPS) | Users and documents spell them differently | Glossary used for query expansion |
| Multiple versions or amendments | Contradictory answers | Versioning mechanism (4.4) |
| Documents from different issuers that overlap or conflict | Contradictory answers | `authority_level` metadata; the prompt prefers the higher authority and mentions the conflict (proposal, to confirm) |
| English questions over a French corpus | Vocabulary mismatch | Multilingual embeddings, tested in spike S3 |

### 4.3 Chunk metadata (contract between M06 and M04)
`document_id`, `document_title`, `document_type`, `issuer`, `authority_level`, `version_id`, `version_label`, `effective_date`, `language`, `section_path` (title, chapter), `article_number`, `page_start`, `page_end`, `chunk_type` (text, table, list), `references` (linked article numbers), `embedding_model_id`.

There is no audience or role field.

### 4.4 Document versioning mechanism (logical overwrite)
"Overwrite" means a new version replaces the old one logically. The old one is kept for rollback.

1. **Stable identity.** Each document has a permanent `document_id` (for example `code-marches-publics`), assigned by the administrator at first upload. A new file for the same document is uploaded with a "replace" action against that ID. No matching by file name or by guessing.
2. **Version record.** Each upload creates a `version_id` with version label, effective date, SHA-256 file hash, uploader, ingestion date and status.
3. **Status lifecycle.** `DRAFT -> INDEXING -> VALIDATED -> ACTIVE -> SUPERSEDED -> ARCHIVED`.
4. **One active version per document.** Enforced by a database constraint (unique on `document_id` where status is `ACTIVE`).
5. **Duplicate guard.** A file with the same hash as the active version is rejected as "already active".
6. **Invisible build.** The new version is parsed, chunked and embedded in the background. Its chunks carry the `version_id` and are excluded from search, because retrieval always filters on active versions.
7. **Validation gate before the switch.** A parse report without blocking errors; article and chunk counts within a tolerance of the previous version (otherwise the administrator must confirm); the same embedding model identifier as the index; smoke questions pass.
8. **Atomic switch.** One transaction promotes the new version to `ACTIVE` and demotes the old one to `SUPERSEDED`. Queries resolve the active version at query time, so the change is instantaneous and all-or-nothing.
9. **After the switch.** Cache entries tied to that document are invalidated, and an audit entry is written.
10. **Rollback.** Re-activating the previous version is one action (keep the last 3 superseded versions, proposal).
11. **Cleanup.** Vectors of superseded versions beyond the retention limit are purged.
12. **Traceability.** Citations show version label and effective date. Stored interactions and support tickets keep a text snapshot of the cited passages.
13. **Embedding model change.** Reindex everything as a new index generation, with the same validate-then-switch rule.

**Open point:** an amendment that modifies only some articles of a text. Preferred rule: ingest a consolidated version of the full text whenever one exists. Otherwise ingest the amendment as its own document with a higher authority level and a link to the text it modifies.

### 4.5 Evaluation dataset (a Phase 3 data deliverable)
About 150 to 200 questions with expected answers and expected source articles:
- Straightforward factual questions (the bulk).
- Numeric and deadline questions.
- English questions with the same content as French ones.
- Follow-up chains (two or three turns).
- Out-of-scope and unrelated questions.
- Adversarial questions (prompt injection, requests to ignore the rules).

### 4.6 Update frequency
To be established per document. It sets how often ingestion runs and how aggressively the cache expires.

## 5. Feasibility spikes
Each is time-boxed to one or two days with a pass criterion.

| ID | Question | Method | Pass criterion |
|---|---|---|---|
| S1 | Can we extract the documents cleanly? | Parse with two or three tools, compare article detection, table handling and document type tagging | At least 95% of articles detected with correct numbers |
| S2 | Which chunking strategy retrieves best? | Fixed-size vs article-aware chunks on 30 questions | Higher recall@5 for the chosen strategy, target >= 90% |
| S3 | Does retrieval work across French and English? | English questions against the French index with candidate multilingual embedding models | English recall within 10 points of French |
| S4 | How fast is retrieval? | Measure embed, search and rerank on the real corpus | Retrieval stage under 300 ms at p95 |
| S5 | How much traffic can the LLM layer take? | Load test hosted vs local model: tokens per second, queueing, concurrent streams | A measured concurrency ceiling and first token under 2 s |
| S6 | Can the system refuse reliably? | Out-of-scope and adversarial questions against a prototype prompt | At least 90% correct refusals |
| S7 | Can frequent questions be grouped across languages? | Embed and cluster a sample of questions, check by hand | Paraphrases and FR/EN pairs cluster together |

## 6. Threat and misuse analysis
| Threat | Example | Mitigation | Module |
|---|---|---|---|
| Prompt injection (direct) | "Ignore your rules and..." | Guardrail layer, constrained system prompt, adversarial tests | M03 |
| Prompt injection via documents | Malicious text inside an ingested file | Ingestion restricted to staff, review of files, retrieved text treated as data | M06, M03 |
| Off-topic or abusive use | Using the bot as a free general assistant | Scope classification, refusal, rate limiting | M03, M02 |
| Cost and capacity exhaustion | Scripted flood of requests | Rate limits, queue bounds, per-client quotas | M02, M05 |
| Personal data leakage in logs | Users pasting NIU or emails | PII masking, retention limits, restricted access | M08 |
| Ticket reference guessing | Reading other people's support answers | Long random references, rate-limited lookup | M09, M02 |
| Staff account compromise | Stolen password | Password policy, lockout, audit trail, optional second factor | M10 |
| Cross-site scripting | User input or bot output rendered as HTML | Output encoding, safe rendering, content security policy | M01, M11 |
| Cache poisoning | A bad answer cached and served to many | Cache only grounded, well-rated answers; invalidate on change | M07 |
| Wrong legal or procedural answer | Hallucinated deadline or fee | Mandatory citations, grounding check, disclaimer | M03 |
| Stale information | An old article version served | Version metadata, atomic switch, cache invalidation | M06, M07 |
| Widget abuse from other sites | Third parties embedding the API | Origin checks and CORS, API keys per deployment | M02 |

## 7. Diagrams
Status: drawn in chat, **pending validation**.
- Global use case diagram.
- Sequence W1 "Ask a question".
- Sequence W2 "Escalation to human support".

To produce: sequences W3 (ingestion), W4 (feedback), W5 (statistics), then class diagrams per module, in development order.

Files go in `docs/diagrams/`, using Mermaid text so they render on GitHub. Naming: `usecase-global.md`, `sequence-w01-ask-question.md`, `class-m04-retrieval.md`.
