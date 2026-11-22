---
phase: 1
title: Problem definition and scope
status: validated
last_updated: 2026-11-21
---

# Phase 1: Project framing

## 1.1 Context
COLEPS (Cameroon Online E-Procurement System) is the national platform that digitizes the public procurement lifecycle: planning, tender publication, access to tender documents (DAO), bid submission, evaluation, award and monitoring. Its users are bidders, contracting authorities, tender boards (CPM) and the regulator (ARMP). The rules are written in official documents, such as the Code des Marchés Publics and the Manuel du Soumissionnaire, plus many other documents covering the different actors of public procurement.

## 1.2 Problem statement
Users of COLEPS must find answers to procedural questions (registration, PKI certificate, required documents, deadlines, submission, appeals) inside long, dense official documents. Finding the right rule is slow and error-prone, and mistakes can lead to rejected bids or missed deadlines. Nothing on the platform gives a reliable answer at the moment the question arises.

## 1.3 Proposed solution
A chatbot embedded at the bottom-right of the COLEPS platform that answers questions in natural language, strictly from the official documents, and shows the source of each answer. It uses Retrieval-Augmented Generation (RAG).

## 1.4 Target users
**All users of COLEPS are primary users**: bidders, contracting authorities, tender board members, ARMP and administration staff, and visitors. Answers **never depend on who asks**: one shared corpus. A bidder may ask about the ARMP and gets the answer if it is in the documents.

Internal staff (support agents, administrators) are a separate, authenticated user group for the support and administration functions.

Languages: French and English.

## 1.5 Objectives
1. Give accurate answers grounded exclusively in the official documents.
2. Show the source (document, article or section) behind each answer.
3. Refuse politely when the answer is not in the documents, instead of inventing one.
4. Answer fast enough to feel interactive, under high traffic.
5. Be built as separate, replaceable modules, deployable on an enterprise's own servers.

## 1.6 Scope

**In scope**
- Question answering over the official document corpus, in French and English, with follow-up questions.
- Source citations in answers.
- Refusal and redirection of out-of-scope questions, including escalation to human support.
- Storage of all questions, answers and ratings for statistics and frequent-question detection.
- Embeddable chat widget for the COLEPS site.
- Ingestion pipeline with document versioning.
- Support console and administration interface.
- Deployment package and documentation for a local-server installation.
- Quality evaluation framework.

**Out of scope**
- Acting on user accounts or submitting anything on the platform.
- Access to live platform data (current tenders, bid status).
- Legal advice or interpretation beyond what the documents state.
- Authentication or personalized answers for end users.
- Voice interface, mobile app.
- Reuse of human support answers by the bot (deferred, see FR-18 in Phase 2).

## 1.7 Success criteria
| Criterion | Target |
|---|---|
| Answer correctness on the evaluation set | >= 85% |
| Answers supported by retrieved sources (no hallucination) | >= 95% |
| Out-of-scope questions correctly refused or escalated | >= 90% |
| Time to first token | < 2 s |
| Complete answer | < 8 s |
| Concurrent conversations on the reference server | to be set by benchmarks (initial target 100) |
| LLM, embeddings, vector store replaceable without touching other modules | yes |

## 1.8 Assumptions
- The official documents are available in digital form and their usage is permitted. A large and varied set of documents is already available.
- The documents are mostly in French.
- A reference machine or VM can be used to simulate the enterprise server.

## 1.9 Constraints
- Academic internship timeline and a single developer.
- Limited hardware and budget.
- No dependency on a single vendor.
- Fixed technology choices: Spring Boot (backend), GitHub (versioning), Postman and JUnit (testing).

## 1.10 Main risks
| Risk | Mitigation idea |
|---|---|
| Wrong or invented answers on legal and procedural topics | Strict grounding, citations, refusal behavior, evaluation set |
| Slow responses | Offline ingestion, streaming, caching, right-sized models |
| Poor document parsing (tables, scans, article structure) | Early data analysis and parsing spike |
| Documents become outdated | Versioned re-ingestion pipeline |
| Prompt injection or misuse | Input filtering, constrained system prompt, security tests |
| Scope creep | MoSCoW prioritization, strict scope list |
