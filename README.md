# COLEPS Chatbot

A modular Retrieval-Augmented Generation (RAG) chatbot for the **COLEPS** public procurement platform (Cameroon Online E-Procurement System).

The chatbot appears at the bottom-right of the platform and answers questions from all COLEPS users, in **French and English**, based **only on official documents** such as the Code des Marchés Publics and the Manuel du Soumissionnaire. Every answer shows its source. When the documents do not contain the answer, the bot says so and offers to forward the question to human support.

> Internship project. Designed to be robust under high traffic and deployable on an enterprise's own servers.

## Why this project
Procurement rules are spread across long, dense official documents. Finding the right rule is slow and error-prone, and mistakes can lead to rejected bids or missed deadlines. This chatbot gives reliable, sourced answers at the moment the question arises.

## Key features
- Question answering grounded in official documents, with citations (document, article, page, version)
- French and English support, including follow-up questions
- Refusal instead of invention when the answer is not in the documents
- Streaming answers and a fast retrieval pipeline
- Human support console for out-of-scope questions
- Storage of questions and ratings, with frequent-question detection and statistics
- Document ingestion with versioning and safe replacement
- Designed as 12 independent, replaceable modules (LLM, embeddings and vector store are swappable by configuration)

## Architecture at a glance
The system is organized into 12 modules:

| ID | Module | Role |
|---|---|---|
| M01 | Chat Widget | Embeddable chat interface |
| M02 | Chat API | Entry point for end-user traffic |
| M03 | RAG Orchestrator | Turns a question into an answer |
| M04 | Retrieval | Finds relevant passages fast |
| M05 | LLM Gateway | One interface to hosted or local models |
| M06 | Knowledge Ingestion | Turns documents into searchable knowledge |
| M07 | Semantic Cache | Serves frequent questions without the LLM |
| M08 | Feedback and Analytics | Stores interactions, ratings, statistics |
| M09 | Human Support | Handles unanswered questions |
| M10 | Identity and Access | Staff authentication and roles |
| M11 | Staff Console | Interface for support agents and administrators |
| M12 | Platform | Configuration, logging, metrics, deployment |

Details are in [`docs/phases/phase-03-analysis.md`](docs/phases/phase-03-analysis.md).

## Technology
Fixed choices: **Spring Boot** (backend), **GitHub** (versioning), **Postman** and **JUnit** (testing).
All other choices (LLM, embeddings, vector store, parsing, OCR, and so on) are being selected in Phase 4. See [`docs/phases/phase-04-technology-selection.md`](docs/phases/phase-04-technology-selection.md).

## Project status
The project follows a 12-phase lifecycle ([`docs/process/development-lifecycle.md`](docs/process/development-lifecycle.md)).

| Phase | Status |
|---|---|
| 1. Problem definition and scope | Done |
| 2. Requirements | Done |
| 3. Analysis | Done (diagrams in progress) |
| 4. Technology selection | In progress |
| 5. Architecture and design | Not started |
| 6 to 12. Foundations, implementation, testing, packaging, deployment, documentation, final deliverables | Not started |

There is no runnable code yet.

## Documentation
All project documentation lives in [`docs/`](docs/README.md):

```
docs/
├── README.md        index and conventions
├── process/         development lifecycle
├── phases/          one document per phase
├── diagrams/        use case, sequence and class diagrams
├── decisions/       technology decision records
└── study/           technology study guide
```

## Getting started
Instructions will be added when the first runnable version is available.

## Author
KANA MBAKOP LEONCE ELSA, Saint Jean Ingenieur INGE 5 ISI, internship project.

## License
To be decided.
