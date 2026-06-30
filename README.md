# System Architecture Diagram

Knowledge Base & HMIS Chatbot System — Week 1 Deliverable

## High-Level Architecture

```mermaid
flowchart TB
    subgraph Client_KB["KB Web App (Same Origin)"]
        A[React/Next.js Frontend<br/>Dashboard, Editor, Search]
    end

    subgraph Client_HMIS["External HMIS Mockup (Different Origin)"]
        B[HMIS Dashboard Page]
        C["Embedded Chat Widget<br/>(script tag / iframe)"]
        B --> C
    end

    subgraph Backend["Backend API Server (Node.js/Express)"]
        D[Auth Service<br/>JWT issuance/validation]
        E[RBAC Middleware]
        F[Articles API<br/>/api/v1/articles]
        G[Search API<br/>/api/v1/search]
        H[Chat API<br/>/api/v1/chat]
        I[Feedback API<br/>/api/v1/feedback]
        J[Admin/Analytics API<br/>/api/v1/admin]
        K[CORS Middleware<br/>Allowlist origins]
    end

    subgraph Data["Data Layer"]
        L[(PostgreSQL<br/>articles, users, tags,<br/>feedback, search_logs)]
        M[(Search Index<br/>Postgres FTS / MeiliSearch)]
        N[(File Storage<br/>S3 / Local FS)]
        O[(Chat Logs<br/>chat_sessions, chat_messages)]
    end

    A -- "HTTPS + JWT (same-site cookie or bearer token)" --> K
    C -- "HTTPS + CORS preflight + API key/JWT" --> K
    K --> D
    K --> E
    E --> F
    E --> G
    E --> H
    E --> I
    E --> J
    F --> L
    G --> M
    G --> L
    H --> L
    H --> O
    I --> L
    J --> L
    F --> N
```

## CORS & Widget Communication Flow

The chatbot widget is embedded into the external HMIS page (a different origin from the KB backend), so every request from the widget to the backend is a cross-origin request and must pass a CORS preflight check.

```mermaid
sequenceDiagram
    participant U as User (in HMIS)
    participant W as Chat Widget (HMIS origin)
    participant API as KB Backend API
    participant DB as Database

    U->>W: Opens floating widget, types question
    W->>API: OPTIONS /api/v1/chat (CORS preflight)
    API-->>W: 204 + Access-Control-Allow-Origin: hmis.allowed-origin.com
    W->>API: POST /api/v1/chat (Authorization: Bearer <JWT/API key>, body: {message, module_context})
    API->>API: Validate JWT/API key + role
    API->>DB: Query published articles relevant to message
    DB-->>API: Matching articles
    API->>DB: Log chat_message + citations + search_log
    API-->>W: 200 {answer, citations[], confidence}
    W-->>U: Render answer + source links + feedback control
```

## CORS Configuration Plan

- The backend maintains an explicit **allowlist** of permitted origins (e.g., the KB web app's own origin and the HMIS mockup's origin) — no wildcard (`*`) origins for authenticated endpoints.
- Preflight (`OPTIONS`) requests are handled for all `/api/v1/*` routes that the widget calls (`/chat`, `/articles` read endpoints, `/feedback`).
- Allowed methods: `GET, POST, OPTIONS`.
- Allowed headers: `Content-Type, Authorization`.
- Credentials: cross-origin requests from the widget use a bearer token/API key in the `Authorization` header rather than cookies, avoiding the need for `Access-Control-Allow-Credentials` complexity.
- Widget-issued tokens are scoped to **read-only / chat-only** permissions distinct from the full editor/admin session tokens used by the KB web app, so a compromised widget token cannot be used to edit or publish content (supports FR-5.4).

## Authentication Strategy

- **KB Web App (Editors/Admins/Viewers):** JWT-based session auth. On login, the server issues a short-lived access token + refresh token; sessions expire after 8 hours of inactivity (FR-3.5).
- **Embedded Widget:** Uses a separate, scoped API key or widget-specific JWT issued per embedding product (HMIS, lab, pharmacy), tied to a role so the chatbot can enforce FR-5.4 (role-based content visibility).
- **Password storage:** bcrypt hashing, never plaintext.
- **RBAC Middleware:** Every protected route checks the decoded token's `role` claim against an allowlist of permitted roles for that route (Viewer / Editor / Admin) before reaching the controller.
- **Audit Logging:** All admin actions (publish, reject, archive, role assignment) are written to `audit_logs` with who/what/when (FR-3.6).
