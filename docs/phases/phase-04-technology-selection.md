---
phase: 4
title: Technology selection
status: in progress (candidate list ready; decisions pending the author's study)
last_updated: 2026-11-21
---

# Phase 4: Technology selection

Detailed comparison material: `docs/study/technology-stack-study-guide.pdf`.
Final decisions are recorded as short records in `docs/decisions/` (one per choice, e.g. `adr-001-vector-store.md`) and summarized in section 4.

## 1. Fixed choices (non-negotiable)
| Area | Choice |
|---|---|
| Backend | Spring Boot (Java) |
| Versioning | GitHub |
| API testing | Postman |
| Unit and integration testing | JUnit |

## 2. Evaluation criteria (same for every candidate)
1. Fit with Java and Spring Boot
2. Quality on French and English
3. Performance and scalability (retrieval under 300 ms, many concurrent users)
4. Self-hostable on the enterprise's own servers, ideally offline
5. License and cost (the project may be sold: check license terms)
6. Maturity and maintenance activity
7. Ease of replacement (swappable by configuration)
8. Learning curve for the author

## 3. Areas and shortlists (candidates, not decisions)
| Code | Area | Shortlist |
|---|---|---|
| A | Backend foundation and architecture | Java LTS, Spring Boot, Maven or Gradle, Spring Modulith, MVC with virtual threads or WebFlux, SSE, springdoc-openapi, Resilience4j, Bucket4j |
| B | RAG orchestration | Spring AI, LangChain4j, thin custom layer |
| C | LLM generation | Hosted API (Claude, GPT, Mistral, Gemini) vs self-hosted open models served by vLLM, Ollama, llama.cpp, TGI |
| D | Embedding model | BGE-M3, Qwen3-Embedding, multilingual-e5, Snowflake Arctic-Embed 2.0, Granite multilingual |
| E | Embedding and reranker serving | Hugging Face TEI, Ollama, ONNX in the JVM, vLLM |
| F | Reranker | BGE-reranker-v2-m3, Qwen3-Reranker, Jina reranker, hosted rerank APIs |
| G | Vector store and chunk storage | PostgreSQL + pgvector, Qdrant, OpenSearch or Elasticsearch, Milvus, Weaviate |
| H | Relational database and persistence | PostgreSQL, MariaDB; Spring Data JPA, jOOQ; Flyway, Liquibase |
| I | Document parsing | Apache PDFBox, Apache Tika, Apache POI, Docling, PyMuPDF, Unstructured, Marker |
| J | OCR | Tesseract (Tess4J, OCRmyPDF), PaddleOCR, docTR, EasyOCR, Surya |
| K | Chunking | Custom article-aware chunker, Spring AI TokenTextSplitter, LangChain4j splitters, Docling chunkers |
| L | Caching | Redis or Valkey, Caffeine, pgvector table |
| M | Language detection and text utilities | Lingua, Tika detector, fastText; acronym glossary |
| N | Security | Spring Security + JWT, Keycloak, Argon2 or BCrypt, guardrail classifiers, Presidio or custom PII masking, OWASP Dependency-Check, Trivy, Dependabot, CodeQL |
| O | Frontend | Web Component widget (vanilla TypeScript, Preact, Svelte, React), Vite; console in React, Vue, Angular or Thymeleaf + htmx |
| P | Observability | Micrometer, Prometheus, Grafana, OpenTelemetry, Loki or ELK, Langfuse |
| Q | Analytics and clustering | pgvector similarity grouping, HDBSCAN or BERTopic, Smile; Metabase or Grafana |
| R | Testing and evaluation | JUnit 5, Mockito, AssertJ, Testcontainers, WireMock, Postman + Newman, ArchUnit, JaCoCo, k6 or Gatling or JMeter, Ragas or DeepEval, custom JUnit evaluation harness, promptfoo, Playwright |
| S | Packaging, deployment, CI/CD | Docker, Docker Compose, k3s, Helm, Nginx or Traefik or Caddy, GitHub Actions, Ansible, SonarQube |
| T | Development workflow | IntelliJ IDEA, VS Code, Claude Code, GitHub Copilot, Cursor, Mermaid, PlantUML, draw.io, MkDocs, GitHub Issues + Projects |

## 4. Decisions
| Area | Decision | Reason | Record |
|---|---|---|---|
| Backend | Spring Boot | Fixed | - |
| Versioning | GitHub | Fixed | - |
| API testing | Postman | Fixed | - |
| Unit/integration testing | JUnit | Fixed | - |
| B to T | *(pending the author's study)* | | |

## 5. Study order
1. Highest impact and risk: C (LLM), D and E (embeddings), G (vector store), B (RAG framework).
2. Quality of the knowledge: I, J, K (parsing, OCR, chunking).
3. Operational: L, N, P, S.
4. Productivity and delivery: R, T.

## 6. Notes from the landscape check (November 2026)
- Spring AI 2.0 is generally available (late June 2026) and targets Spring Boot 4.x. Check the Spring Boot version before starting.
- Embedding model popularity: BGE-M3 and Qwen3-Embedding are widely used open multilingual models. Public rankings are not a substitute for spike S3 on the real corpus.
- Most embedding, reranking and OCR tooling is Python-first. Options for a Spring Boot backend: separate inference service called over HTTP, ONNX inside the JVM, or hosted API. All keep modules swappable.
