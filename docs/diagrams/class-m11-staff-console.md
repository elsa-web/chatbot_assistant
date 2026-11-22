# M11 Staff Console: class diagram

Web interface for support agents and administrators (frontend). Calls the staff endpoints of M06, M08, M09 and M10 over HTTP.

Legend: grey dashed boxes are classes owned by other modules. They are links only and are not redrawn here.

```mermaid
classDiagram
    class ConsoleApp
    class AuthGuard
    class LoginPage
    class TicketListPage
    class TicketDetailPage
    class DocumentsPage
    class IngestionStatusView
    class StatisticsDashboard
    class FrequentQuestionsView
    class ExportPage
    class UserManagementPage
    class ConsoleApiClient

    ConsoleApp --> AuthGuard
    ConsoleApp *-- LoginPage
    ConsoleApp *-- TicketListPage
    ConsoleApp *-- DocumentsPage
    ConsoleApp *-- StatisticsDashboard
    ConsoleApp *-- ExportPage
    ConsoleApp *-- UserManagementPage
    TicketListPage --> TicketDetailPage
    DocumentsPage *-- IngestionStatusView
    StatisticsDashboard *-- FrequentQuestionsView
    LoginPage --> ConsoleApiClient
    TicketListPage --> ConsoleApiClient
    TicketDetailPage --> ConsoleApiClient
    DocumentsPage --> ConsoleApiClient
    StatisticsDashboard --> ConsoleApiClient
    ExportPage --> ConsoleApiClient
    UserManagementPage --> ConsoleApiClient
    ConsoleApiClient --> M09_TicketService : HTTP
    ConsoleApiClient --> M06_IngestionJob : HTTP
    ConsoleApiClient --> M08_StatisticsService : HTTP
    ConsoleApiClient --> M10_AuthService : HTTP
    class M09_TicketService["M09 TicketService (link)"]
    class M06_IngestionJob["M06 IngestionJob (link)"]
    class M08_StatisticsService["M08 StatisticsService (link)"]
    class M10_AuthService["M10 AuthService (link)"]
    classDef external fill:#f2f2f2,stroke:#999999,stroke-dasharray:4 3,color:#555555
    cssClass "M09_TicketService,M06_IngestionJob,M08_StatisticsService,M10_AuthService" external
```

Attributes and methods are added in Phase 5 (architecture and design).
