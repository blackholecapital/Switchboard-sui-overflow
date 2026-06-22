# Switchboard CRM Repository Map Amendment

## Newly Confirmed Runtime Services

Date: June 2026

---

# Backend Route Layer

Confirmed Route Modules

```text
backend/routes/
```

Contains operational service endpoints.

---

# Core CRM Route

File

```text
core-crm.js
```

Responsibilities

* Contacts CRUD
* Companies CRUD
* Tasks CRUD
* Workflow triggering
* Activity generation
* Audit generation
* Graph event generation

---

# Graph360 Route

File

```text
graph360.js
```

Responsibilities

* Graph relationship queries
* Contact360 assembly
* Company360 assembly
* Relationship retrieval

---

# Assistant Route

File

```text
assistant.js
```

Responsibilities

* AI chat endpoint
* CRM search retrieval
* Context assembly
* Ollama integration
* Qwen integration

---

# Retrieval Route

File

```text
retrieval.js
```

Responsibilities

* CRM indexing
* Search
* Ranking
* Retrieval APIs

---

# CRM Index Sources

Current Reindex Pipeline

```text
crm.contacts
crm.companies
sales.deals
crm.tasks
billing.invoices
files.record_files
```

Feeds:

```text
retrieval.crm_chunks
```

---

# AI Request Path

Current Flow

```text
User
 ↓

AI Sidecar
 ↓

assistant.js
 ↓

retrieval.crm_chunks
 ↓

Ollama
 ↓

Qwen 30B
 ↓

Response
```

---

# Contact Record Lifecycle

Current Flow

```text
UI
 ↓

POST /api/contacts
 ↓

crm.contacts
 ↓

graph.graph_nodes
 ↓

graph.graph_events
 ↓

crm.activities
 ↓

automation.workflows
```

---

# Company Record Lifecycle

Current Flow

```text
UI
 ↓

POST /api/companies
 ↓

crm.companies
 ↓

graph.graph_nodes
 ↓

graph.graph_events
 ↓

crm.activities
```

---

# Relationship Future State

Current

```text
Nodes
```

Automatic

Future

```text
Edges
```

Automatic

Target

```text
CRM
 ↓

Graph Nodes
 ↓

Graph Edges
 ↓

360 Views
 ↓

AI Retrieval
```

---

# Development Debt Inventory

Confirmed Categories

```text
Backup Files
Restore Files
Experimental Variants
UI Iterations
Graph Duplicates
```

Recommendation

Do not delete.

Create:

```text
/archive
```

and move historical artifacts after final shell stabilization.

---

# Current Build Priority

1. Finish Contacts Sidecar
2. Finish Companies Sidecar
3. Finish Vendors Sidecar
4. Finish Partners Sidecar
5. Wire Relationships
6. Wire Photo Uploads
7. Build Editable Center Pages
8. Enable Assistant Actions
9. Connect Retrieval Enhancements
10. Consolidate Legacy Artifacts

---

# Project Status

Estimated Completion

Backend Foundation: 90%

Graph Foundation: 85%

Retrieval Foundation: 90%

Assistant Foundation: 80%

UI Shell: 95%

Relationship Persistence: 40%

Photo Pipeline: 10%

Center Pages: 60%

Overall Platform Completion:

Approximately 80–85% complete.
