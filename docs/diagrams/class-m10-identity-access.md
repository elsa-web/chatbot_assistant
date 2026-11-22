# M10 Identity and Access: class diagram

Staff authentication, roles and permissions, audit trail. Other modules call AccessPolicy to check permissions.

```mermaid
classDiagram
    class StaffUser
    class Role
    class Permission
    class Credential
    class AuthService
    class TokenService
    class AccessPolicy
    class PasswordPolicy
    class AuditEntry
    class AuditLogger

    StaffUser *-- Credential
    StaffUser "1" --> "1..*" Role
    Role "1" o-- "1..*" Permission
    AuthService --> StaffUser
    AuthService --> PasswordPolicy
    AuthService --> TokenService
    AccessPolicy --> Role
    AccessPolicy --> Permission
    AuditLogger "1" *-- "0..*" AuditEntry
    note for AccessPolicy "Called by M06, M08, M09 and M11 for permission checks"
```

Attributes and methods are added in Phase 5 (architecture and design).
