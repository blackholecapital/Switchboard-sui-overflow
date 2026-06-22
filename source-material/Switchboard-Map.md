# Switchboard Platform Filesystem Map

Root:

```text
/mnt/eila-hot-sidecar/switchboard-platform
```

## Top Level

```text
switchboard-platform
├── 00-admin
├── 01-config
├── 02-databases
├── 03-core
├── 04-modules
├── 05-plugins
├── 06-apps
├── 07-retrieval
├── 08-client-data
├── 09-build-runs
├── 10-deploy
├── 11-docs
├── 12-tests
├── assets
├── data-volumes
└── product-exports
```

---

## Configuration

```text
01-config
├── docker
├── env
│   ├── .env
│   └── .env.example
└── supabase
```

---

## Database Layer

```text
02-databases
├── backups
├── exports
├── graph
└── supabase
    ├── migrations
    │   ├── 0001_create_schemas.sql
    │   ├── 0002_core_identity.sql
    │   ├── 0003_crm_core.sql
    │   ├── 0004_activity_tasks_files.sql
    │   ├── 0005_audit_graph.sql
    │   ├── 0006_stage3_missing_tables.sql
    │   ├── 0007_security_rls_base.sql
    │   ├── 0008_tenant_rls_policies.sql
    │   ├── 0009_app_role.sql
    │   └── 0010_workflow_kernel.sql
    │
    ├── schemas
    │   ├── ai.sql
    │   ├── audit.sql
    │   ├── automation.sql
    │   ├── core.sql
    │   ├── crm.sql
    │   ├── files.sql
    │   ├── graph.sql
    │   ├── marketing.sql
    │   ├── reporting.sql
    │   ├── sales.sql
    │   └── social.sql
    │
    └── seeds
        ├── 0001_bootstrap.sql
        ├── 0002_pipeline_stages.sql
        ├── 0003_demo_crm_records.sql
        ├── 0004_demo_graph.sql
        ├── 0005_permissions.sql
        ├── 0006_admin_user.sql
        ├── 0007_audit_test.sql
        ├── 0008_graph_event_test.sql
        └── 0009_graph_relationships.sql
```

---

## Core Services

```text
03-core
├── ai-assistant
├── audit
├── auth
├── events
├── files
├── graph
├── notifications
├── permissions
├── retrieval
├── roles
├── tenants
└── users
```

---

## Applications

```text
06-apps
└── web-crm
    ├── backend
    ├── frontend
    └── shared
```

---

## Retrieval Layer

```text
07-retrieval
├── crm_architecture
│   └── seed
│       └── docs_index.txt
│
├── crm_assets
│   └── seed_index.txt
│
├── crm_build_runs
│   └── seed
│       └── db_build_index.txt
│
├── crm_client_001
│   └── seed
│       └── client_docs_index.txt
│
├── crm_workflows
│   └── seed_index.txt
│
├── tools
│   ├── build_switchboard_lancedb.cjs
│   ├── retrieve_switchboard_context.cjs
│   ├── package.json
│   └── node_modules/
│
├── context_seed.json
├── LANCEDB_SOURCES.md
├── README.md
├── RETRIEVAL_MANIFEST.md
└── retrieval_sections.json
```

---

## Runtime Storage

```text
data-volumes
├── backups
├── lancedb
│   ├── crm_architecture
│   ├── crm_assets
│   ├── crm_build_runs
│   ├── crm_client_001
│   ├── crm_workflows
│   └── switchboard_context.lance
│
├── postgres
├── redis
└── supabase
```

---

## Deployment

```text
10-deploy
├── cloudflare
├── podman
│   ├── docker-compose.yml
│   └── README.md
├── scripts
├── supabase
└── workers
```

---

## Documentation

```text
11-docs
├── CRM_BUILD_WORKFLOW.md
├── CRM_DATABASE_MAP.md
├── CRM_FILESYSTEM_TREE.md
├── CRM_MASTER_PLAN.md
├── CRM_MODULE_MAP.md
├── CRM_PLUGIN_MAP.md
├── CRM_SECURITY_MAP.md
└── CRM_AI_MAP.md   (planned Stage 7)
```

---

## Current Completed Stages

```text
Stage 1  ✓ Isolation
Stage 2  ✓ Bootstrap
Stage 3  ✓ Database Foundation
Stage 4  ✓ Security + RLS
Stage 5  ✓ Graph Layer
Stage 6  ✓ LanceDB Retrieval Layer
Stage 7  → AI Assistant Clone (next)
```

## Runtime Status

```text
PostgreSQL 16      RUNNING
Redis 7            RUNNING
Podman             RUNNING
LanceDB            ACTIVE
Graph Layer        ACTIVE
RLS Policies       ACTIVE
Retrieval Index    ACTIVE (225 records)
```
