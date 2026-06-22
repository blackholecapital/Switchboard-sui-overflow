# Storage360 System Reference

## Status

Current State: OPERATIONAL

Date Verified:
2026-06-20

Verified Upload:

Blob ID:
c1Qlr34KNjUpILN__RNJdMQV9pRqBBViaUfh9Doa9bU

Transaction:
AZacpdsugkr7w5u4g7zE56tBmXesNaJhRB6Kua4Wk5Kn

Wallet:
0xcfd61e24943e826f61dca8f6ec540bb505c9da72d7a53b2637dbd332b2e7f6b5

---

# Architecture

Storage360 consists of:

Frontend
↓
Vite Proxy
↓
Storage360 Backend API
↓
Walrus Service
↓
Sui Mainnet
↓
Walrus Storage

---

# Runtime Ports

## Frontend

Port:

5174

Process:

refine dev
vite dev

Proxy:

/crm-api/*
→
http://127.0.0.1:4010/api/*

File:

frontend/blog-refine-supabase-auth/vite.config.ts

Proxy Configuration:

target:
http://127.0.0.1:4010

rewrite:
crm-api → api

---

## Storage360 Backend

Port:

4010

Health Endpoint:

GET

/api/storage360/health

Response:

{
"ok": true,
"service": "storage360"
}

Upload Endpoint:

POST

/api/storage360/upload

Overview Endpoint:

GET

/api/storage360/overview

Backup List:

GET

/api/storage360/backups

Blob Retrieval:

GET

/api/storage360/blob/:id

---

## Core Runtime

Port:

3000

Purpose:

Factory runtime

NOT Storage360

Requests sent here return:

{
"ok": false,
"error": "not_found"
}

This caused confusion during troubleshooting.

Storage360 lives on:

4010

NOT

3000

---

# Source Files

## Frontend Page

frontend/blog-refine-supabase-auth/src/modules/storage360/page.tsx

Contains:

Create Backup button

Backup status handling

Storage360 UI wiring

---

## Frontend Upload Helper

frontend/blog-refine-supabase-auth/src/shared/blockchain/walrus/uploadBlob.ts

Contains:

uploadBlobToWalrus()

Posts to:

/crm-api/storage360/upload

---

## Backend Route

backend/routes/storage360.js

Registers:

/api/storage360/health

/api/storage360/upload

/api/storage360/backups

/api/storage360/overview

/api/storage360/blob/:id

---

## Walrus Integration

backend/services/storage360/walrus.service.js

Responsible for:

Wallet loading

Walrus uploads

Transaction creation

Epoch configuration

Metadata generation

Explorer links

Aggregator links

---

## Backup Persistence

backend/services/storage360/storage360.bucket.js

Persists:

data/storage360/backups.json

Functions:

listBackups()

saveBackup()

getStorage360Overview()

---

# Storage Locations

## Backup Index

data/storage360/backups.json

Contains:

blob ids

transaction digests

wallet metadata

storage metadata

timestamps

---

## Backup Directory

data/storage360/

Created automatically if missing.

---

# Wallet

Environment Variable:

STORAGE360_SUI_PRIVATE_KEY

Runtime Wallet:

0xcfd61e24943e826f61dca8f6ec540bb505c9da72d7a53b2637dbd332b2e7f6b5

Loaded From:

Process Environment

Confirmed Present:

YES

---

# Root Cause of Failure

Original Configuration:

epochs: 3

Location:

backend/services/storage360/walrus.service.js

Error:

MoveAbort
0x2::balance::split
abort code 2

Meaning:

Wallet balance insufficient for requested storage allocation.

Fix Applied:

epochs: 1

Result:

Successful upload

Successful certification

Successful persistence

Successful UI update

---

# Verification Commands

Health:

curl http://127.0.0.1:4010/api/storage360/health

Upload Test:

curl -X POST 
http://127.0.0.1:4010/api/storage360/upload 
-H "Content-Type: application/json" 
-d '{"kind":"test"}'

Backups:

curl http://127.0.0.1:4010/api/storage360/backups

Overview:

curl http://127.0.0.1:4010/api/storage360/overview

---

# Known Backup Files

backend/routes/storage360.js

backend/routes/storage360.js.bak

backend/routes/storage360.js.debug

backend/routes/storage360.js.broken

backend/routes/storage360.js.pre-real-walrus

backend/routes/storage360.js.pre-global-persistence

These are historical snapshots and are not loaded at runtime.

Runtime File:

backend/routes/storage360.js

Only.

---

# Recovery Procedure

If backups stop working:

1. Verify health endpoint

4010/api/storage360/health

2. Verify wallet key exists

STORAGE360_SUI_PRIVATE_KEY

3. Test upload endpoint directly

4. Inspect walrus.service.js

5. Confirm epoch count

6. Confirm wallet has SUI balance

7. Restart backend

8. Retest upload

---

# Current Operational State

Frontend:
Operational

Proxy:
Operational

Backend:
Operational

Walrus:
Operational

Persistence:
Operational

Backup Button:
Operational

Transaction Recording:
Operational

Blob Recording:
Operational

Storage360:
ONLINE
