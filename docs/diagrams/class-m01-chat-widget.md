# M01 Chat Widget: class diagram

Embeddable chat interface (frontend). Components rather than server classes.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class ChatWidget
    class LauncherButton
    class ChatWindow
    class MessageList
    class MessageBubble
    class SourceCitationView
    class InputBox
    class FeedbackButtons
    class EscalationForm
    class TicketLookupForm
    class LanguageSelector
    class SessionStore
    class ChatApiClient

    ChatWidget *-- LauncherButton
    ChatWidget *-- ChatWindow
    ChatWindow *-- MessageList
    ChatWindow *-- InputBox
    ChatWindow *-- LanguageSelector
    ChatWindow *-- EscalationForm
    ChatWindow *-- TicketLookupForm
    MessageList "1" *-- "0..*" MessageBubble
    MessageBubble "1" *-- "0..*" SourceCitationView
    MessageBubble "1" *-- "0..1" FeedbackButtons
    ChatWindow --> ChatApiClient
    ChatWindow --> SessionStore
    ChatApiClient --> M02_ChatController : HTTP contract only
    class M02_ChatController["M02 ChatController (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M02_ChatController" external
```

Attributes and methods are added in Phase 5 (architecture and design).
