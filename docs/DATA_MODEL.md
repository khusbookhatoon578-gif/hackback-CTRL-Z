### File 4: `docs/DATA_MODEL.md`

```markdown
# DATA_MODEL.md

## 1. Entity-Relationship Diagram (Mermaid)

```mermaid
erDiagram
    USER ||--o{ RESOURCE : "owns"
    USER {
        string id PK
        string email UK
        string passwordHash
        string role
        datetime createdAt
    }
    RESOURCE {
        string id PK
        string title
        string description
        string userId FK
        datetime createdAt
    }