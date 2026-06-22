# CRM Foundation File System Tree

Root:

/mnt/eila-hot-sidecar/crm-foundation

## Purpose

This is the isolated 500G CRM build zone.  
It runs on top of the existing ILO OS / agentic core and connects to:

- Supabase/Postgres
- SQL graph tables
- LanceDB retrieval
- Qwen coder context bundles
- Plugin shells
- Cloudflare front door
- CRM module filesystem

## Tree

crm-foundation/
  00-admin/
    README.md
    build-notes/
    decisions/
    handoffs/
    logs/

  01-config/
    env/
    secrets.example/
    cloudflare/
    nginx/
    supabase/
    ollama/
    qwen/
    agent-core/

  02-databases/
    supabase/
      migrations/
      seeds/
      schemas/
      policies/
      functions/
    graph/
      graph_nodes.sql
      graph_edges.sql
      graph_events.sql
    backups/
    exports/

  03-core/
    auth/
    tenants/
    users/
    roles/
    permissions/
    audit/
    events/
    files/
    notifications/
    ai-assistant/
    retrieval/
    graph/

  04-modules/
    contacts/
    companies/
    leads/
    pipeline/
    tasks/
    calendar/
    campaigns/
    forms/
    automation/
    reporting/
    documents/
    conversations/
    social/
    client-portal/
    admin-settings/

  05-plugins/
    telegram/
    facebook/
    instagram/
    youtube/
    snapchat/
    discord/
    email/
    sms/
    webhooks/

  06-apps/
    web-crm/
    admin-panel/
    worker/
    ai-agent/
    plugin-gateway/

  07-retrieval/
    lancedb/
    context-bundles/
    module-indexes/
    client-indexes/
    build-runs/
    qwen-prompts/

  08-client-data/
    client-001/
      imports/
      exports/
      files/
      documents/
      reports/
      backups/

  09-build-runs/
    scratch/
    staging/
    artifacts/
    promoted/
    failed/

  10-deploy/
    docker/
    systemd/
    nginx/
    cloudflare/
    backup-restore/
    launch-checklists/

  11-docs/
    architecture/
    module-specs/
    database-maps/
    plugin-specs/
    security/
    runbooks/

  12-tests/
    smoke/
    database/
    auth/
    api/
    frontend/
    automation/
    graph/
    retrieval/


crm-foundation/
  product-exports/
    crm-core/
    lead-capture/
    pipeline-crm/
    automation-engine/
    reporting-dashboard/
    proposal-builder/
    onboarding-portal/
    support-desk/
  

    

## Module Standard

Every module should contain:

module-name/
  README.md
  module.json
  schema.sql
  routes.json
  permissions.json
  events.json
  automations.json
  retrieval.json
  tests.md
  frontend/
  backend/
  workers/

## Core Rule

Every important CRM action should write to:

1. Primary business table
2. activity log
3. audit log
4. graph event
5. optional automation event