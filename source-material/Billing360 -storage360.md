# Switchboard / Billing360 / Storage360 Handoff Report

Date: 2026-06-20

Purpose: handoff for the next ChatGPT/agent instance. This documents exactly what was debugged and repaired around Billing360, Storage360, Sui USDC payment TXIDs, Walrus attestations, nginx routing, Cloudflare worker callback behavior, and the local server state.

## 1. High-level result

Main goal for this session:

**Pay invoice with Sui USDC -> mark Billing360 invoice paid -> capture TXID -> create Storage360/Walrus attestation.**

Current status:

- Billing360 invoice paid status is working.
- The backend `mark-paid` route is working.
- The public Cloudflare/nginx route to `mark-paid` is working.
- The Sui USDC path can send `txDigest` into the backend.
- Storage360/Walrus upload works once `STORAGE360_SUI_PRIVATE_KEY` is present in the running Node process.
- Storage360 now shows new `invoice_attestation` records and Sui Mainnet transaction records.
- Billing360 still needs UI polish to show the TXID / Walrus blob in the right-side invoice detail panel.
- Storage360 still needs UI polish to show invoice number labels like `INV-9166.attestation` instead of generic `invoice_attestation`.

Do not reopen the payment plumbing unless a test proves it broke.

## 2. Server and important paths

SSH target used:

```bash
ssh blackhole@100.104.23.59
```

Main host prompt:

```text
xyz-Factory
```

Main project root:

```bash
/mnt/eila-hot-sidecar/switchboard-platform
```

Backend path:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend
```

Frontend path:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth
```

Checkout frontend clone path:

```bash
/mnt/eila-hot-sidecar/github/switchboard-invoice-checkout
```

Important backend files:

```text
06-apps/web-crm/backend/server.js
06-apps/web-crm/backend/routes/invoices.js
06-apps/web-crm/backend/routes/storage360.js
06-apps/web-crm/backend/routes/stripe.js
06-apps/web-crm/backend/services/storage360/walrus.service.js
06-apps/web-crm/backend/services/storage360/storage360.bucket.js
06-apps/web-crm/backend/data/storage360/backups.json
```

## 3. Public URLs and routing truth

Billing360 UI:

```text
https://switchboard.xyz-labs.xyz/billing360
```

Storage360 UI:

```text
https://switchboard.xyz-labs.xyz/storage360
```

Correct external CRM API base:

```text
https://switchboard.xyz-labs.xyz/crm-api
```

Correct external mark-paid route:

```text
https://switchboard.xyz-labs.xyz/crm-api/invoices/:id/mark-paid
```

Correct internal mark-paid route:

```text
http://127.0.0.1:4010/api/invoices/:id/mark-paid
```

Do **not** use:

```text
https://switchboard.xyz-labs.xyz/crm-api/api/invoices/:id/mark-paid
```

Why: nginx already maps `/crm-api/` to `/api/`.

Observed nginx mapping:

```nginx
location /crm-api/ {
    proxy_pass http://127.0.0.1:4010/api/;
}
```

This file was identified:

```bash
/etc/nginx/conf.d/switchboard-crm.conf
```

External route was confirmed working:

```bash
curl -i \
-X POST \
https://switchboard.xyz-labs.xyz/crm-api/invoices/756105dc-3030-4a46-9376-c1c4bdda4417/mark-paid \
-H "Content-Type: application/json" \
-d '{}'
```

Result:

```json
{"ok":true}
```

## 4. Backend server truth

Backend entry:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend/server.js
```

Backend listens on:

```js
app.listen({ host: "127.0.0.1", port: 4010 });
```

Observed process after restart:

```text
LISTEN 0 511 127.0.0.1:4010 ... users:(("node",pid=110816,fd=24))
```

Database config in `server.js`:

```js
const db = new pg.Pool({
  host: "127.0.0.1",
  port: 54329,
  database: "switchboard",
  user: "switchboard_admin",
  password: "change_me",
});
```

`server.js` has a route mirroring wrapper that mirrors `/api/...` routes to `/crm-api/...` internally:

```js
for (const method of ["get", "post", "put", "patch", "delete"]) {
  const original = app[method].bind(app);
  app[method] = (path, ...args) => {
    original(path, ...args);
    if (typeof path === "string" && path.startsWith("/api/")) {
      original(path.replace("/api/", "/crm-api/"), ...args);
    }
    return app;
  };
}
```

## 5. Invoice route patch that was applied

File:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend/routes/invoices.js
```

Current top imports after repair:

```js
import { uploadJsonToWalrus } from "../services/storage360/walrus.service.js";
import { saveBackup } from "../services/storage360/storage360.bucket.js";
import crypto from "crypto";
import { executeWorkflow } from "../services/workflow-engine.js";
```

These imports were verified with:

```bash
grep -n "uploadJsonToWalrus\\|saveBackup\\|import crypto" routes/invoices.js
```

Output:

```text
1:import { uploadJsonToWalrus } from "../services/storage360/walrus.service.js";
2:import { saveBackup } from "../services/storage360/storage360.bucket.js";
3:import crypto from "crypto";
```

Original problem: `/api/invoices/:id/mark-paid` only updated invoice/payment/event rows and returned `{ ok: true }`. It did not call Storage360/Walrus.

The route was located around line 337:

```bash
grep -n "mark-paid" routes/invoices.js
# 337: app.post("/api/invoices/:id/mark-paid", ...
```

The patch inserted this attestation block before `return { ok: true };`:

```js
const inv = await db.query(`
  select id, invoice_no, client, total
  from billing.invoices
  where id=$1
  limit 1
`,[invoiceId]);

const invoice = inv.rows[0];

if (invoice && b.txDigest) {
  try {
    const attestation = {
      type: "invoice_payment_attestation",
      invoiceId: invoice.id,
      invoiceNo: invoice.invoice_no,
      client: invoice.client,
      total: invoice.total,
      paymentProvider: b.payment_provider || "sui_usdc",
      txDigest: b.txDigest,
      paidAt: new Date().toISOString()
    };

    const upload = await uploadJsonToWalrus(attestation);

    saveBackup({
      id: crypto.randomUUID(),
      kind: "invoice_attestation",
      fileName: `${invoice.invoice_no}.attestation`,
      invoiceId: invoice.id,
      invoiceNo: invoice.invoice_no,
      txDigest: upload.txDigest,
      blobId: upload.blobId,
      blobObjectId: upload.blobObjectId,
      aggregatorUrl: upload.aggregatorUrl,
      explorerUrl: upload.explorerUrl,
      status: "stored",
      provider: "walrus",
      createdAt: new Date().toISOString()
    });
  } catch (err) {
    console.error("INVOICE_ATTESTATION_FAILED", err);
  }
}
```

Patch command that successfully ran:

```bash
cd /mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend

python3 <<'PY'
from pathlib import Path

p = Path("routes/invoices.js")
s = p.read_text()

old = """]); return { ok: true };
  });
}"""

new = """
    ]);

    const inv = await db.query(`
      select id, invoice_no, client, total
      from billing.invoices
      where id=$1
      limit 1
    `,[invoiceId]);

    const invoice = inv.rows[0];

    if (invoice && b.txDigest) {
      try {
        const attestation = {
          type: "invoice_payment_attestation",
          invoiceId: invoice.id,
          invoiceNo: invoice.invoice_no,
          client: invoice.client,
          total: invoice.total,
          paymentProvider: b.payment_provider || "sui_usdc",
          txDigest: b.txDigest,
          paidAt: new Date().toISOString()
        };

        const upload = await uploadJsonToWalrus(attestation);

        saveBackup({
          id: crypto.randomUUID(),
          kind: "invoice_attestation",
          fileName: `${invoice.invoice_no}.attestation`,
          invoiceId: invoice.id,
          invoiceNo: invoice.invoice_no,
          txDigest: upload.txDigest,
          blobId: upload.blobId,
          blobObjectId: upload.blobObjectId,
          aggregatorUrl: upload.aggregatorUrl,
          explorerUrl: upload.explorerUrl,
          status: "stored",
          provider: "walrus",
          createdAt: new Date().toISOString()
        });
      } catch (err) {
        console.error("INVOICE_ATTESTATION_FAILED", err);
      }
    }

    return { ok: true };
  });
}"""

if old not in s:
    raise SystemExit("PATCH_TARGET_NOT_FOUND")

p.write_text(s.replace(old,new,1))
print("PATCHED")
PY
```

Result:

```text
PATCHED
```

Syntax check:

```bash
node -c routes/invoices.js
```

Result: no output. File parses.

## 6. Storage360 Walrus private key issue

After patch, initial test returned `{ok:true}` but attestation failed:

```text
INVOICE_ATTESTATION_FAILED Error: Missing STORAGE360_SUI_PRIVATE_KEY
    at getSigner (.../services/storage360/walrus.service.js:13:11)
    at uploadJsonToWalrus (.../services/storage360/walrus.service.js:22:18)
    at Object.<anonymous> (.../routes/invoices.js:375:30)
```

Confirmed no env var in process shell at that moment:

```bash
env | grep STORAGE360
```

Output: empty.

Relevant file:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend/services/storage360/walrus.service.js
```

Key code:

```js
function getSigner() {
  if (!process.env.STORAGE360_SUI_PRIVATE_KEY) {
    throw new Error("Missing STORAGE360_SUI_PRIVATE_KEY");
  }

  return Ed25519Keypair.fromSecretKey(
    process.env.STORAGE360_SUI_PRIVATE_KEY
  );
}
```

User provided the key:

```bash
export STORAGE360_SUI_PRIVATE_KEY="suiprivkey1qrlh6aaglmgpzvd3rlsqnzu8fyrf3egrhv4334utryayrt2qacg4jahqqng"
```

Important: the backend must be restarted from a shell where this env var is exported.

Restart command used/needed:

```bash
export STORAGE360_SUI_PRIVATE_KEY="suiprivkey1qrlh6aaglmgpzvd3rlsqnzu8fyrf3egrhv4334utryayrt2qacg4jahqqng"

PID=$(ss -lntp | grep 4010 | grep -o 'pid=[0-9]*' | cut -d= -f2)
kill $PID

cd /mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/backend
nohup node server.js > server.log 2>&1 &

sleep 3
ss -lntp | grep 4010
```

Long-term fix needed: persist `STORAGE360_SUI_PRIVATE_KEY` in actual service startup environment, not only interactive shell. Use `.env` plus loader, PM2 ecosystem env, systemd `Environment=`, or a wrapper script.

## 7. Manual tests

Local mark-paid with test TXID:

```bash
curl -X POST \
http://127.0.0.1:4010/api/invoices/756105dc-3030-4a46-9376-c1c4bdda4417/mark-paid \
-H "Content-Type: application/json" \
-d '{
  "txDigest":"TEST-WALRUS-TX-001",
  "payment_provider":"sui_usdc",
  "amount":1
}'
```

Expected:

```json
{"ok":true}
```

After key and restart, Storage360 visibly showed:

- Backup count increased.
- Walrus Objects increased.
- Sui Receipts increased.
- New `invoice_attestation` record appeared in Backup History.
- New `invoice_attestation` record appeared in Local Attestation Records.
- New `invoice_attestation` record appeared in Walrus Blob Records.
- New `invoice_attestation` record appeared in Sui Chain Records.

## 8. Known invoice IDs

Important invoice:

```text
Invoice no: INV-9166
Invoice UUID: 756105dc-3030-4a46-9376-c1c4bdda4417
Status after work: paid
```

Another paid test invoice:

```text
Invoice no: INV-6047
Status after work: paid
```

## 9. Storage360 current behavior

Storage360 UI now shows records but labels are not ideal.

Current new record label seen:

```text
invoice_attestation
```

Older records still show:

```text
billing360.invoice.attestation
```

Desired display:

```text
INV-9166.attestation
```

Backend now saves:

```js
fileName: `${invoice.invoice_no}.attestation`
invoiceNo: invoice.invoice_no
kind: "invoice_attestation"
```

Likely UI issue: Storage360 frontend is displaying `record.kind` instead of `record.fileName`.

Next patch should search frontend:

```bash
cd /mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth

grep -RIn "invoice_attestation\\|billing360.invoice.attestation\\|fileName\\|invoiceNo\\|kind\\|Storage360" src/modules/storage360 src 2>/dev/null
```

Likely frontend title logic should become:

```ts
const title =
  record.fileName ||
  record.invoiceNo ||
  record.name ||
  record.kind ||
  "record";
```

## 10. Billing360 current behavior

Billing360 invoice list shows invoice paid correctly.

Right-side invoice details still show generic data:

```text
Attestation / Record Data
id: 756105dc-3030-4a46-9376-c1c4bdda4417
owner: current user
created: Jun 20, 2026
hash: pending
```

This is not reading the payment/attestation record.

Fastest useful UI patch:

- For selected invoice, fetch:
  ```text
  GET /crm-api/invoices/:id/payments
  ```
- Show latest:
  ```text
  payment_reference
  payment_provider
  amount
  currency
  created_at
  ```
- `payment_reference` contains the incoming `txDigest` from the Sui payment path.

Existing backend route in `routes/invoices.js`:

```js
app.get("/api/invoices/:id/payments", async (req) => {
  const { rows } = await db.query(`
    select *
    from billing.invoice_payments
    where invoice_id=$1
    order by created_at desc
  `,[req.params.id]);

  return rows;
});
```

Also available:

```text
GET /crm-api/invoices/:id/timeline
```

Recommended Billing360 panel display:

```text
Payment Provider: sui_usdc
Status: paid
TX Digest: <payment_reference>
Explorer: https://suiscan.xyz/mainnet/tx/<payment_reference>
Walrus Blob: <blobId if found from Storage360 records>
Attestation: INV-9166.attestation
```

Frontend search to find the right panel:

```bash
cd /mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth

grep -RIn "Attestation / Record Data\\|hash: pending\\|owner: current user\\|Invoice Details" src 2>/dev/null
```

## 11. Checkout frontend repo facts

Repo cloned at:

```bash
/mnt/eila-hot-sidecar/github/switchboard-invoice-checkout
```

File tree observed:

```text
BILLING360_LINK_SNIPPET.md
components/InvoiceSummary.tsx
components/SuiPaymentPanel.tsx
components/WalletConnect.tsx
hooks/useInvoice.ts
hooks/useSuiPayment.ts
index.html
invoice.ts
lib/payment.ts
package.json
pages/FailurePage.tsx
pages/SuccessPage.tsx
README.md
src/app.tsx
src/config.ts
src/main.tsx
src/style.css
src/wallet.ts
vite.config.ts
```

This repo is frontend only. It does not include the Cloudflare Worker source.

Search results showed:

```text
./README.md:50:VITE_CARD_CHECKOUT_URL=https://api.xyz-labs.xyz/checkout/create
./src/app.tsx:13:  CARD_CHECKOUT_URL,
./src/app.tsx:103:    const response = await fetch(CARD_CHECKOUT_URL, {
./src/app.tsx:118:        callback_url:
./src/app.tsx:119:          "https://switchboard.xyz-labs.xyz/crm-api/invoices/mark-paid",
./src/app.tsx:228:          `${callbackBase}/invoices/${encodeURIComponent(callbackInvoiceId)}/mark-paid`,
./src/config.ts:11:export const CARD_CHECKOUT_URL =
./src/config.ts:12:  import.meta.env.VITE_CARD_CHECKOUT_URL ||
./src/config.ts:13:  "https://api.xyz-labs.xyz/checkout/create";
./src/config.ts:18:  "https://switchboard.xyz-labs.xyz/crm-api";
```

Sui path appears to call:

```ts
`${callbackBase}/invoices/${encodeURIComponent(callbackInvoiceId)}/mark-paid`
```

This is the important path for the current demo.

There is still a likely stale card callback URL lacking invoice ID:

```text
https://switchboard.xyz-labs.xyz/crm-api/invoices/mark-paid
```

Do not spend time on this unless card/Stripe callback is explicitly in scope. User said current goal is USDC paid + TXID.

## 12. Cloudflare Worker facts

Cloudflare worker name:

```text
checkout-worker
```

Cloudflare dashboard showed repo:

```text
blackholecapital/Checkout-worker
```

Worker variables shown after repair:

```text
CALLBACK_SIGNING_SECRET
CALLBACK_URL
CORS_ORIGINS
DEFAULT_CANCEL_URLS
DEFAULT_SUCCESS_URLS
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
```

Worker route behavior:

```js
if (url.pathname === "/checkout/create" && request.method === "POST") {
  return withCors(await handleCreateCheckout(request, env), env, requestOrigin);
}

if (url.pathname === "/webhooks/stripe" && request.method === "POST") {
  return await handleStripeWebhook(request, env);
}

if (url.pathname === "/checkout/status" && request.method === "GET") {
  return withCors(await handleGetStatus(request, env), env, requestOrigin);
}
```

Worker stores Stripe webhook status like:

```js
const stored = {
  checkout_id: checkoutId,
  stripe_session_id: object.id,
  txDigest: object.payment_intent || object.id,
  payment_provider: "stripe",
  amount: object.amount_total ? Number(object.amount_total) / 100 : 0,
  currency: object.currency ? object.currency.toUpperCase() : "USD",
  status,
  mode: object.mode || "payment",
  email: object.customer_details?.email || object.customer_email,
  payment_status: paymentStatus,
  updated_at: new Date().toISOString()
};
```

Again: not current priority unless user asks for Stripe/card.

## 13. Disk/ops notes

Root disk was cleaned earlier.

Large files found:

```text
/swapfile                                                        34G
/usr/share/ollama/.ollama/models/blobs/...                       8.9G
/opt/eila-os/tracer-foundation-2026-06-20.tar.gz                 7.0G
/opt/eila-os/factory-xyz-root-backup/...                         2.6G
/usr/local/lib/ollama/cuda_v12/libggml-cuda.so                   1.8G
```

Actions taken:

```bash
sudo swapoff /swapfile
sudo rm -f /swapfile
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

rm -f /opt/eila-os/tracer-foundation-2026-06-20.tar.gz
sudo dnf clean all
sudo rm -rf /var/cache/dnf/*
sudo rm -rf /var/log/pcp/*
sync
```

Root result after cleanup:

```text
/dev/mapper/rl_cowboybutts-root 70G 38G 33G 54% /
```

RAID state:

```text
md127 active raid6 [8/7] [_UUUUUUU]
State: active, degraded
Active Devices: 7
Working Devices: 7
Failed Devices: 0
Spare Devices: 0
```

This is an operational risk but not part of demo payment path.

## 14. Frontend build note

Frontend path:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth
```

Build command:

```bash
npm run build
```

Build succeeded after earlier StoragePrimitives issue was repaired.

Warning observed:

```text
Some chunks are larger than 500 kB after minification.
```

This is not fatal.

Earlier issue:

```text
src/modules/storage360/tabs/Providers/ProvidersTab.tsx:2:10 - error TS2305:
Module '"../../components/StoragePrimitives"' has no exported member 'Provider'.
```

This was already resolved enough for build success. Do not reopen unless build fails again.

## 15. Next recommended minimal tasks

If continuing before contest/demo, do these only:

1. **Storage360 label fix**
   - Show `record.fileName || record.invoiceNo || record.kind`.
   - Goal: show `INV-9166.attestation` instead of `invoice_attestation`.

2. **Billing360 TXID display**
   - In invoice detail panel, fetch `/crm-api/invoices/:id/payments`.
   - Show latest `payment_reference` as Sui TX digest.
   - Add Sui explorer link:
     ```text
     https://suiscan.xyz/mainnet/tx/<payment_reference>
     ```

3. **Optional Billing360 attestation display**
   - Fetch `/crm-api/storage360/backups`.
   - Filter by selected invoice ID or invoice number.
   - Show `blobId`, `aggregatorUrl`, `explorerUrl`.

Do not re-architect primitives today.

## 16. Architecture note for future cleanup

The user's intended model:

```text
Primitive Bucket
  -> Tagged Objects
  -> Unified Retrieval
  -> Every View
```

Current implementation still has multiple data paths:

```text
billing.invoices
billing.invoice_payments
billing.invoice_events
data/storage360/backups.json
Walrus blobs
frontend-specific view models
older dashboard mock data
```

This is why views drift.

Future agents should not add more adapters unless absolutely necessary. Follow one canonical bucket/path and tags.

Suggested future tags:

```text
invoice
invoice:INV-9166
invoice_id:756105dc-3030-4a46-9376-c1c4bdda4417
customer:forrest-gump
payment:sui_usdc
attestation
walrus
sui
txdigest:<digest>
```

## 17. Behavior guidance for next AI instance

The user is tired, under deadline pressure, and needs exact execution, not broad commentary.

Rules:

- Do not drift.
- Do not reopen solved problems.
- Do not touch Stripe/card path unless explicitly asked.
- Do not touch nginx unless routing breaks.
- Do not hand-edit with nano unless user asks.
- When user asks for bash, provide bash.
- Use small patch commands with backups.
- Always cite exact file paths.
- Treat current priority as demo readiness, not perfect architecture.
- The current working path is USDC -> mark paid -> TXID -> Walrus/Storage360.

## 18. Current truth

Completed:

- Payment route works.
- External route works.
- `routes/invoices.js` patched.
- Backend syntax checked.
- Env var issue identified.
- Env var provided.
- Storage360 shows new records after key/restart.
- Billing360 shows paid invoices.

Not completed:

- Billing360 TXID display.
- Storage360 invoice-number label display.
- Unified primitive/tag system.
- Card/Stripe callback cleanup.
