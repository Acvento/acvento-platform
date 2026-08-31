# Architecture

This document describes the baseline platform architecture for Acvento.

```mermaid
flowchart TB
    classDef idp fill:#f2f1ee,stroke:#4d4d4d,stroke-width:1.5px,color:#2d2d2d
    classDef web fill:#dfe5fb,stroke:#5b6ed6,stroke-width:1.5px,color:#1d2b6b
    classDef api fill:#d9f1e8,stroke:#1d8b6c,stroke-width:1.5px,color:#0d493a
    classDef db fill:#f3d9d2,stroke:#d06c4d,stroke-width:1.5px,color:#5c2e20
    classDef region fill:#f5f4f1,stroke:#5a5a5a,stroke-width:1px,color:#2e2e2e

    subgraph region["Azure — West Europe"]
        direction TB
        idp["Identity provider<br/>Auth service"]
    end

    subgraph trust["Authentication & data access"]
        direction LR
        web["Static Web Apps<br/>Next.js frontend"]
        app["Mobile appstore<br/>Flutter.dart frontend"]
        api["App Service<br/>ASP.NET Core API"]
        db["PostgreSQL<br/>Flexible server"]
    end

    idp -->|OIDC / access tokens| api
    web <-->|HTTPS| api
    app <-->|HTTPS| api
    api <-->|Data access / queries| db

    class idp idp
    class web web
    class app app
    class api api
    class db db
    class region region
    class trust region
```

## Components

- Identity provider: handles authentication and issues tokens for the application. This will be a third party Auth service.
- Static Web Apps: hosts the Next.js frontend and serves the user-facing experience.
- App Service: runs the ASP.NET Core API and validates authenticated requests.
- PostgreSQL Flexible Server: stores application data in a managed relational database.

