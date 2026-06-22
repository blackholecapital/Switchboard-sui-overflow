# SWITCHBOARD CRM ARCHITECTURE SNAPSHOT

**Snapshot Date:** 2026-06-12
**Build Status:** Active Development (Stage 7+)
**Purpose:** Canonical filesystem and module architecture for replicating the Switchboard CRM ecosystem.

---

# Root

```text
/mnt/eila-hot-sidecar/switchboard-platform
```

---

# Platform Structure

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

# Database Layer

```text
02-databases
└── supabase
    ├── migrations
    ├── schemas
    └── seeds
```

Core schemas:

```text
ai
audit
automation
core
crm
files
graph
marketing
reporting
sales
social
```

---

# Core Services

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

# Retrieval Layer

```text
07-retrieval
├── crm_architecture
├── crm_assets
├── crm_build_runs
├── crm_client_001
├── crm_workflows
├── tools
├── context_seed.json
├── retrieval_sections.json
└── RETRIEVAL_MANIFEST.md
```

---

# Application Layer

```text
06-apps
└── web-crm
    ├── backend
    ├── frontend
    └── shared
```

---

# Frontend Application

```text
frontend/blog-refine-supabase-auth
├── src
│   ├── pages
│   ├── modules
│   ├── components
│   ├── hooks
│   ├── lib
│   └── providers
│
├── .quarantine
└── _quarantine
```

---

# CRM Module Pages

```text
src/pages/modules

admin-users.tsx
assistant.tsx
automation.tsx
billing360.tsx
calendar.tsx
campaigns.tsx
command-center.tsx
comms.tsx
companies.tsx
company360.tsx
contact360.tsx
contacts.tsx
conversations.tsx
data-library.tsx
deal360.tsx
document-preview.tsx
documents.tsx
forecast.tsx
forms.tsx
graph.tsx
invoices.tsx
leads.tsx
notes.tsx
notifications.tsx
pipeline.tsx
reporting.tsx
settings.tsx
social.tsx
tasks.tsx
workspace.tsx
```

---

# 360 Module Contract

All major CRM domains follow this pattern:

```text
module360/
├── cards
├── components
├── filters
├── graph
├── lib
├── sidecars
├── tabs
├── page.tsx
└── index.ts
```

---

# Contacts360

```text
contacts360
├── cards
├── components
├── filters
├── graph
├── lib
├── sidecars
├── tabs
│   ├── PeopleTab
│   ├── CompaniesTab
│   ├── VendorsTab
│   └── PartnersTab
├── page.tsx
└── index.ts
```

---

# Deals360

```text
deals360
├── cards
│   ├── DealTile
│   ├── ForecastTile
│   └── TaskTile
│
├── components
├── filters
├── graph
├── lib
├── sidecars
│
├── tabs
│   ├── PipelineTab
│   ├── LeadsTab
│   ├── ForecastTab
│   └── TasksTab
│
├── page.tsx
└── index.ts
```

---

# Billing360

```text
billing360
├── cards
├── components
├── filters
├── graph
├── lib
├── sidecars
├── tabs
│   ├── BillingTab
│   ├── InvoicesTab
│   ├── PaymentsTab
│   └── EstimatesTab
│
├── payme
│   ├── admin
│   ├── components
│   ├── config
│   ├── lib
│   └── types
│
├── page.tsx
└── index.ts
```

---

# Documents360 (New Standard)

```text
documents360
├── api
├── cards
├── components
│   ├── header
│   ├── modals
│   ├── rightrail
│   └── sidebar
│
├── data
├── filters
├── graph
├── hooks
├── lib
│   ├── retrieval
│   ├── scoring
│   └── mappers
│
├── packs
├── preview
├── sidecars
├── types
│
├── tabs
│   ├── Repository
│   ├── Templates
│   └── Builder
│
├── page.tsx
└── index.ts
```

---

# Shared Media Layer

```text
media
├── avatar
├── company-logo
├── contact-photo
├── upload
├── components
└── lib
```

---

# Shared UI Layer

```text
shared
└── chips
    ├── AvatarChip
    ├── EntityChip
    └── OwnerChip
```

---

# Asset Warehouse

```text
factory-assets
├── components
├── data
└── pages
```

Purpose:

* harvested assets
* templates
* previews
* reusable dashboard parts
* enrichment candidates

---

# Debt Management

Active development debt is never deleted.

Move preservation artifacts into:

```text
.quarantine/
_quarantine/
```

Examples:

```text
*.pre-*
*.before-*
*.bak*
*.broken*
```

---

# Repository Strategy

Documents360 Repository becomes the system of record for:

```text
Assets
Templates
Packs
Favorites
Previews
Retrieval Results
Scoring
Metadata
Promotion Queue
Industry Packs
```

Templates and Builder consume Repository data.

Repository owns document intelligence.

---

# Runtime Status

```text
PostgreSQL     Active
Redis          Active
Graph Layer    Active
LanceDB        Active
Retrieval      Active
CRM Modules    Active
Documents360   Scaffolded
```
