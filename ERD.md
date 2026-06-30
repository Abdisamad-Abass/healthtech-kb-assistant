# Entity Relationship Diagram (ERD)

Knowledge Base & HMIS Chatbot System — Week 1 Deliverable

## Diagram

```mermaid
erDiagram
    USERS ||--o{ ARTICLES : authors
    USERS ||--o{ FEEDBACK : submits
    USERS ||--o{ SEARCH_LOGS : generates
    USERS ||--o{ MEDIA : uploads
    USERS ||--o{ AUDIT_LOGS : performs
    USERS ||--o{ CHAT_SESSIONS : starts

    CATEGORIES ||--o{ ARTICLES : classifies
    CATEGORIES ||--o{ CATEGORIES : "parent_id (self-ref)"

    ARTICLES ||--o{ ARTICLE_TAGS : has
    TAGS ||--o{ ARTICLE_TAGS : has
    ARTICLES ||--o{ FEEDBACK : receives
    ARTICLES ||--o{ MEDIA : contains
    ARTICLES ||--o{ ARTICLE_VERSIONS : "tracked by"
    ARTICLES ||--o{ ARTICLE_LINKS : "cross-links (from)"
    ARTICLES ||--o{ CHAT_MESSAGE_CITATIONS : "cited by"

    CHAT_SESSIONS ||--o{ CHAT_MESSAGES : contains
    CHAT_MESSAGES ||--o{ CHAT_MESSAGE_CITATIONS : cites

    USERS {
        uuid id PK
        string name
        string email
        string password_hash
        string role "admin|editor|viewer"
        string department
        timestamp created_at
    }

    CATEGORIES {
        uuid id PK
        string name
        string slug
        uuid parent_id FK
        string description
        string icon
        int sort_order
    }

    ARTICLES {
        uuid id PK
        string title
        string slug
        text content
        string template_type "How-To|SOP|FAQ|Troubleshooting|Feature Reference|Release Notes"
        uuid category_id FK
        uuid author_id FK
        string status "draft|pending_review|published|rejected|archived"
        string product_version
        timestamp last_reviewed_at
        timestamp created_at
        timestamp updated_at
        int views
    }

    ARTICLE_VERSIONS {
        uuid id PK
        uuid article_id FK
        int version_number
        text content_snapshot
        uuid edited_by FK
        timestamp created_at
    }

    TAGS {
        uuid id PK
        string name
        string slug
    }

    ARTICLE_TAGS {
        uuid article_id FK
        uuid tag_id FK
    }

    ARTICLE_LINKS {
        uuid id PK
        uuid source_article_id FK
        uuid related_article_id FK
    }

    FEEDBACK {
        uuid id PK
        uuid article_id FK
        uuid user_id FK
        int rating "1-5"
        text comment
        timestamp created_at
    }

    MEDIA {
        uuid id PK
        uuid article_id FK
        string filename
        string url
        string type "image|pdf|video"
        uuid uploaded_by FK
        timestamp created_at
    }

    SEARCH_LOGS {
        uuid id PK
        string query
        int results_count
        uuid user_id FK
        timestamp created_at
    }

    AUDIT_LOGS {
        uuid id PK
        uuid user_id FK
        string action
        string entity_type
        uuid entity_id
        timestamp created_at
    }

    CHAT_SESSIONS {
        uuid id PK
        uuid user_id FK
        string source_module "hmis|lab|pharmacy|kb_web"
        timestamp started_at
        timestamp last_active_at
    }

    CHAT_MESSAGES {
        uuid id PK
        uuid session_id FK
        string sender "user|assistant"
        text message
        boolean low_confidence
        boolean was_helpful
        timestamp created_at
    }

    CHAT_MESSAGE_CITATIONS {
        uuid id PK
        uuid message_id FK
        uuid article_id FK
    }
```

## Notes on Schema Decisions (mapping to requirements)

| Table | Requirements Covered |
|---|---|
| `articles` | FR-1.1, FR-1.2, FR-1.6, FR-1.7, FR-1.8 |
| `article_versions` | FR-1.4 (version history + rollback) |
| `article_links` | FR-1.5 (cross-linking related articles) |
| `categories`, `tags`, `article_tags` | FR-1.5, FR-2.3 |
| `users` | FR-3.1–FR-3.4 (RBAC: admin/editor/viewer) |
| `audit_logs` | FR-3.6 (admin action audit trail) |
| `feedback` | FR-4.1, FR-4.4 |
| `search_logs` | FR-2.6 |
| `chat_sessions`, `chat_messages` | FR-5.7 (multi-turn context), FR-5.10 (was this helpful) |
| `chat_message_citations` | FR-5.3 (citation/link to source article) |

`articles.status` includes `pending_review`/`rejected` to support FR-1.3 (admin review/publish/reject) beyond the original draft/published model. `chat_messages.low_confidence` supports FR-5.9 (admin review of low-confidence questions).
