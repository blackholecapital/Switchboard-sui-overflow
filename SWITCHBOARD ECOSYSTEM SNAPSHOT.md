# SWITCHBOARD ECOSYSTEM SNAPSHOT

**Date:** 2026-06-12

## Platform Root

```text
/mnt/eila-hot-sidecar/switchboard-platform
```

---

# Ecosystem Layout

```text
switchboard-platform
│
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
│
├── 13-resource-packs          (target)
├── 14-factory-assets          (target)
│
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

Schemas:

```text
core
crm
sales
marketing
files
graph
automation
reporting
social
audit
ai
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
├── module-manifests
├── tools
├── context_seed.json
├── retrieval_sections.json
└── RETRIEVAL_MANIFEST.md
```

---

# LanceDB Runtime

```text
data-volumes
└── lancedb
    ├── crm_architecture
    ├── crm_assets
    ├── crm_build_runs
    ├── crm_client_001
    ├── crm_workflows
    └── switchboard_context.lance
```

---

# Web CRM

```text
06-apps
└── web-crm
    ├── backend
    ├── frontend
    └── shared
```

Frontend Root:

```text
frontend/blog-refine-supabase-auth
```

---

# CRM Module Root

```text
frontend/blog-refine-supabase-auth/src/modules
```

---

# Primary CRM Domains

```text
contacts360
deals360
billing360
documents360
automation360
marketing360
workspace360
comms360
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
├── page.tsx
└── index.ts
```

Tabs:

```text
People
Companies
Vendors
Partners
```

---

# Deals360

```text
deals360
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

Tabs:

```text
Pipeline
Leads
Forecast
Tasks
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
├── payme
├── sidecars
├── tabs
├── page.tsx
└── index.ts
```

PayMe:

```text
admin
components
config
lib
types
```

---

# Documents360

```text
documents360
├── api
├── cards
│   └── repository
├── components
│   ├── header
│   ├── modals
│   ├── repository
│   ├── rightrail
│   └── sidebar
├── data
│   └── seeds
├── filters
├── graph
├── hooks
├── lib
│   ├── retrieval
│   ├── scoring
│   └── mappers
├── packs
│   └── system
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

# Automation360

```text
automation360
├── api
├── cards
├── components
├── data
│   ├── seeds
│   └── templates
├── filters
├── graph
├── hooks
├── lib
│   ├── workflow-engine
│   ├── agents
│   ├── triggers
│   ├── actions
│   ├── conditions
│   ├── execution
│   ├── monitoring
│   ├── integrations
│   └── templates
├── sidecars
├── tabs
├── types
├── builder
│   ├── canvas
│   ├── nodes
│   ├── edges
│   ├── validators
│   └── templates
├── page.tsx
└── index.ts
```

Tabs:

```text
Workflows
Agents
Integrations
Events
Monitoring
```

---

# Marketing360

```text
marketing360
├── api
├── cards
├── components
├── data
│   ├── campaigns
│   ├── content
│   └── audiences
├── filters
├── graph
├── hooks
├── lib
│   ├── campaigns
│   ├── audiences
│   ├── content
│   ├── attribution
│   ├── scoring
│   ├── analytics
│   ├── channels
│   └── recommendations
├── sidecars
├── tabs
├── types
├── content-studio
│   ├── templates
│   ├── assets
│   ├── creatives
│   └── publishing
├── page.tsx
└── index.ts
```

Tabs:

```text
Campaigns
Social Media
Attribution
Analytics
```

---

# Workspace360

```text
workspace360
├── api
├── cards
├── components
├── data
│   ├── seeds
│   └── mock
├── filters
├── graph
├── hooks
├── lib
│   ├── calendar
│   ├── bookings
│   ├── scheduling
│   ├── availability
│   └── integrations
├── sidecars
├── tabs
├── types
├── page.tsx
└── index.ts
```

Tabs:

```text
Calendar
Meetings
Schedule
Bookings
Team
```

---

# Comms360

```text
comms360
├── api
├── cards
├── components
├── data
│   ├── seeds
│   ├── templates
│   └── mock
├── filters
├── graph
├── hooks
├── lib
│   ├── channels
│   ├── conversations
│   ├── messaging
│   ├── email
│   ├── sms
│   ├── calls
│   ├── templates
│   ├── tracking
│   ├── scheduling
│   ├── scoring
│   ├── ai-suggestions
│   └── integrations
├── sidecars
├── tabs
├── types
├── social-bridge
│   ├── accounts
│   ├── handles
│   ├── inbox
│   ├── publishing
│   ├── scheduled-posts
│   ├── webhooks
│   └── auth
├── page.tsx
└── index.ts
```

Tabs:

```text
Conversations
Messages
Campaigns
Social
```

---

# Shared Domains

```text
factory-assets
media
shared
```

Purpose:

```text
Factory Assets  = Asset warehouse
Media           = Uploads / images / avatars
Shared          = Reusable CRM UI primitives
```

---

# Quarantine Policy

```text
frontend/blog-refine-supabase-auth/.quarantine
frontend/_quarantine
```

All:

```text
*.pre-*
*.before-*
*.bak*
*.broken*
```

move here.

---

# Deployment Goal

```text
switchboard-platform
    ↓
self-contained
    ↓
git repository
    ↓
clone
    ↓
deploy
    ↓
multi-tenant SaaS
```

No runtime dependency on development warehouses.

Resource packs and factory assets eventually migrate into the platform tree itself.

```
```
