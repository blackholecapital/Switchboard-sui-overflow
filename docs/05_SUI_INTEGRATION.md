# Sui Integration

Switchboard uses Sui as a background business infrastructure layer.

## Sui Surfaces

- wallet connection
- Sui USDC payment flow
- transaction digest capture
- receipt visibility
- backup attestations
- treasury wallet balance
- payment history context

## Payment Flow

```text
Invoice
  ↓
Payment Surface
  ↓
Sui USDC Payment
  ↓
Transaction Digest
  ↓
Invoice Paid State
  ↓
CRM / Billing Record Update
```

## Storage Flow

```text
Backup Event
  ↓
Storage Record
  ↓
Sui Receipt / Transaction Record
  ↓
Storage360 History
```

## Treasury Flow

```text
Connected Wallet
  ↓
Balance Read
  ↓
Treasury360 Overview
  ↓
Payment / Receipt Context
```

## Design Intent

The user should not need to understand blockchain details to benefit from Sui. Switchboard surfaces Sui as receipts, payment confirmation, wallet-aware controls, and operational records.
