# M06 Knowledge Ingestion: class diagram

Offline pipeline: parse, OCR, chunk, embed, index, version, atomic switch. Owns the Chunk and ChunkMetadata contract read by M04.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class Document
    class DocumentVersion
    class VersionStatus {
        <<enumeration>>
        DRAFT
        INDEXING
        VALIDATED
        ACTIVE
        SUPERSEDED
        ARCHIVED
    }
    class DocumentParser {
        <<interface>>
    }
    class PdfParser
    class DocxParser
    class OcrEngine {
        <<interface>>
    }
    class StructuredDocument
    class Section
    class Article
    class Chunker {
        <<interface>>
    }
    class ArticleAwareChunker
    class Chunk
    class ChunkMetadata
    class IndexBuilder
    class KnowledgeStoreWriter {
        <<interface>>
    }
    class VersionManager
    class IngestionJob
    class IngestionReport
    class IngestionError

    Document "1" *-- "1..*" DocumentVersion
    DocumentVersion "1" o-- "0..*" Chunk
    DocumentVersion --> VersionStatus
    Chunk *-- ChunkMetadata
    DocumentParser <|.. PdfParser
    DocumentParser <|.. DocxParser
    PdfParser --> OcrEngine : scanned pages
    DocumentParser ..> StructuredDocument : produces
    StructuredDocument "1" *-- "1..*" Section
    Section "1" *-- "0..*" Article
    Chunker <|.. ArticleAwareChunker
    Chunker ..> Chunk : produces
    IngestionJob --> DocumentParser
    IngestionJob --> Chunker
    IngestionJob --> IndexBuilder
    IngestionJob --> VersionManager
    IngestionJob --> IngestionReport
    IngestionReport "1" *-- "0..*" IngestionError
    IndexBuilder --> KnowledgeStoreWriter
    VersionManager --> KnowledgeStoreWriter : atomic switch
    IndexBuilder --> M04_EmbeddingModel
    VersionManager --> M07_CacheInvalidator
    class M04_EmbeddingModel["M04 EmbeddingModel (link)"]
    class M07_CacheInvalidator["M07 CacheInvalidator (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M04_EmbeddingModel,M07_CacheInvalidator" external
```

Attributes and methods are added in Phase 5 (architecture and design).
