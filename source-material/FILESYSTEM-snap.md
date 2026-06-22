# SWITCHBOARD PLATFORM FILESYSTEM SNAPSHOT

**Snapshot Date:** 2026-06-12
**Purpose:** Canonical filesystem map for cloning, rebuilding, auditing, or onboarding additional Switchboard instances.

---

# PLATFORM ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform
```

Everything below this root belongs to the Switchboard ecosystem.

---

# COMPLETE PLATFORM TREE

```text
/mnt/eila-hot-sidecar/switchboard-platform
│
├── 00-admin
│
├── 01-config
│   ├── docker
│   ├── env
│   │   ├── .env
│   │   └── .env.example
│   └── supabase
│
├── 02-databases
│   ├── backups
│   ├── exports
│   ├── graph
│   └── supabase
│       ├── migrations
│       ├── schemas
│       └── seeds
│
├── 03-core
│   ├── ai-assistant
│   ├── audit
│   ├── auth
│   ├── events
│   ├── files
│   ├── graph
│   ├── notifications
│   ├── permissions
│   ├── retrieval
│   ├── roles
│   ├── tenants
│   └── users
│
├── 04-modules
│
├── 05-plugins
│
├── 06-apps
│   └── web-crm
│       ├── backend
│       ├── frontend
│       └── shared
│
├── 07-retrieval
│   ├── crm_architecture
│   ├── crm_assets
│   ├── crm_build_runs
│   ├── crm_client_001
│   ├── crm_workflows
│   ├── tools
│   ├── context_seed.json
│   ├── retrieval_sections.json
│   └── RETRIEVAL_MANIFEST.md
│
├── 08-client-data
│
├── 09-build-runs
│
├── 10-deploy
│   ├── cloudflare
│   ├── podman
│   ├── scripts
│   ├── supabase
│   └── workers
│
├── 11-docs
│
├── 12-tests
│
├── assets
│
├── data-volumes
│   ├── backups
│   ├── lancedb
│   ├── postgres
│   ├── redis
│   └── supabase
│
└── product-exports
```

---

# WEB CRM ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm
```

---

# FRONTEND ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth
```

---

# CRM SOURCE ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth/src
```

---

# CRM MODULE ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/modules
```

Current modules:

```text
billing360
contacts360
deals360
documents360
factory-assets
media
shared
```

---

# CRM PAGE ROOT

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/pages/modules
```

Major pages:

```text
contacts.tsx
contact360.tsx
companies.tsx
company360.tsx
pipeline.tsx
deal360.tsx
documents.tsx
billing360.tsx
forecast.tsx
reporting.tsx
assistant.tsx
automation.tsx
workspace.tsx
runtimec/*
```

---

# DOCUMENTS360 ROOT

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/modules/documents360
```

```text
documents360
├── api
├── cards
├── components
│   ├── header
│   ├── modals
│   ├── rightrail
│   └── sidebar
├── data
├── filters
├── graph
├── hooks
├── lib
│   ├── retrieval
│   ├── scoring
│   └── mappers
├── packs
├── preview
├── sidecars
├── tabs
│   ├── Repository
│   ├── Templates
│   └── Builder
├── types
├── page.tsx
└── index.ts
```

---

# RETRIEVAL ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/07-retrieval
```

Collections:

```text
crm_architecture
crm_assets
crm_build_runs
crm_client_001
crm_workflows
```

Tooling:

```text
07-retrieval/tools
├── build_switchboard_lancedb.cjs
├── retrieve_switchboard_context.cjs
├── package.json
└── node_modules
```

---

# LANCEDB ROOT

```text
/mnt/eila-hot-sidecar/switchboard-platform/data-volumes/lancedb
```

Collections:

```text
crm_architecture
crm_assets
crm_build_runs
crm_client_001
crm_workflows
switchboard_context.lance
```

---

# TEMPLATE WAREHOUSE ROOT

Primary warehouse:

```text
/07-retrieval/crm_assets
```

Indexed assets:

```text
templates
components
dashboard fragments
ui blocks
cards
layouts
industry packs
factory assets
```

Frontend asset browser:

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/modules/factory-assets
```

```text
factory-assets
├── components
├── data
└── pages
```

Purpose:

```text
Preview Assets
Review Assets
Promote Assets
Score Assets
Import Assets
```

---

# RUNTIME-C ROOT

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/pages/modules/runtimec
```

Contains:

```text
RuntimeLibraryPage
RuntimePreviewModal
RuntimePackCard
runtime-library.json
runtime-registry.ts
runtime-preview-map.ts
runtime-finalists.ts
```

Purpose:

```text
Runtime Preview System
Pack Evaluation
Asset Selection
Template Review
Candidate Promotion
```

---

# MEDIA ROOT

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/src/modules/media
```

Contains:

```text
avatar
company-logo
contact-photo
upload
components
lib
```

---

# DATABASE ROOT

```text
/02-databases/supabase
```

Schemas:

```text
core
crm
files
graph
sales
marketing
reporting
social
automation
audit
ai
```

---

# GRAPH ROOT

```text
/03-core/graph
```

Database schema:

```text
/02-databases/supabase/schemas/graph.sql
```

---

# FILE STORAGE ROOT

```text
/03-core/files
```

Database schema:

```text
/02-databases/supabase/schemas/files.sql
```

---

# ACTIVE QUARANTINE ROOTS

Development debt:

```text
/06-apps/web-crm/frontend/blog-refine-supabase-auth/.quarantine
```

Archive debt:

```text
/06-apps/web-crm/frontend/_quarantine
```

---

# PLATFORM OF RECORD

If a future instance needs to understand Switchboard, these are the only roots that matter:

```text
PLATFORM
└── /mnt/eila-hot-sidecar/switchboard-platform

DATABASE
└── /02-databases

CORE SERVICES
└── /03-core

APPLICATIONS
└── /06-apps

RETRIEVAL
└── /07-retrieval

LANCEDB
└── /data-volumes/lancedb

WEB CRM
└── /06-apps/web-crm

MODULES
└── /frontend/blog-refine-supabase-auth/src/modules

PAGES
└── /frontend/blog-refine-supabase-auth/src/pages/modules

FACTORY ASSETS
└── /src/modules/factory-assets

DOCUMENTS360
└── /src/modules/documents360
```
