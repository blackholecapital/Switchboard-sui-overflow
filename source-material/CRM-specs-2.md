# Switchboard CRM Architecture Amendment v2

## Backend Discovery Report & Source of Truth Mapping

Date: June 2026

---

# Executive Summary

A detailed backend inspection revealed that Switchboard CRM is significantly more complete than originally assumed.

The frontend currently presents itself as a partially completed CRM; however, the backend already contains:

* CRM CRUD operations
* Graph integration
* Audit logging
* Activity logging
* Workflow execution
* Retrieval indexing
* AI assistant retrieval
* Contact360 APIs
* Company360 APIs
* Graph360 APIs

This changes the implementation strategy from:

Build Missing Systems

to:

Connect Existing Systems

---

# Confirmed Source of Truth

## Primary System of Record

```text
crm.contacts
crm.companies
crm.tasks
sales.deals
billing.invoices
```

These are the authoritative business entities.

---

# Graph Layer

Confirmed Production Graph

```text
graph.graph_nodes
graph.graph_edges
graph.graph_events
```

Purpose:

* relationship intelligence
* graph navigation
* graph analytics
* AI context generation

---

# Contact Creation Flow

Confirmed Flow

```text
Create Contact
        ↓
crm.contacts
        ↓
graph.graph_nodes
        ↓
graph.graph_events
        ↓
crm.activities
        ↓
automation workflows
```

Status:

Implemented

---

# Company Creation Flow

Confirmed Flow

```text
Create Company
        ↓
crm.companies
        ↓
graph.graph_nodes
        ↓
graph.graph_events
        ↓
crm.activities
```

Status:

Implemented

---

# Audit Layer

Confirmed

```text
audit.audit_log
```

Current Events:

* contact.updated
* contact.deleted
* company.updated
* company.deleted
* task.created

Purpose:

* change tracking
* compliance
* rollback support
* future activity intelligence

---

# Activity Layer

Confirmed

```text
crm.activities
```

Used by:

* Contacts
* Companies
* Tasks
* Future workflow tracking

Purpose:

Unified activity stream.

---

# Graph360 Services

Confirmed APIs

```text
/api/graph/edges
/api/graph/record/:id
/api/contact360/:id
/api/company360/:name
```

Status:

Operational

---

# Contact360 Data Sources

Contact360 currently assembles:

```text
Contact
Company
Deals
Tasks
Invoices
Activities
Documents
Notes
Graph Relationships
```

This endpoint already behaves like a complete CRM 360 service.

---

# Company360 Data Sources

Company360 currently assembles:

```text
Company
Deals
Activities
Documents
Notes
Graph Relationships
```

Status:

Operational

---

# AI Assistant Discovery

Major finding:

Assistant is NOT a placeholder.

Current Architecture:

```text
AI Sidecar
      ↓
/api/assistant/chat
      ↓
retrieval.crm_chunks
      ↓
Ollama
      ↓
Qwen 30B
```

Status:

Operational

---

# Retrieval Layer Discovery

Confirmed Index

```text
retrieval.crm_chunks
```

Indexed Sources

```text
crm.companies
crm.contacts
sales.deals
crm.tasks
billing.invoices
files.record_files
```

Purpose:

CRM RAG System

---

# Retrieval Search

Search currently performs:

* full text search
* weighted ranking
* entity prioritization
* CRM context assembly

before passing context to the LLM.

---

# Assistant Capability Model

Current:

```text
Search CRM
Answer Questions
Retrieve Context
Generate Responses
```

Future:

```text
Create Contacts
Create Deals
Create Invoices
Create Tasks
Run Workflows
Graph Exploration
```

---

# Missing Relationship Wiring

Discovered Gap

Current:

```text
graph_nodes
```

Created automatically.

Missing:

```text
graph_edges
```

Creation from CRM UI.

Result:

Relationships appear in UI but are not fully persisted.

---

# Missing Contact Relationship Model

Current Contact Drawer Contains

```text
Relationship Type
Relationship Label
Link Relationship
```

Backend currently lacks:

```text
relationship_target
relationship_label
relationship_type
```

persistence.

---

# Missing Photo Pipeline

Not yet implemented.

Desired:

```text
Upload Photo
      ↓
Storage
      ↓
avatar_url
      ↓
Contact Profile
```

Status:

Pending

---

# Remaining CRM Build Strategy

Phase 1

Finish Sidecars

* Contacts
* Companies
* Vendors
* Partners

Phase 2

Graph Edge Creation

Contact
↔
Company

Contact
↔
Vendor

Contact
↔
Partner

Company
↔
Company

Phase 3

Complete Center Pages

Using:

* Contact360
* Company360
* Graph360

No new APIs required.

Phase 4

Assistant Actions

Assistant becomes operational command layer.

---

# Architectural Conclusion

Switchboard CRM is no longer a greenfield CRM project.

It is an integration and consolidation project.

Most major subsystems already exist and are functioning.

Primary remaining work:

* UI standardization
* relationship persistence
* graph edge creation
* photo uploads
* center-page completion
* assistant action execution

The platform foundation is substantially complete.
