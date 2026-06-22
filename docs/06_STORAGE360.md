# Storage360

Storage360 is the storage, backup, receipt, and attestation layer of Switchboard OS.

## Confirmed Capabilities From Source Notes

- operational backup controls
- backup manifests
- backup history
- blob ID display
- Sui receipt history
- wallet-aware signer display
- Walrus-oriented backup architecture
- storage provider controls
- document attestation records

## Conceptual Architecture

```text
Frontend
  ↓
Backend API
  ↓
Storage Provider
  ↓
Sui Receipt / Attestation
  ↓
Storage360 UI
```

## Demo Role

In the demo, Storage360 shows how a business user can trigger or inspect operational backups and receipt records without leaving the business dashboard.

## Why It Matters

Business backups and documents are normally disconnected from verifiable event history. Storage360 connects backup actions and storage records to inspectable receipt trails.
