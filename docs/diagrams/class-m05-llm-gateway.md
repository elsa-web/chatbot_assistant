# M05 LLM Gateway: class diagram

Isolates the language model behind one interface: hosted or local, streaming, retries, fallback, concurrency limits.

```mermaid
classDiagram
    class LlmClient {
        <<interface>>
    }
    class HostedLlmProvider
    class LocalLlmProvider
    class ModelRouter
    class LlmRequest
    class LlmResponse
    class TokenStream
    class ConcurrencyLimiter
    class RetryPolicy
    class UsageMetrics

    LlmClient <|.. HostedLlmProvider
    LlmClient <|.. LocalLlmProvider
    ModelRouter "1" o-- "1..*" LlmClient : primary and fallback
    ModelRouter --> ConcurrencyLimiter
    ModelRouter --> RetryPolicy
    ModelRouter --> UsageMetrics
    LlmClient --> LlmRequest
    LlmClient ..> LlmResponse
    LlmClient ..> TokenStream
```

Attributes and methods are added in Phase 5 (architecture and design).
