# M07 Semantic Cache: class diagram

Serves frequent questions without calling the LLM. Entries expire when a source document changes.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class SemanticCache
    class CacheEntry
    class SimilarityMatcher
    class CachePolicy
    class CacheInvalidator
    class CacheStats

    SemanticCache "1" *-- "0..*" CacheEntry
    SemanticCache --> SimilarityMatcher
    SemanticCache --> CachePolicy
    SemanticCache --> CacheStats
    CacheInvalidator --> SemanticCache
    CacheEntry ..> M06_DocumentVersion : refers by identifier only
    SemanticCache --> M04_EmbeddingModel : embeds the question
    class M06_DocumentVersion["M06 DocumentVersion (link)"]
    class M04_EmbeddingModel["M04 EmbeddingModel (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M06_DocumentVersion,M04_EmbeddingModel" external
```

Attributes and methods are added in Phase 5 (architecture and design).
