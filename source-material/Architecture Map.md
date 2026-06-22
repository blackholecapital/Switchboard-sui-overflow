# Switchboard Platform Architecture Map

**Current Known Structure**

---

# Platform Root

```text
switchboard-platform/
```

The platform consists of multiple layers.

---

# 01. CRM Application Layer

```text
06-apps/
└── web-crm/
```

Purpose:

* Main user application
* CRM user interface
* Business workflows
* Operational dashboards

Contains:

```text
frontend/
backend/
storage/
```

---

## Frontend

```text
frontend/blog-refine-supabase-auth/
```

Purpose:

* React application
* CRM interface
* Sidecars
* Dashboards

### Major Modules

```text
contacts.tsx
contact360.tsx

companies.tsx
company360.tsx

pipeline.tsx
deal360.tsx

tasks.tsx
calendar.tsx

documents.tsx

automation.tsx

reporting.tsx

assistant.tsx

graph.tsx
```

---

## Shared UI Layer

```text
components/
```

Contains:

### Shell

```text
Header
Sidebar
Navigation
Top Bar
```

### Sidecars

```text
Contact Sidecar
User Sidecar
AI Sidecar
Future Entity Sidecars
```

### CRM Components

```text
Tables
Cards
Forms
Activity Streams
Relationship Views
```

---

# 02. Database Layer

```text
02-databases/
```

Primary system of record.

---

## Supabase

```text
02-databases/supabase/
```

Contains:

### Migrations

```text
migrations/
```

### Policies

```text
RLS
Tenant Security
Permissions
```

### Graph Tables

```sql
graph.graph_nodes
graph.graph_edges
```

Purpose:

* Relationship storage
* Entity linking
* AI context graph

---

# 03. Runtime Engine Layer

```text
03-core/
```

Purpose:

* Platform services
* Runtime orchestration
* Shared infrastructure

---

## EILA OS

```text
03-core/eilaos-switchboard/
```

Contains:

### Runtime Graph

```text
factory_graph_nodes
factory_graph_edges
```

Purpose:

* Runtime orchestration
* System relationships
* Process graphing

---

# 04. Retrieval Layer

```text
07-retrieval/
```

Purpose:

* AI memory
* Semantic retrieval
* Context assembly

---

## LanceDB

Collections identified:

```text
crm_architecture
crm_assets
crm_build_runs
crm_workflows
switchboard_context
```

Purpose:

* Semantic search
* Knowledge retrieval
* Context injection

---

# 05. AI Layer

```text
assistant/
ai/
retrieval/
```

Purpose:

* CRM Assistant
* Voice interactions
* Retrieval-augmented actions
* Workflow assistance

---

# 06. Graph Layer

Logical architecture:

```text
Contacts
Companies
Deals
Tasks
Documents
Workflows
```

Stored as:

```text
Nodes
Edges
Relationships
```

Purpose:

* Entity intelligence
* Relationship mapping
* Future graph analytics

---

# 07. Automation Layer

```text
automation/
```

Purpose:

* Workflow execution
* Trigger handling
* Notifications
* Scheduled actions

---

# 08. Reporting Layer

```text
reporting/
```

Purpose:

* KPIs
* Dashboards
* Revenue
* Pipeline analytics

---

# Data Flow

```text
User
  ↓

Frontend
  ↓

CRM API
  ↓

Database
  ↓

Graph Layer
  ↓

Retrieval Layer
  ↓

AI Assistant
```

---

# Current Technical Debt

Identified:

* Multiple backup files
* Multiple restore snapshots
* Legacy experimental modules
* Duplicate graph implementations
* Partial relationship wiring

Recommendation:

1. Freeze UI shell
2. Verify API routes
3. Verify database mappings
4. Wire graph persistence
5. Enable audit logging
6. Consolidate backups
7. Complete remaining sidecars
8. Build final dashboards

---

# End-State Vision

```text
Switchboard CRM

CRM
+
Graph Database
+
Automation Engine
+
Retrieval System
+
AI Assistant
+
Executive Reporting

=
Unified Business Operating System
```
