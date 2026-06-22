# Switchboard Primitive Registry

## Sidebar, Navigation, Primitive Registry, and Platform Foundation Work

### Summary

A major platform cleanup and architecture pass was completed across the CRM frontend and shared core layers.

The system is now in a significantly healthier state with:

* Sidebar persistence fixed
* Navigation architecture stabilized
* Storage 360 promoted to a first-class platform domain
* Agentic Workflows and Coding Agent beta entries added
* Sidebar configuration externalized
* Primitive registry framework established
* Primitive debt reduced to zero
* Build passing successfully

---

# Sidebar Work Completed

## Sidebar Persistence

Problem:

Sidebar state was resetting during navigation.

Desired behavior:

* User closes sidebar → remains closed
* User opens sidebar → remains open
* State survives route changes

Resolution:

`src/components/ui/sidebar.tsx`

Updated internal state initialization to restore from cookie.

Persistence mechanism:

```ts
sidebar_state
```

Cookie persistence now works correctly.

Result:

* State retained across navigation
* No longer reopens unexpectedly

---

# Navigation Cleanup

## Storage 360 Promotion

Storage 360 was originally placed in secondary navigation.

Moved into primary navigation directly under:

```text
Treasury 360
Storage 360
Reports
```

Storage is now treated as a platform domain rather than a utility.

---

## Automation Rename

Changed:

```text
Automation
```

to:

```text
Automation 360
```

to align with platform naming conventions.

---

## Removed Upper Settings Wheel

Settings icon was removed from top-level navigation placement.

Global Settings remains available inside Information/Utility links.

---

# Agent Features Added

## Agentic Workflows (Beta)

Renamed:

```text
AI-Assistant
```

to:

```text
Agentic Workflows (Beta)
```

Route unchanged:

```text
/assistant
```

Bolt icon retained temporarily.

Purpose:

Workflow orchestration, agents, automation.

---

## Coding Agent (Beta)

New placeholder page created.

Route:

```text
/coding-agent
```

Module:

```text
src/modules/codingagent
```

Current state:

```text
Coming Soon
```

Purpose:

Future Runtime-C / Coding System integration.

---

# Sidebar Architecture Refactor

## Quarantine Structure Added

Created:

```text
src/_quarantine/sidebar
```

Archived versions:

app-sidebar.tsx.pre-360-expansion

app-sidebar.tsx.pre-config-extraction

app-sidebar.tsx.pre-config-wireup

app-sidebar.tsx.pre-final-config-cutover

sidebar.tsx.pre-cookie-persistence

Purpose:

Rollback capability and architecture history.

---

# Sidebar Configuration Extraction

Created:

```text
src/config/sidebar.config.ts
src/config/sidebar.types.ts
```

Goal:

Move navigation definitions out of component code.

Benefits:

* Centralized navigation
* Easier future module registration
* Supports platform governance

---

## Sidebar Config Contents

Primary Navigation:

* Dashboard
* Contacts 360
* Deals 360
* Billing 360
* Comms 360
* Documents 360
* Automation 360
* Marketing 360
* Workspace 360
* Treasury 360
* Storage 360
* Reports

Information Links:

* Docs
* Community
* Help / FAQ
* Agentic Workflows (Beta)
* Coding Agent (Beta)
* Global Settings
* More

---

## Config Cutover

Completed:

```ts
const data = {
  navMain,
  documents: informationLinks,
}
```

Result:

Embedded navigation removed from component.

Sidebar now consumes centralized configuration.

---

# Primitive Architecture Created

Created root registry:

```text
03-core/primitives
```

Domains:

contacts

deals

billing

documents

storage

treasury

workspace

marketing

automation

comms

reporting

shared

---

## Domain Structure

Every domain now owns:

```text
catalog.ts
index.ts
README.md
```

Governance file:

```text
03-core/primitives/PRIMITIVE_GOVERNANCE.md
```

---

# 360 Module Primitive Bridges

Created bridge exports:

```text
src/modules/*360/primitives/index.ts
```

Mapping:

contacts360 → contacts

deals360 → deals

billing360 → billing

documents360 → documents

storage360 → storage

treasury360 → treasury

workspace360 → workspace

marketing360 → marketing

automation360 → automation

comms360 → comms

reporting360 → reporting

---

# Primitive Debt Elimination

Initial state:

10 empty primitive files.

Final state:

0 empty primitive files.

Verified:

```bash
find 03-core/primitives \
-type f \
-name "*.primitive.ts" \
-size 0 | wc -l

0
```

---

# Storage Primitive Catalog

Registered:

* storage_provider
* storage_manifest
* walrus_blob
* attestation
* chain_receipt
* backup_job

Exports wired.

Catalog created.

---

# Treasury Primitive Catalog

Registered:

* wallet
* treasury_account
* portfolio_position
* vault_position
* clmm_position
* payment_receipt

Exports wired.

Catalog created.

---

# Build Status

Final build status:

SUCCESS

```bash
npm run build
```

Passing.

No TypeScript errors.

No sidebar errors.

No navigation errors.

---

# Next Priority For Billing 360 Agent

## Immediate Investigation

Wallet connection persistence.

Audit:

```bash
grep -RIn "useCurrentAccount\|useCurrentWallet\|WalletProvider\|SuiClientProvider" src
```

Investigate:

```bash
grep -RIn "<a href=\|window.location\|location.href" src
```

Suspected root cause:

Full page navigation causing provider teardown and wallet reconnection issues.

Potential remediation:

Convert navigation to SPA routing:

```tsx
<Link to="/route" />
```

instead of:

```tsx
<a href="/route" />
```

This is likely the next highest-value platform stability improvement.

---

# Platform State

Status: Stable

Sidebar: Stable

Primitive Registry: Operational

Navigation: Operational

Build: Passing

Technical Debt: Reduced

Ready for Wallet Persistence Investigation.
