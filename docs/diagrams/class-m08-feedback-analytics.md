# M08 Feedback and Analytics: class diagram

Stores interactions and ratings with PII masking, groups similar questions, produces statistics and exports.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class Interaction
    class InteractionRecorder
    class PiiMasker
    class Rating
    class RatingService
    class RetentionPolicy
    class RetentionJob
    class QuestionCluster
    class ClusterEngine
    class StatisticsService
    class StatisticsSnapshot
    class ExportService

    Interaction "1" *-- "0..1" Rating
    InteractionRecorder --> PiiMasker
    InteractionRecorder --> Interaction
    RatingService --> Interaction
    RatingService --> Rating
    ClusterEngine --> Interaction
    ClusterEngine ..> QuestionCluster : produces
    QuestionCluster "1" o-- "1..*" Interaction
    StatisticsService --> QuestionCluster
    StatisticsService --> Rating
    StatisticsService --> Interaction
    StatisticsService ..> StatisticsSnapshot : produces
    ExportService --> StatisticsService
    RetentionJob --> RetentionPolicy
    RetentionJob --> Interaction
    Interaction ..> M03_AnswerStatus
    class M03_AnswerStatus["M03 AnswerStatus (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M03_AnswerStatus" external
```

Attributes and methods are added in Phase 5 (architecture and design).
