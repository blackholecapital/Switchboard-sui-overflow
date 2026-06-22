# EILA Factory / Billing360 / PayMe / Sui USDC Work Log

**Instance Summary**

This session focused primarily on:

* Billing360 invoice payment flow
* Embedded PayMe checkout
* Sui USDC payment integration
* Invoice PDF/print behavior
* Invoice Graph Picker preservation/restoration
* Removal of obsolete external payment handoff behavior
* Build stabilization
* Investigation of invoice download payment button behavior

---

# Environment

Repository:

```bash
/mnt/eila-hot-sidecar/switchboard-platform/06-apps/web-crm/frontend/blog-refine-supabase-auth
```

Primary module:

```text
src/modules/billing360
```

---

# Initial Objective

Original goal evolved into:

1. Keep existing Billing360 invoice system intact.
2. Preserve Stripe/Card/Apple Pay/Google Pay.
3. Preserve PayPal/Venmo.
4. Add Sui USDC as an additional payment rail.
5. Avoid replacing existing payment infrastructure.
6. Allow invoice recipients to pay directly from invoice experiences.
7. Embed PayMe checkout into Billing360.
8. Remove dependency on external payment landing redirects where unnecessary.

---

# Sui USDC Integration

## New Constant File

Created:

```text
src/modules/billing360/payme/services/suiUsdc.constants.ts
```

Contents:

```ts
export const SUI_USDC_COIN_TYPE =
  "0xdba34672e30cb065b1f93e3ab55318768fd6fef66c15942c9f7cb846e2f900e7::usdc::USDC";

export const SUI_USDC_DECIMALS = 6;
```

Purpose:

* Define canonical Sui USDC token type.
* Standardize decimal handling.

---

## New Sui Payment Component

Created:

```text
src/modules/billing360/payme/components/SuiUsdcPayButton.tsx
```

Functionality:

* Uses Mysten dapp-kit.
* ConnectButton support.
* Reads current wallet.
* Fetches Sui USDC balances.
* Merges multiple coin objects.
* Splits payment amount.
* Transfers USDC to merchant wallet.
* Returns transaction digest on success.

Observed dependencies:

```ts
@mysten/dapp-kit
@mysten/sui
```

---

# PayMe Checkout Integration

## Component Import

Installed import:

```ts
import SuiUsdcPayButton from './SuiUsdcPayButton';
```

inside:

```text
PayMeCheckout.tsx
```

---

## Embedded Checkout Modal

Billing360 invoices were modified to use:

```ts
setCheckoutOpen(true)
```

instead of forcing external redirects.

Observed:

```ts
const openCheckout = () => setCheckoutOpen(true);
```

Invoice actions now launch an embedded PayMe modal.

---

## Basket Context Injection

Billing360 now passes invoice context into PayMe:

```ts
basketOrder={{
  invoiceId,
  invoiceNo,
  customerName,
  customerEmail,
  subtotal,
  total,
  description,
  onetimeItems
}}
```

Purpose:

* Checkout opens already populated.
* No secondary invoice reconstruction required.

---

# Invoice Document Work

File:

```text
src/modules/billing360/document/InvoiceDocument.tsx
```

---

## Existing Behavior

Invoice document originally rendered:

```tsx
<a href={payUrl}>
  Pay Now
</a>
```

which opened:

```text
https://api.xyz-labs.xyz/checkout/create
```

through a generated handoff URL.

---

## Investigation Result

The PDF/print invoice was still rendering a payment link because:

```tsx
InvoiceDocument.tsx
```

contained its own Pay Now element independent of the embedded checkout modal.

Important discovery:

The visible invoice preview and downloaded invoice are not the same interaction surface.

The embedded checkout worked in Billing360.

The PDF/print invoice still contained a separate payment control.

---

# External Handoff Discovery

Located:

```ts
const paymeHandoffUrl =
  import.meta.env.VITE_PAYME_HANDOFF_URL ||
  "https://api.xyz-labs.xyz/checkout/create";
```

Generated:

```ts
const payUrl = ...
```

inside:

```text
InvoicesTab.tsx
```

---

# Handoff Removal

Attempted removal:

```ts
const paymeHandoffUrl
const payUrl
```

were deleted from:

```text
InvoicesTab.tsx
```

Purpose:

* Eliminate obsolete API handoff path.
* Force invoice payment flow into embedded PayMe checkout.

---

# InvoiceDocument Conversion

Modified:

```tsx
<a href={payUrl}>
```

to:

```tsx
<button onClick={onPayNow}>
```

Result:

InvoiceDocument now accepts:

```ts
onPayNow
```

prop.

InvoicesTab passes:

```ts
onPayNow={openPayNow}
```

to InvoiceDocument.

---

# Build Errors Encountered

After removing payUrl:

Error:

```ts
TS6133
'payUrl' is declared but never read
```

and

```ts
TS2304
Cannot find name 'payUrl'
```

because InvoiceDocument and InvoicesTab still referenced payUrl.

These references required cleanup.

---

# Graph Picker Investigation

User reported:

> Main bucket graph picker disappeared.

Investigation performed against:

```text
InvoicesTab.tsx
```

Confirmed:

```tsx
<Section title="Create Invoice · Graph Picker">
```

still existed.

Confirmed:

```tsx
<CrmGraphPicker
  types={["Companies","Contacts","Deals"]}
  ...
/>
```

still existed.

Observed state:

Graph Picker component was present in source.

Likely issue was data/render behavior rather than component removal.

---

# Historical Recovery References Found

Located backups:

```text
InvoicesTab.tsx.pre-relationship-picker
InvoicesTab.tsx.pre-entity-type-repair
InvoicesTab.tsx.pre-relationship-state-fix
InvoicesTab.tsx.pre-embedded-payme-checkout
InvoicesTab.tsx.pre-invoice-payment-context
InvoicesTab.tsx.pre-payurl-print-repair
InvoicesTab.tsx.pre-remove-external-payurl
```

These provide rollback points.

---

# Build Results

Multiple production builds completed successfully.

Observed outputs:

```text
✓ 12299 modules transformed
✓ 12301 modules transformed
```

Bundles:

```text
dist/assets/index-*.js
```

approximately:

```text
2.2 MB - 2.3 MB
```

Warnings only:

```text
Rollup PURE annotation warnings
Chunk size warnings
```

No blocking compile failures during successful build stages.

---

# Dev Server Issues

Encountered repeated port conflicts.

Ports:

```text
5001
5011
5174
5175
```

Observed processes:

```text
node
```

Required cleanup:

```bash
fuser -k -9
kill -9
```

Repeated Refine Devtools conflicts occurred.

---

# PDF / Print Invoice Improvements

Observed modifications:

Added:

```html
<!doctype html>
```

Injected stylesheets into print window.

Expanded table rendering:

```css
table {
  width: 100%;
}
```

Added:

```css
@media print
```

rules.

Improved standalone invoice rendering.

---

# Current Known State

## Working

* Billing360 loads.
* Invoice editor loads.
* Embedded PayMe modal exists.
* SuiUsdcPayButton component exists.
* Sui USDC transaction code exists.
* Invoice preview renders.
* Builds generally complete.

---

## Verified Remaining Issue

Downloaded / printed invoice still displays a Pay Now control.

Reason:

InvoiceDocument generates a static button inside print content.

A printed document cannot launch the parent React modal because:

```text
window.open()
```

creates a separate document context.

The embedded checkout modal exists only in the parent SPA.

Therefore:

```tsx
onPayNow
```

works in Billing360 UI

but

cannot function inside exported print/PDF documents without a URL destination.

---

# Outstanding Decision

Need to choose one of:

## Option A

Remove Pay Now entirely from PDF.

Result:

Static invoice only.

---

## Option B

Create dedicated invoice payment route.

Example:

```text
/pay/invoice/:invoiceId
```

PDF button links there.

User lands inside embedded PayMe checkout.

Recommended.

---

## Option C

Generate unique public payment URLs.

Example:

```text
/pay/invoice/INV-7976
```

Open invoice-specific checkout page.

Most scalable.

---

# Current Priority

1. Restore/verify Graph Picker data population.
2. Finalize Sui USDC payment path.
3. Replace PDF Pay Now behavior with a real invoice payment route.
4. Remove remaining dead payUrl references.
5. Verify end-to-end invoice recipient payment experience.
6. Preserve Stripe/Card flows as primary payment rails.
7. Keep Sui USDC as supplemental payment option.

---

# Net Result of Session

Major architectural shift completed:

FROM:

```text
Invoice
   ->
External checkout/create redirect
   ->
Separate payment experience
```

TO:

```text
Invoice
   ->
Embedded PayMe Checkout
   ->
Stripe / Card / Wallets / USDC
```

with active work underway to make downloaded invoices follow the same embedded payment architecture.
