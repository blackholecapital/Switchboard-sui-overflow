# Runtime-C Search Engine Usage Guide

## Overview

The Runtime-C Search Engine is a retrieval and ndassembly system designed to discover, rank, materialize, and package implementation patterns from large software repositories.

Unlike a traditional search engine that returns links, Runtime-C produces implementation-ready resource packs containing source files, metadata, context bundles, and assembly instructions.

---

# Root Directory

```text
/mnt/eila-hot-sidecar/runtime-c-assets/09-resource-packs
```

Primary handoff:

```text
/mnt/eila-hot-sidecar/runtime-c-assets/09-resource-packs/ASSEMBLY_HANDOFF.md
```

---

# What the Search Engine Produces

Each search run generates one or more Resource Packs.

Example:

```text
09-resource-packs/
├── invoices/
├── calendar/
├── tasks/
├── contacts/
├── companies/
├── deals/
├── documents/
├── notes/
├── automation/
└── reporting/
```

Each pack contains:

```text
pack/
├── manifest.json
├── RETRIEVAL_HITS.json
├── QWEN_CONTEXT_BUNDLE.md
├── FILES.md
└── files/
```

---

# Pack Contents

## manifest.json

Ranked search results.

Contains:

* candidate score
* source location
* retrieval metadata
* implementation ranking

Use this first to understand why files were selected.

---

## RETRIEVAL_HITS.json

Raw retrieval results before filtering.

Useful for:

* debugging searches
* improving ranking
* discovering missed candidates

---

## QWEN_CONTEXT_BUNDLE.md

Curated context package.

Contains:

* implementation notes
* architecture hints
* module guidance
* supporting references

Designed for AI-assisted implementation.

---

## FILES.md

Human-readable source inventory.

Contains:

* source file locations
* copied file locations
* ranking order
* provenance information

Example:

```text
- source:
/path/to/original/file.php

- copied:
/path/to/resource-pack/files/file.php
```

---

## files/

Materialized source files.

These are the actual implementation artifacts copied from source repositories.

Examples:

* PHP modules
* controllers
* metadata
* vardefs
* dashboard examples
* workflow definitions
* UI examples

---

# Search Workflow

## Stage 1 — Discovery

Search identifies relevant assets.

Sources may include:

* SuiteCRM
* Metabase
* n8n
* Refine
* Supabase
* internal registries
* repository indexes

---

## Stage 2 — Ranking

Candidates receive scores.

Factors include:

* keyword relevance
* entity match
* module match
* implementation completeness
* repository importance

---

## Stage 3 — Materialization

Files are copied into pack folders.

Result:

```text
original source
    ↓
resource-pack/files/
```

This creates a stable implementation corpus.

---

## Stage 4 — Assembly

Agents consume:

```text
manifest.json
FILES.md
files/
```

to reconstruct complete modules.

---

# Recommended Usage

For any module:

## 1. Open manifest.json

Understand:

* top candidates
* ranking
* retrieval confidence

---

## 2. Read FILES.md

Review:

* source locations
* copied artifacts
* implementation coverage

---

## 3. Inspect files/

Identify:

* models
* controllers
* views
* metadata
* workflows

---

## 4. Select Complete Patterns

Prefer complete implementations over fragments.

Example:

Good:

```text
Invoice Model
Invoice Controller
Invoice Views
Invoice Metadata
Invoice Relationships
```

Bad:

```text
Single field definition only
```

---

## 5. Build From Existing Patterns

Avoid creating placeholder implementations.

Reuse:

* workflows
* layouts
* forms
* relationships
* dashboards

whenever possible.

---

# Reporting Pack Special Handling

Reporting has a dedicated Metabase harvest.

Location:

```text
reporting/metabase-files/
```

Documentation:

```text
reporting/REPORTING_METABASE_HANDOFF.md
```

Contains:

* KPI examples
* revenue dashboards
* forecast dashboards
* chart patterns
* query patterns
* dashboard layouts
* embedded dashboard examples

Use this corpus first for reporting features.

Ignore older reporting candidates unless necessary.

---

# Recommended Build Order

```text
1. invoices
2. calendar
3. tasks
4. contacts
5. companies
6. deals
7. documents
8. notes
9. automation
10. reporting
```

This order minimizes dependency conflicts.

---

# Design Philosophy

Runtime-C Search is not intended to generate code from scratch.

Its purpose is to:

1. Discover proven implementations.
2. Preserve implementation context.
3. Materialize reusable artifacts.
4. Enable deterministic reconstruction.
5. Reduce hallucinated architecture.

The preferred workflow is:

```text
Search
    ↓
Rank
    ↓
Materialize
    ↓
Inspect
    ↓
Assemble
    ↓
Ship
```

---

# Success Criteria

A successful search session produces:

* ranked candidates
* copied source files
* implementation context
* assembly instructions
* reproducible build inputs

When these artifacts exist, implementation should begin from the Resource Pack rather than from a blank prompt.

---

# Current Status

Resource packs successfully generated and materialized.

Ready for:

* CRM module reconstruction
* dashboard development
* workflow implementation
* reporting systems
* AI-assisted assembly
* future retrieval refinement

Runtime-C Search Engine is operational.
