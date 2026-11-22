# M09 Human Support: class diagram

Escalation tickets for questions the bot cannot answer: reference numbers, status workflow, replies, retrieval by reference.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class Ticket
    class TicketReference
    class TicketStatus {
        <<enumeration>>
        new
        in_progress
        answered
        closed
    }
    class TicketService
    class TicketRepository
    class ContactInfo
    class ConversationSnapshot
    class PassageSnapshot
    class TicketReply
    class TicketAssignment
    class Notifier

    Ticket *-- TicketReference
    Ticket --> TicketStatus
    Ticket *-- ConversationSnapshot
    ConversationSnapshot "1" *-- "0..*" PassageSnapshot
    Ticket "1" o-- "0..1" ContactInfo : stored apart from the question
    Ticket "1" *-- "0..*" TicketReply
    Ticket "1" *-- "0..*" TicketAssignment
    TicketService --> TicketRepository
    TicketService --> Notifier : optional email add-on
    TicketReply ..> M10_StaffUser : by identifier only
    TicketAssignment ..> M10_StaffUser : by identifier only
    class M10_StaffUser["M10 StaffUser (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M10_StaffUser" external
```

Attributes and methods are added in Phase 5 (architecture and design).
