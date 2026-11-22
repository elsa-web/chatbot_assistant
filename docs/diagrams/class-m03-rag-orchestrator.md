# M03 RAG Orchestrator: class diagram

Turns a question into an answer: language, rewriting, guardrails, cache, retrieval, generation, grounding, citations, decision.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class AnswerPipeline
    class QuestionContext
    class LanguageDetector
    class QueryRewriter
    class InputGuardrail
    class PromptBuilder
    class GroundingVerifier
    class CitationAssembler
    class DecisionPolicy
    class AnswerResult
    class PipelineTrace
    class AnswerStatus {
        <<enumeration>>
        answered
        needs_clarification
        out_of_scope
        blocked
        error
    }
    class Language {
        <<enumeration>>
        FR
        EN
    }

    AnswerPipeline --> LanguageDetector
    AnswerPipeline --> QueryRewriter
    AnswerPipeline --> InputGuardrail
    AnswerPipeline --> PromptBuilder
    AnswerPipeline --> GroundingVerifier
    AnswerPipeline --> CitationAssembler
    AnswerPipeline --> DecisionPolicy
    AnswerPipeline --> QuestionContext
    AnswerPipeline --> AnswerResult
    AnswerPipeline *-- PipelineTrace
    AnswerResult --> AnswerStatus
    LanguageDetector --> Language
    AnswerResult "1" o-- "0..*" M04_SearchResult : sources
    AnswerPipeline --> M07_SemanticCache
    AnswerPipeline --> M04_Retriever
    AnswerPipeline --> M05_LlmClient
    QueryRewriter --> M05_LlmClient
    class M04_SearchResult["M04 SearchResult (link)"]
    class M07_SemanticCache["M07 SemanticCache (link)"]
    class M04_Retriever["M04 Retriever (link)"]
    class M05_LlmClient["M05 LlmClient (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M04_SearchResult,M07_SemanticCache,M04_Retriever,M05_LlmClient" external
```

Attributes and methods are added in Phase 5 (architecture and design).
