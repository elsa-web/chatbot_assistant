# M12 Platform: class diagram

Cross-cutting services used by every module: configuration, secrets, logging, metrics, tracing, health checks, backups.

```mermaid
classDiagram
    class ConfigProvider
    class SecretsProvider
    class StructuredLogger
    class MetricsRegistry
    class Tracer
    class HealthCheck
    class BackupJob
    note for ConfigProvider "Every module uses ConfigProvider, StructuredLogger and MetricsRegistry"
    note for HealthCheck "Modules with external connections (database, vector store, LLM) register a HealthCheck"
    note for BackupJob "Backs up the stores owned by M06 (knowledge), M08, M09 and M10"
```

Attributes and methods are added in Phase 5 (architecture and design).
