---
title: Development lifecycle
type: process
status: validated
last_updated: 2026-11-21
---

# Development lifecycle (generic, 12 phases)

Scope: academic internship. All decisions are made by the author; no enterprise sign-off is required. Deployment on "local servers" is simulated on a clean Linux VM or machine.

Each phase has a deliverable. Phase documents live in `docs/phases/`.

| # | Phase | Deliverable | Phase file |
|---|---|---|---|
| 1 | Problem definition and scope | One-page framing | `phase-01-project-framing.md` |
| 2 | Requirements | Requirements specification | `phase-02-requirements-specification.md` |
| 3 | Analysis | Analysis document, diagrams, risk list | `phase-03-analysis.md` |
| 4 | Technology selection | Decision log, validated stack | `phase-04-technology-selection.md` |
| 5 | Architecture and design | Architecture docs, API contracts, data model | `phase-05-architecture-and-design.md` |
| 6 | Engineering foundations | Skeleton with passing CI | `phase-06-engineering-foundations.md` |
| 7 | Implementation | Tested increments | `phase-07-implementation.md` |
| 8 | Testing and quality evaluation | Test and evaluation report | `phase-08-testing-and-evaluation.md` |
| 9 | Packaging and release | Versioned release package | `phase-09-packaging-and-release.md` |
| 10 | Deployment on local servers (simulated) | Running deployment, runbook | `phase-10-deployment.md` |
| 11 | Documentation | Documentation set | `phase-11-documentation.md` |
| 12 | Final deliverables | Report, demo, executive summary | `phase-12-final-deliverables.md` |

## Phase 1: Problem definition and scope
- Write the problem statement, target users and objectives in a few lines.
- Define what is in and out of scope for the internship.
- Set measurable success criteria (answer accuracy, response time, refusal of out-of-scope questions).
- List assumptions, constraints (time, hardware, budget, data) and main risks.

## Phase 2: Requirements
- Functional requirements as user stories with acceptance criteria.
- Non-functional requirements with numbers: latency, concurrent users, language support, availability, security.
- Prioritize with MoSCoW so we know what to cut if time runs short.

## Phase 3: Analysis
- Domain model: actors, use cases, business rules, data entities.
- Main workflows plus failure and edge cases.
- Data analysis: sources, quality, structure, update frequency.
- Feasibility spikes: small, time-boxed experiments on the riskiest assumptions.
- Basic threat and misuse analysis (prompt injection, data leakage, abuse).

## Phase 4: Technology selection
- Define evaluation criteria (fit, performance, cost, maturity, license, on-premise compatibility, own skills).
- Compare candidates, then benchmark finalists on our own data.
- Write a short decision record per choice (context, options, decision, consequences).

## Phase 5: Architecture and design
- Define components, boundaries and responsibilities.
- Specify interfaces before coding (API contracts).
- Design the data model and storage.
- Design for scalability, resilience and observability (logging, metrics).
- Design security: authentication, rate limiting, secrets handling, input validation.
- Keep configuration external to code, so environments differ by configuration only.
- Make UI mockups.

## Phase 6: Engineering foundations
- Repository structure, branching strategy, commit and code conventions.
- Linting, formatting, type checking, pre-commit hooks.
- Environments (dev, test, staging-like).
- CI pipeline from the start: build, tests and scans on every push.
- Backlog and ticket board with small, well-defined tickets.
- Containerized dev setup: the whole project runs with one command.

## Phase 7: Implementation
- Small vertical increments, each linked to a ticket and a pull request (self-review with a checklist, or AI review).
- Module by module against the agreed interfaces, with mocks for dependencies.
- Explicit handling of errors, timeouts and retries.
- Configuration, secrets and business logic kept separate.
- Document as we build: README, module docs, generated API docs.

## Phase 8: Testing and quality evaluation
- Unit, integration and end-to-end tests.
- Performance and load tests against latency and concurrency targets.
- Security checks: dependency scanning, static analysis, input validation, prompt injection tests.
- AI components: an evaluation set of questions with expected answers, measured quantitatively and re-run after every significant change.
- Usability testing with a few people who resemble real bidders.

## Phase 9: Packaging and release
- Semantic versioning and a changelog.
- Reproducible container images and a single deployment definition (e.g. Docker Compose).
- Automated install, upgrade and rollback scripts.
- Configuration templates with sensible defaults.
- Vulnerability scan of images.
- Offline installation path (local servers often have no internet access).

## Phase 10: Deployment on local servers (simulated)
- Prepare a clean Linux VM or machine mimicking an enterprise server, with fixed resources and a restricted network.
- Install using only the documented scripts, to prove the process works.
- Configure TLS, reverse proxy and secrets.
- Integrate the chat widget into a test page imitating the COLEPS site.
- Run smoke tests and a load test on that environment.
- Set up monitoring, centralized logs, automated backups and a tested restore.
- Write a hardware sizing guide based on measurements.

## Phase 11: Documentation
- Installation and administration guide.
- User guide.
- Technical documentation (architecture, conventions, how to extend).
- Operations runbook for common incidents.

## Phase 12: Final deliverables
- Academic report: context, analysis, design, decisions, results, limits, future work.
- Demo, ideally with a short recorded video.
- Executive summary for the board: problem, solution, results, cost and effort to operate.
- Optional: "next steps" section on what a real enterprise rollout would need.

## Cross-cutting activities
- **Ticket tracking and progress notes:** weekly review of what is done and what is late.
- **Traceability:** each requirement links to a design element, a test and a ticket.
- **Security by design:** considered in every phase.
- **Version everything:** code, documents, datasets, configurations.
- **Decision logging:** written justification of choices (see `docs/decisions/`).
