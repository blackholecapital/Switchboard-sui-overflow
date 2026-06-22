# Switchboard CRM Remaining Work Plan

## Post-Architecture Discovery Revision

Current Status:

* Step 4 Deferred
* Step 5 Active
* Backend largely operational
* Graph operational
* Retrieval operational
* Assistant operational
* UI standardization in progress

---

# Step 5 — Sidecar Standardization (Current Phase)

Goal:

Create one visual language for every drawer in the platform.

---

## Standard Sidecar Structure

Every sidecar must contain:

Header

Description

Tooltip Area

Action Area

Content Area

Footer Actions

---

## New Standard Header

```text
[ Icon ] Title                     [ Tooltip ]
Description
```

Tooltip becomes permanent architecture.

Position:

Top-right of sidecar header.

Purpose:

* contextual guidance
* onboarding
* workflow hints
* AI suggestions

---

## Sidecars To Complete

### Contacts

Status:
90%

Remaining:

* photo upload persistence
* graph relationship persistence
* add relationship button
* relationship save
* center-page launch hooks

---

### Companies

Status:
70%

Build using Contacts template.

Includes:

* company details
* relationship management
* linked contacts
* linked deals
* activity stream

---

### Vendors

Status:
0%

Clone Company sidecar.

Adjust labels.

---

### Partners

Status:
0%

Clone Company sidecar.

Adjust labels.

---

### Notifications

Status:
30%

Convert to standard sidecar shell.

---

### User Dashboard

Status:
50%

Already partially built.

Needs:

* tasks
* messages
* notifications
* connected accounts
* preferences
* theme settings

---

### AI Assistant

Status:
85%

Remaining:

* connect assistant endpoint
* connect retrieval endpoint
* conversation history
* suggested prompts
* voice pipeline

---

# Step 4 — Tooltip Knowledge System

Moved after Sidecar Standardization.

Reason:

Tooltips need stable placement.

---

## Tooltip Database

Recommended Schema

tooltip_definitions

Fields:

* id
* module
* screen
* location
* title
* description
* priority
* trigger_type
* condition
* role
* active

---

## Tooltip Scoring

Each tooltip receives:

Position Score

Example:

```text
Header Tooltip       100
Action Tooltip        80
Field Tooltip         60
Footer Tooltip        40
```

---

## Tooltip Conditions

Examples:

First Login

No Contacts

No Deals

Admin User

Sales User

Relationship Missing

Assistant Available

Workflow Available

---

## Tooltip Types

Tips

Warnings

Suggestions

Best Practices

AI Recommendations

Training

---

# Step 6 — User Mini Dashboard

Convert avatar into command center.

Contains:

My Tasks

Messages

Notifications

Connected Accounts

Theme

Profile

Preferences

Recent Activity

Assistant Shortcuts

---

# Step 7 — Communications Consolidation

Build shell first.

Modules:

Email

SMS

Telegram

Social

Internal Messages

Voice

Assistant Conversations

---

## Goal

Single communications experience.

All channels eventually route through:

communications hub

---

# Step 8 — Settings Consolidation

Convert settings from page collection into platform settings.

---

## Sections

Profile

Security

Permissions

Notifications

Branding

Themes

Integrations

Company Settings

Assistant Settings

Communications Settings

Workflow Settings

---

# Step 9 — Forms Consolidation

Low Priority

Reason:

Schemas already exist.

Work is:

reorganization

not creation.

---

## Target

Single form framework.

Reusable across:

Contacts

Companies

Deals

Tasks

Documents

Workflows

---

# Step 10 — Repository Layer

Purpose:

Turn CRM into knowledge platform.

---

## Packs

Document Packs

Industry Packs

Workflow Packs

Template Packs

Automation Packs

Prompt Packs

Agent Packs

---

# Backend Wiring Work

Parallel Track

---

## Relationship Persistence

Current:

Nodes created automatically.

Missing:

Edges.

---

### Save Flow

Contact
↓
Graph Node
↓
Graph Edge
↓
Contact360
↓
Assistant Retrieval

---

## Photo Upload

Upload
↓
Storage
↓
avatar_url
↓
Contact Profile

---

## Assistant Wiring

Current:

Assistant + Retrieval exist.

Remaining:

Sidecar
↓
Assistant Endpoint
↓
Retrieval
↓
Graph Context
↓
CRM Context

---

# Center Pages

After Sidecars

---

## Principle

No inline editing.

Everything edits through sidecars.

---

## Cards

Minimal display

3-dot menu

Edit

Open Sidecar

Save

---

## Pages

Contacts

Companies

Vendors

Partners

Deals

Tasks

Documents

---

# Consolidation Phase

After all above.

Archive:

* .pre files
* restore files
* backup files

Move into:

/archive

Do not delete.

Preserve development history.

---

# Project Assessment

Backend Foundation: 90%

Graph Foundation: 90%

Retrieval Foundation: 90%

Assistant Foundation: 85%

UI Shell: 95%

Sidecars: 75%

Relationship Persistence: 40%

Photo Uploads: 10%

Center Pages: 60%

Overall Platform Completion:
~85%

```
```
