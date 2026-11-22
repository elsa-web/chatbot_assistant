# M04 Retrieval: class diagram

Finds the most relevant passages fast. Owns the EmbeddingModel interface, which M06 and M07 link to so ingestion and querying use the same model.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class Retriever
    class QueryEmbedder
    class EmbeddingModel {
        <<interface>>
    }
    class VectorSearcher
    class KeywordSearcher
    class HybridMerger
    class Reranker
    class RetrievalFilter
    class SearchResult
    class RetrievalMetrics
    class KnowledgeStoreReader {
        <<interface>>
    }

    Retriever --> QueryEmbedder
    Retriever --> VectorSearcher
    Retriever --> KeywordSearcher
    Retriever --> HybridMerger
    Retriever --> Reranker
    Retriever --> RetrievalFilter
    Retriever --> RetrievalMetrics
    Retriever ..> SearchResult : returns
    QueryEmbedder --> EmbeddingModel
    VectorSearcher --> KnowledgeStoreReader
    KeywordSearcher --> KnowledgeStoreReader
    SearchResult o-- M06_Chunk
    SearchResult o-- M06_ChunkMetadata
    note for EmbeddingModel "One adapter per concrete model, swappable by configuration (chosen in Phase 4)"
    class M06_Chunk["M06 Chunk (link)"]
    class M06_ChunkMetadata["M06 ChunkMetadata (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M06_Chunk,M06_ChunkMetadata" external
```

Attributes and methods are added in Phase 5 (architecture and design).
