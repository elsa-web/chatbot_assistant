# M02 Chat API: class diagram

Single entry point for end-user traffic: sessions, validation, rate limiting, streaming, overload control.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class ChatController
    class FeedbackController
    class EscalationController
    class TicketLookupController
    class SessionManager
    class Session
    class ConversationTurn
    class InputValidator
    class RateLimiter
    class RequestQueue
    class StreamingResponse
    class OriginPolicy

    SessionManager "1" *-- "0..*" Session
    Session "1" *-- "0..*" ConversationTurn
    ChatController --> InputValidator
    ChatController --> RateLimiter
    ChatController --> RequestQueue
    ChatController --> SessionManager
    ChatController --> StreamingResponse
    FeedbackController --> RateLimiter
    EscalationController --> RateLimiter
    TicketLookupController --> RateLimiter
    ChatController --> M03_AnswerPipeline
    ChatController --> M08_InteractionRecorder
    FeedbackController --> M08_RatingService
    EscalationController --> M09_TicketService
    TicketLookupController --> M09_TicketService
    note for OriginPolicy "Applied to all controllers (origin checks and CORS)"
    class M03_AnswerPipeline["M03 AnswerPipeline (link)"]
    class M08_InteractionRecorder["M08 InteractionRecorder (link)"]
    class M08_RatingService["M08 RatingService (link)"]
    class M09_TicketService["M09 TicketService (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M03_AnswerPipeline,M08_InteractionRecorder,M08_RatingService,M09_TicketService" external
```

Attributes and methods are added in Phase 5 (architecture and design).
