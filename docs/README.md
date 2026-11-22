# COLEPS chatbot: project documentation

Documentation for the COLEPS RAG chatbot (internship project). Documents follow the 12-phase lifecycle in `process/development-lifecycle.md`.

## Layout
```
docs/
├── README.md                          this index
├── process/
│   └── development-lifecycle.md       the 12 phases and their steps
├── phases/
│   ├── phase-01-project-framing.md
│   ├── phase-02-requirements-specification.md
│   ├── phase-03-analysis.md
│   └── phase-04-technology-selection.md
├── diagrams/                          Mermaid diagrams (use case, sequence, class)
├── decisions/                         one decision record per technology choice (adr-NNN-topic.md)
└── study/
    └── technology-stack-study-guide.pdf
```

## Status
| Phase | File | Status |
|---|---|---|
| 1 | `phases/phase-01-project-framing.md` | Validated |
| 2 | `phases/phase-02-requirements-specification.md` | Validated |
| 3 | `phases/phase-03-analysis.md` | Validated (diagrams pending) |
| 4 | `phases/phase-04-technology-selection.md` | In progress |
| 5 to 12 | not started | |

## Conventions (for developers and AI assistants)
- Files are lowercase kebab-case. Phase files are `phase-NN-topic.md`.
- Stable identifiers: `FR-xx` and `NFR-xx` (requirements), `M01` to `M12` (modules), `W1` to `W8` (workflows), `S1` to `S7` (spikes). Reference them in tickets, tests and commit messages.
- Each file starts with a front-matter block giving phase, status and last update.
- Source of truth order: `phases/` files first, then `decisions/`. If code contradicts a validated phase file, change the document through a recorded decision, not silently in code.
- Only validated content goes into phase files.
