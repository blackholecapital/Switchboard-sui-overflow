# Billing360

Billing360 is the invoice and payment module of Switchboard OS.

## Confirmed Capabilities From Source Notes

- invoice creation workflow
- CRM customer/company picker
- automatic bill-to population
- line items
- notes
- totals
- draft saving
- print/PDF flow
- PayMe checkout integration
- Sui USDC payment option
- Stripe payment option
- TXID capture
- paid-state update
- pipeline/CRM update path

## Demo Flow

```text
Select Customer
  ↓
Create Invoice
  ↓
Open Payment Surface
  ↓
Pay with Sui USDC or Stripe
  ↓
Capture Transaction
  ↓
Update Invoice State
  ↓
Show Receipt / Attestation
```

## Business Value

Billing360 demonstrates a normal business workflow where blockchain payments and receipts happen behind the scenes.
