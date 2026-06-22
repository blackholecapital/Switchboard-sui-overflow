# Switchboard Architecture

## Platform Architecture

```text
Switchboard OS
├── CRM Core
├── Billing360
├── Storage360
├── Treasury360
├── Documents360
├── Automation360
├── Comms360
├── Workspace360
├── Reporting360
├── Wallet Layer
├── Sui Integration Layer
└── AI Sidecars
```

## High-Level Layers

```text
User Interface
  ↓
Module Layer
  ↓
Shared Components / Wallet Layer
  ↓
Business State / Backend APIs
  ↓
Sui / Storage / Payment Integrations
  ↓
Receipts, Attestations, Records
```

## Integration Model

Switchboard is not designed as one isolated app. It is a modular operating system where each business module shares common infrastructure:

- navigation shell
- module sidecars
- wallet provider
- shared UI primitives
- backend API layer
- graph / record model
- retrieval and context systems
- payment and attestation patterns

## Sui-Enabled Business Infrastructure

Sui is used as an operational layer for:

- wallet-aware actions
- USDC payment workflows
- transaction receipts
- backup attestations
- business record verification
- treasury visibility

## Architectural Philosophy

Switchboard aims to hide complexity without hiding power.

A user should see:

```text
Create invoice
Make payment
Create backup
View receipt
```

while the system handles:

```text
wallet state
transaction digest
attestation record
storage linkage
CRM update
treasury visibility
```
