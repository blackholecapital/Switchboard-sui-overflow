# Switchboard CRM — Platform Specification

**Version:** Current Development Snapshot
**Date:** June 2026

---

# Executive Summary

Switchboard CRM is a unified operating system combining CRM, relationship intelligence, graph databases, automation, retrieval systems, reporting, AI assistance, and operational workflows.

Unlike a traditional CRM, Switchboard is designed as a connected data platform where contacts, companies, deals, documents, communications, tasks, workflows, and reporting exist inside a shared relationship graph.

---

# Core Platform Components

## CRM Core

### Contacts

* Contact Management
* Contact 360 View
* Contact Relationships
* Activity Timeline
* Contact Classification

  * People
  * Companies
  * Vendors
  * Partners
* Notes
* Tags
* Status Tracking
* Contact Search

### Companies

* Company Records
* Company 360 View
* Company Relationships
* Associated Contacts
* Associated Deals
* Activity History

### Deals

* Lead Tracking
* Opportunity Management
* Sales Pipeline
* Deal Stages
* Revenue Forecasting
* Deal Relationships

### Tasks

* Task Assignment
* Due Dates
* Activity Tracking
* Follow-Up Management

### Calendar

* Scheduling
* Events
* Follow-Ups
* CRM Activities

---

# Relationship Engine

Switchboard is designed around relationship mapping.

Relationships may exist between:

* Contact ↔ Contact
* Contact ↔ Company
* Contact ↔ Vendor
* Contact ↔ Partner
* Company ↔ Company
* Company ↔ Vendor
* Deal ↔ Contact
* Deal ↔ Company
* Task ↔ Contact

Relationship metadata includes:

* Type
* Label
* Owner
* Role
* Status

Examples:

* Owner
* Sales Contact
* Billing Contact
* Decision Maker
* Technical Contact

---

# Graph Layer

Graph architecture exists for:

## Nodes

Examples:

* Contacts
* Companies
* Deals
* Documents
* Tasks
* Workflows

## Edges

Examples:

* owns
* works_for
* manages
* related_to
* purchased
* assigned_to

Graph tables currently identified:

```sql
graph.graph_nodes
graph.graph_edges
```

Purpose:

* Relationship intelligence
* Entity navigation
* AI context retrieval
* Future graph analytics

---

# AI Assistant

Current UI implemented.

Purpose:

* CRM navigation
* Search assistance
* Contact lookup
* Deal lookup
* Reporting assistance
* Workflow assistance

Planned:

* Voice input
* Voice output
* Graph-aware retrieval
* Context-aware actions
* Record creation from prompts

Examples:

* Create Contact
* Create Deal
* Create Invoice
* Search Records
* Open Support Ticket

---

# Automation Layer

Automation module exists.

Planned capabilities:

* Trigger workflows
* Record updates
* Notifications
* Task generation
* AI actions
* External integrations

Potential integrations:

* Email
* SMS
* Calendar
* Documents
* AI agents

---

# Reporting System

Reporting module exists.

Data sources:

* Contacts
* Companies
* Deals
* Tasks
* Revenue

Planned dashboards:

* Pipeline
* Forecasting
* Revenue
* Activity
* Team Performance
* Conversion Analytics

---

# Retrieval Layer

Runtime retrieval infrastructure exists.

Includes:

* LanceDB
* CRM Context Collections
* Workflow Collections
* Build Collections
* Asset Collections

Purpose:

* AI memory
* Semantic search
* Context assembly
* Workflow retrieval

---

# Security

Identified components:

* Supabase Authentication
* Row Level Security (RLS)
* Tenant Isolation
* Role-Based Permissions

---

# User Interface Framework

Technology Stack:

* React
* TypeScript
* Refine
* Tailwind
* Lucide Icons
* Tabler Icons

UI Architecture:

* Left Navigation Shell
* Top Command Bar
* Sidecar Drawers
* Entity 360 Pages
* Assistant Drawer
* Reporting Dashboards

---

# Current Development Status

### Stable

* CRM Shell
* Navigation
* Contacts Module
* Companies Module
* Pipeline Module
* Task Module
* Reporting Framework
* Assistant Drawer

### In Progress

* Relationship Persistence
* Graph Wiring
* Contact Save Pipeline
* Photo Upload Pipeline
* AI Action Execution
* Workflow Execution

### Future

* Full Graph Intelligence
* AI Agent Actions
* Cross-Module Relationship Mapping
* Predictive Analytics
* Executive Dashboards

---

# Architectural Goal

Switchboard is evolving toward:

CRM
+
Graph Database
+
Retrieval Engine
+
Automation Platform
+
AI Operating System

inside a single operational workspace.
