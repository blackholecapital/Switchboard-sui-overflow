# Storage360 – Mainnet Walrus Integration Status Report

**Date:** 2025-06-19
**Project:** Switchboard CRM → Storage360
**Environment:** Production/Mainnet Only

---

# Executive Summary

Storage360 was originally operating with a mocked upload provider that generated fake blob IDs.

During this session:

1. Walrus SDK was installed and validated.
2. Sui SDK compatibility issues were investigated.
3. A funded Sui/WAL wallet was connected to backend signing.
4. Real Walrus uploads were successfully executed.
5. Real Sui Mainnet objects were created.
6. Storage360 backend now returns real Walrus metadata.
7. Frontend/backend routing issues were identified and corrected.
8. Persistence requirements were clarified.
9. Remaining work is primarily persistence, retrieval, dashboard metrics, and UX cleanup.

The system has successfully written data to Walrus Mainnet and created corresponding Sui objects.

---

# Major Discoveries

## SDK Versions

Backend:

```json
{
  "@mysten/sui": "2.19.0",
  "@mysten/walrus": "1.2.1"
}
```

Confirmed:

```bash
import { SuiGrpcClient } from "@mysten/sui/grpc"
import { Ed25519Keypair } from "@mysten/sui/keypairs/ed25519"
import { walrus } from "@mysten/walrus"
```

all load successfully.

---

# Initial Walrus Investigation

Verified SDK contains:

```ts
client.walrus.writeBlob()
```

Confirmed from SDK documentation:

```ts
await client.walrus.writeBlob({
  blob,
  deletable:false,
  epochs:3,
  signer
});
```

Walrus package configuration is automatically selected using:

```ts
new SuiGrpcClient(...)
  .$extend(walrus())
```

for Mainnet/Testnet.

---

# Wallet Testing

Generated test wallet:

```text
0xcfd61e24943e826f61dca8f6ec540bb505c9da72d7a53b2637dbd332b2e7f6b5
```

Verified private key format:

```text
suiprivkey1...
```

Confirmed:

```ts
Ed25519Keypair.fromSecretKey(...)
```

works correctly.

---

# First Real Failure

Walrus upload initially failed:

```text
Insufficient balance of WAL
Required: 17971317
Available: 0
```

Conclusion:

Wallet contained no WAL.

---

# Wallet Funding

Wallet later funded.

Walrus upload moved beyond balance checks and reached chain execution.

---

# Real Walrus Upload Success

Successful upload response:

```json
{
  "provider":"walrus",
  "network":"Sui Mainnet",
  "blobId":"kyGvSHndhLOAIzDxsOzYwN0HPAbjIL-05nuUYfp0FBQ",
  "walletAddress":"0xcfd61e24943e826f61dca8f6ec540bb505c9da72d7a53b2637dbd332b2e7f6b5",
  "registeredEpoch":33,
  "certifiedEpoch":33
}
```

This proves:

* Upload reached Walrus
* Blob registered
* Blob certified
* Storage allocated
* Mainnet transaction succeeded

---

# Example Mainnet Object

Observed:

```json
{
  "blobObjectId":"0x8d5af599348bb45c3cbf97b94bb5dc7142bd662e2b3fa502943d39f20d5859c0"
}
```

Storage object:

```json
{
  "storageObjectId":"0x91a5d603a129e66468b93b104e956d33d28f479302c545e2f54df96fa3df8c97"
}
```

Explorer URL generated:

```text
https://suiscan.xyz/mainnet/object/<objectId>
```

Walrus aggregator URL generated:

```text
https://aggregator.walrus-mainnet.walrus.space/v1/blobs/<blobId>
```

---

# Storage Cost Discovery

Observed allocation:

```json
{
  "storage_size":"66034000",
  "start_epoch":33,
  "end_epoch":36
}
```

For tiny test blobs.

This indicates:

* Storage allocation is prepaid.
* Cost is not a one-time platform fee.
* Cost scales with storage size and epoch duration.

Exact WAL economics still need documentation.

---

# Frontend Upload Routing Issue

Frontend was originally posting to:

```ts
/api/storage360/upload
```

However nginx was configured:

```nginx
location ^~ /crm-api/ {
    proxy_pass http://127.0.0.1:4010/api/;
}
```

Correct public endpoint:

```ts
/crm-api/storage360/upload
```

This mismatch caused uploads to silently fail.

Fixed by replacing:

```ts
/api/storage360/
```

with:

```ts
/crm-api/storage360/
```

through source tree.

---

# Mock Provider Period

Temporary local provider returned:

```json
{
  "ok":true,
  "blobId":"blob-xxxxxxxx",
  "provider":"local"
}
```

This was used to validate UI flows before Mainnet integration.

Later replaced by Walrus provider.

---

# Current Persistence Problem

Current backup history is NOT persistent.

Reason:

History is currently browser-local.

Observed behavior:

* Open new browser
* Open new machine
* Login
* No backup history visible

This is unacceptable for judging/demo.

---

# Correct Persistence Model

Persistence must NOT be wallet-based.

Reason:

Judges will not connect platform wallet.

Users should see:

* Backup history
* Storage records
* Attestations
* Transaction history

regardless of wallet.

Correct model:

```text
Switchboard Account
    |
    +-- Storage360 Records
            |
            +-- Walrus Blob IDs
            +-- Sui Object IDs
            +-- Explorer URLs
            +-- Status
```

Wallet is authorization.

Wallet is NOT persistence.

---

# Desired Persistent Storage

Recommended:

```text
backend/data/storage360/backups.json
```

or

```text
Postgres table
```

Every upload appends:

```json
{
  "createdAt":"",
  "blobId":"",
  "blobObjectId":"",
  "storageObjectId":"",
  "explorerUrl":"",
  "aggregatorUrl":"",
  "status":"stored"
}
```

Frontend loads:

```http
GET /api/storage360/history
```

and displays global history.

---

# Backend Service Discovery

Critical finding:

Port 4010 backend is launched by systemd.

Located:

```text
/etc/systemd/system/switchboard-crm-backend.service
```

Contains:

```ini
ExecStart=/usr/bin/node server.js
```

This explains why manually exporting environment variables did not affect the running backend.

---

# Root Cause of Missing Private Key

Running process:

```bash
PID 1663700
```

Environment inspection:

```bash
tr '\0' '\n' < /proc/1663700/environ
```

showed:

```text
STORAGE360_SUI_PRIVATE_KEY
MISSING
```

Therefore uploads returned:

```json
{
  "ok":false,
  "error":"Missing STORAGE360_SUI_PRIVATE_KEY"
}
```

even though the shell had the variable.

---

# Remaining Work

## Priority 1

Persistent history API

Endpoints:

```http
GET  /api/storage360/history
POST /api/storage360/upload
```

Store every successful upload.

---

## Priority 2

Backup retrieval

Add:

```http
GET /api/storage360/blob/:blobId
```

Return metadata.

---

## Priority 3

Document upload

Current uploads are JSON manifests.

Need:

```text
PDF Upload
DOCX Upload
Invoice Upload
Backup Package Upload
```

to Walrus.

---

## Priority 4

Storage Dashboard

Display:

* Total blobs
* Total storage used
* Remaining allocation
* Last backup
* Last attestation

---

## Priority 5

Attestation Enhancements

Current attestation records exist locally.

Need:

* persistent backend storage
* explorer links
* blob links

---

## Priority 6

Automated Backups

Desired:

```text
Daily
Weekly
Monthly
```

scheduler.

Flow:

```text
Generate manifest
Upload to Walrus
Save history
Create attestation
Notify user
```

---

# Current State Assessment

Status: FUNCTIONAL

Confirmed Working:

* Frontend
* Backend
* Walrus SDK
* Sui SDK
* Mainnet uploads
* Blob creation
* Blob certification
* Sui object creation
* Explorer URL generation
* Aggregator URL generation

Missing:

* Persistent history
* Retrieval UI
* Storage metrics
* PDF/document workflows
* Automated backups
* Production cleanup

---

# Demo Readiness

Today:

8/10

Tomorrow after persistence + retrieval:

10/10

The hardest part (real Mainnet Walrus upload and Sui object creation) is already complete.
