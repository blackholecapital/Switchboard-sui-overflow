# Switchboard Demo Workflows

## Workflow 1: Invoice to Sui USDC Payment

```mermaid
flowchart LR
    A[Create Invoice] --> B[PayMe Checkout]
    B --> C[Sui USDC Payment]
    C --> D[Transaction ID Captured]
    D --> E[Invoice Marked Paid]
    E --> F[CRM Pipeline Updated]
```

This workflow demonstrates how a business invoice can be paid through a blockchain-native payment surface while still behaving like a normal business transaction inside the CRM.

## Workflow 2: Business Record to Attestation

```mermaid
flowchart LR
    A[Business Event] --> B[Record Created]
    B --> C[Receipt / TXID]
    C --> D[Attestation]
    D --> E[Audit Trail]
```

The platform can connect records such as invoices, backups, and document events to verifiable proof records.

## Workflow 3: Storage360 Backup Flow

```mermaid
flowchart LR
    A[User Starts Backup] --> B[Backup Record]
    B --> C[Storage Provider]
    C --> D[Sui Receipt]
    D --> E[Storage360 History]
```

Storage360 provides a business-facing control surface for backup records and receipt visibility.

## Workflow 4: Treasury360 Visibility

```mermaid
flowchart LR
    A[Connected Wallet] --> B[Live Balance Read]
    B --> C[Treasury Overview]
    C --> D[Payment Context]
    D --> E[Operational Visibility]
```

Treasury360 is the financial visibility layer for the platform, connecting wallet activity to business operations.

## Workflow 5: AI Sidecar Assistance

```mermaid
flowchart LR
    A[Module Context] --> B[AI Sidecar]
    B --> C[Guidance]
    C --> D[Suggested Action]
    D --> E[Workflow Execution]
```

AI sidecars support the user inside each module instead of forcing them to leave the workflow to ask a separate assistant.
