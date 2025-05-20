```mermaid
graph TD

    %% Web アプリケーション層
    subgraph WebApps[Web アプリケーション]
        USER_WEB[aiila-user-web<br>Next.js (UI)] --> USER_API
        ADMIN_WEB[aiila-admin-web<br>Next.js or FastAPI (UI)] --> ADMIN_API
    end

    %% API 層
    subgraph APIs[API サーバー]
        USER_API[aiila-user-api<br>FastAPI] --> DOMAIN
        ADMIN_API[aiila-admin-api<br>FastAPI] --> DOMAIN
    end

    %% バックグラウンド・非同期処理
    subgraph Functions[Functions / 非同期処理]
        FUNCTIONS[aiila-functions] --> DOMAIN
    end

    %% AI 関連処理
    subgraph AI[AI 処理系]
        EMBEDDING[aiila-embedding] --> DOMAIN
        AI_MODEL[aiila-ai-model] --> DOMAIN
    end

    %% ドメインモデル
    subgraph DOMAIN["aiila-domain-model<br>ドメイン層（Python パッケージ）"]
        DM_Assistant[Assistant]
        DM_Document[Document]
        DM_User[User / Operator]
        DM_Role[Role / Permission]

        DM_Assistant --> ORM
        DM_Document --> ORM
        DM_User --> ORM
        DM_Role --> ORM
    end

    %% ORM
    subgraph ORM["SQLAlchemy ORM"]
        Session[Session]
        Base[Declarative Base]
        RelQuery[Relationship / Query]
    end

    ORM --> DB

    %% データベース
    subgraph DB["Cloud SQL (PostgreSQL)"]
        DB_Assistants[assistants]
        DB_Documents[documents]
        DB_Users[users / operators]
        DB_Roles[roles / permissions]
    end

    %% CI/CD
    subgraph CICD["CI/CD パイプライン（GitHub Actions / Cloud Build）"]
        GitHub[GitHub Repo] --> BuildDeploy[Build / Test / Deploy]
        BuildDeploy --> |Deploy| USER_WEB
        BuildDeploy --> |Deploy| ADMIN_WEB
        BuildDeploy --> |Deploy| USER_API
        BuildDeploy --> |Deploy| ADMIN_API
        BuildDeploy --> |Deploy| FUNCTIONS
        BuildDeploy --> |Deploy| EMBEDDING
        BuildDeploy --> |Deploy| AI_MODEL
    end

    %% ログ・監視
    subgraph MONITOR["ログ・監視 (Cloud Logging / Error Reporting / Monitoring)"]
        USER_API --> Logs[ログ出力]
        ADMIN_API --> Logs
        FUNCTIONS --> Logs
        EMBEDDING --> Logs
        AI_MODEL --> Logs

        Logs --> Monitoring[Cloud Monitoring]
        Logs --> ErrorReport[Error Reporting]
    end

    %% ドメインモデルと ORM 関係
    DOMAIN --> ORM

    %% ORM と DB マッピング
    Session --> DB_Assistants
    Session --> DB_Documents
    Session --> DB_Users
    Session --> DB_Roles

    %% スタイル定義
    classDef core fill:#ffe0b2,stroke:#d35400,stroke-width:2px
    class DOMAIN,DM_Assistant,DM_Document,DM_User,DM_Role core

    classDef orm fill:#f0f0f0,stroke:#888888,stroke-dasharray: 5 5
    class ORM,Session,Base,RelQuery orm

    classDef db fill:#e0f7fa,stroke:#00796b
    class DB,DB_Assistants,DB_Documents,DB_Users,DB_Roles db

    classDef api fill:#fce4ec,stroke:#ad1457
    class USER_API,ADMIN_API api

    classDef web fill:#e8f5e9,stroke:#2e7d32
    class USER_WEB,ADMIN_WEB web

    classDef ai fill:#ede7f6,stroke:#512da8
    class EMBEDDING,AI_MODEL ai

    classDef fn fill:#f3e5f5,stroke:#6a1b9a
    class FUNCTIONS fn

    classDef cicd fill:#e3f2fd,stroke:#1565c0
    class CICD,GitHub,BuildDeploy cicd

    classDef monitor fill:#fbe9e7,stroke:#bf360c
    class MONITOR,Logs,Monitoring,ErrorReport monitor
```
