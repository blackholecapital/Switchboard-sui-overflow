# CRM Foundation Build Workflow

## Goal

Build one high-end, single-client CRM foundation first.

The system should be:

- online
- secure
- modular
- Supabase/Postgres-backed
- graph-aware
- AI-assistant-ready
- LanceDB searchable
- plugin-ready
- later SaaS deployable

## Stage 1 — Isolate CRM Build Zone

Create:

/mnt/eila-hot-sidecar/crm-foundation

Set ownership, permissions, and base folders.

Test points:

- root exists
- permissions correct
- folder tree created
- docs folder created
- build logs folder created

## Stage 2 — Create Core Docs

Create:

- CRM_FILESYSTEM_TREE.md
- CRM_BUILD_WORKFLOW.md
- CRM_MASTER_PLAN.md
- CRM_MODULE_MAP.md
- CRM_DATABASE_MAP.md
- CRM_SECURITY_MAP.md
- CRM_PLUGIN_MAP.md

Test points:

- docs are present
- module list is frozen
- database schemas are named
- plugin shells are defined

## Stage 3 — Database Foundation

Create Supabase/Postgres schemas:

- core
- crm
- sales
- marketing
- automation
- reporting
- graph
- ai
- audit
- files
- social

Core tables:

- tenants
- users
- roles
- permissions
- contacts
- companies
- leads
- deals
- pipeline_stages
- tasks
- notes
- activities
- files
- campaigns
- forms
- workflows
- graph_nodes
- graph_edges
- graph_events
- audit_log

Test points:

- migrations run
- tables exist
- tenant_id exists where needed
- timestamps exist
- indexes exist
- test insert works

## Stage 4 — Security Base

Set up:

- Supabase Auth
- roles
- permissions
- RLS policies
- admin user
- audit log
- Cloudflare Access for admin/internal routes

Test points:

- login works
- unauthorized access blocked
- RLS blocks cross-tenant reads
- audit log records login/action

## Stage 5 — Graph Layer

Create graph system from day one:

- graph_nodes
- graph_edges
- graph_events

Initial relationships:

- contact belongs_to company
- lead came_from source
- deal linked_to contact
- deal assigned_to user
- task linked_to deal
- file attached_to record
- campaign generated lead
- message sent_to contact

Test points:

- creating a contact creates node
- linking contact/company creates edge
- activity creates graph_event
- graph query returns relationships

## Stage 6 — LanceDB / Retrieval Layer

Use existing LanceDB.

Create CRM retrieval sections:

- crm_assets
- crm_architecture
- crm_client_001
- crm_workflows
- crm_build_runs

Connect:

- runtime-c-assets
- CRM registries
- module specs
- build outputs
- client docs

Test points:

- LanceDB health check passes
- context retrieval works
- QWEN_CONTEXT_BUNDLE.md generated
- module-specific query returns useful results

## Stage 7 — AI Assistant Clone

Clone existing working LLM/Ollama assistant.

Point it to:

- new CRM database
- graph tables
- LanceDB CRM context
- CRM filesystem maps

Initial mode:

- read-only
- logged
- no destructive writes
- admin approval for actions

Test points:

- assistant starts
- assistant can query CRM schema
- assistant can query graph
- assistant can retrieve context
- assistant logs requests

## Stage 8 — Plugin Shells

Create plugin folders:

- Telegram
- Facebook
- Instagram
- YouTube
- Snapchat
- Discord
- Email
- SMS
- Webhooks

Each plugin gets:

- README.md
- plugin.json
- auth_config.json
- webhook_receiver.md
- event_mapper.json
- permissions.json
- logs/

Test points:

- plugin registry loads
- Telegram shell linked first
- webhook route stub exists
- plugin events can map to CRM events

## Stage 9 — Frontend Seed

Seed from:

- Refine + Supabase auth
- shadcn/Radix
- TanStack Table
- Recharts
- React Flow
- FullCalendar

Initial frontend target:

- login
- dashboard
- sidebar
- module routes
- table layout
- record detail layout
- pipeline placeholder
- AI/search panel placeholder

Test points:

- frontend runs
- login screen works
- routes load
- sidebar loads all modules
- Supabase connection works

## Stage 10 — 15 Module Shells

Create route shells for:

1. dashboard
2. contacts
3. companies
4. leads
5. pipeline
6. tasks
7. calendar
8. campaigns
9. forms
10. automation
11. reporting
12. documents
13. conversations
14. social
15. admin/settings

Test points:

- every route loads
- every module has README/module.json
- every module has placeholder UI
- every module has permissions stub

## Stage 11 — Core CRM CRUD

Build first working modules:

- contacts
- companies
- leads
- deals/pipeline
- tasks
- notes
- files
- activity timeline

Test points:

- create/read/update/delete works
- pipeline drag/update works
- activity log records changes
- graph writes happen
- audit log records actions

## Stage 12 — Automation Layer

Wire:

- n8n
- webhooks
- lead capture
- task creation
- notifications
- Telegram later
- email later

Test points:

- webhook receives test event
- event writes to CRM
- automation log created
- task auto-created from event
- failed automation logged

## Stage 13 — Reporting Layer

Start with basic reporting:

- lead count
- deal value
- pipeline by stage
- conversion rate
- task completion
- activity volume
- campaign/source attribution

Later connect:

- Metabase
- Superset

Test points:

- reports load
- numbers match database
- filters work
- tenant isolation works

## Stage 14 — Cloudflare / Online Deploy

Deploy through:

- Cloudflare Tunnel
- Cloudflare Access for admin
- HTTPS
- Supabase Auth
- server firewall rules
- backup job

Test points:

- public app reachable
- admin route protected
- login works externally
- tunnel stable
- backup runs

## Stage 15 — Client-001 Launch Prep

Prepare first client workspace:

- tenant record
- admin user
- import folders
- file buckets
- baseline pipeline
- default roles
- default dashboards
- backup snapshot

Test points:

- client login works
- import test works
- CRM records visible
- automations stubbed
- reporting visible
- backup snapshot created

## Qwen Coder Usage

Use Qwen Coder manually with narrow instructions.

Good instruction pattern:

“Build only the Contacts module using this schema, this route, this UI pattern, and these reference paths. Do not change global architecture.”

Do not ask Qwen:

“Build the whole CRM.”

Ask Qwen:

- build one module
- wire one table
- create one route
- create one component
- create one migration
- create one test
- inspect one bug

## Build Rule

No polish until the core loop works:

- login
- create record
- link record
- graph write
- audit write
- search/retrieve
- automation event
- report value

Once that works, polish the UI.