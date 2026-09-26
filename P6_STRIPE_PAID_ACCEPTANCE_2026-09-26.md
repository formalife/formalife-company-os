# P6 Stripe Paid Acceptance

Status: CURRENT TEST — PAID ACCEPTANCE HARNESS READY; FRESH SINGLE RUN REQUIRED AFTER READINESS REMEDIATION
Date: 2026-09-26

## Context

The signed-expiry gate is already closed / PASS in `P6_STRIPE_WEBHOOK_STAGING_DEPLOYMENT_2026-09-25.md` from Cloudflare staging run `36231789069` (#11), proving a genuine signed `checkout.session.expired` path with Order cancellation, reservation release, restored capacity, zero PaymentRecord, zero Enrollment and verified public `CANCELLED` state.

The first remaining money-path bottleneck is a genuine Stripe Sandbox successful payment producing a signed `checkout.session.completed` delivery linked to the real Formalife Order and protected reservation.

## Paid acceptance harness — PR #37

**TEST — IMPLEMENTED AND MERGED.**

Implementation repository: `formalife/platform`.

- PR #37 — `Add real paid Stripe staging acceptance`;
- merge commit: `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`;
- new script: `scripts/p6-stripe-paid-webhook-acceptance.mjs`;
- `cloudflare-staging` accepts `paid_acceptance = none | single | couple`;
- PR bootstrap run `36232644170`: PASS;
- PR P6 commerce-contract run `36232644167`: PASS.

The paid acceptance:

1. creates a fresh synthetic confirmed Twenty edition;
2. starts checkout through the deployed public Formalife endpoint;
3. requires a real Stripe Sandbox Checkout Session with the canonical offer amount/currency/seat count;
4. verifies the linked Twenty Order and protected reservation before payment;
5. exposes the hosted Stripe Sandbox Checkout URL for the one required interactive sandbox payment;
6. polls Stripe until the Session is `complete` and `payment_status=paid`;
7. then verifies the genuine signed webhook operational effects;
8. requires Order `PAID`, one PaymentRecord, exact Enrollment count, consumed reservation/capacity and public verified `PAID` state.

No production payment credentials or card data are committed. Hosted Checkout is not bypassed with detached synthetic events.

## Run #12 — first requested SINGLE attempt

**RESULT — FAILED BEFORE PAID ACCEPTANCE; NO SUCCESSFUL-PAYMENT CLAIM.**

- workflow: `cloudflare-staging`;
- run: `36233877518`;
- run number: `12`;
- attempt: `1`;
- commit: `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`;
- job: `108381999964`;
- conclusion: `FAILURE`.

Passed before the failure:

- runner/repository/bootstrap;
- Astro production build;
- commerce Worker deploy;
- `COMMERCE_BOUNDARY` Service Binding configuration;
- web Worker deploy and runtime-secret installation;
- direct deployed commerce coordinator probe;
- complete deployed serialized-capacity probe.

The failure occurred in the already-existing signed-expiry step before the new paid step could execute:

`Public checkout failed (401): {"error":"UNAUTHORIZED","message":"Commerce boundary HTTP 401"}`

The direct authenticated commerce probe immediately before that public checkout passed with the newly generated boundary token. Therefore the commerce Worker itself accepted the current token while the web Worker path still reached it with a non-matching token value.

**DIAGNOSIS — DEPLOYMENT READINESS / SECRET-PROPAGATION RACE.**

The staging workflow rotates `FORMALIFE_BOUNDARY_TOKEN` on each run, redeploys both Workers and begins the public commerce acceptance almost immediately. Run #12 exposed that direct commerce readiness did not prove that the web Worker's runtime secret had converged to the same token yet.

This is not evidence that the signed-expiry logic regressed: run #11 remains the accepted signed-expiry PASS. It is also not a successful-payment failure because the paid step was skipped after the upstream expiry-step failure and no paid Checkout Session was created by that step.

### Provenance correction

The founder explicitly launched this attempt intending `paid_acceptance=single`.

The final run artifact showed `paidAcceptanceRequested: none`, but that field was populated from `P6_PAID_OPTION`, which existed only in the paid step's local environment. Because the paid step was skipped after the upstream failure, the later evidence-writing step could not see that variable. Therefore `paidAcceptanceRequested: none` does **not** prove that the workflow-dispatch input was `none` and must not be used to contradict the founder's reported selection.

## Readiness remediation — PR #38

**REMEDIATION — MERGED; FRESH STAGING PROOF PENDING.**

- PR #38 — `Gate Stripe staging acceptance on web-commerce readiness`;
- tested head: `c53d4d74931d96bdcf775374246c84eff2eb67df`;
- merge commit / current platform main at write-back: `eed488e409960c96be287c811cbc33b40de7f971`.

PR #38 adds a staging-only, non-mutating web-side readiness route:

`GET /api/commerce/health`

The route traverses the actual deployed path:

**web Worker -> native `COMMERCE_BOUNDARY` Service Binding -> authenticated commerce Worker -> Durable Object coordinator**

and returns ready only after that path can execute the commerce contract with the propagated runtime token. Outside `APP_ENV=staging` it returns `404`.

The signed-expiry acceptance now waits up to 60 seconds for this web-to-commerce readiness proof before creating any Twenty edition, purchaser identity, Order, reservation or Stripe Checkout Session.

PR #38 validation:

- bootstrap run `36236239647`: SUCCESS;
- full web run `36236239621`: SUCCESS;
- repository contract: PASS;
- Astro type-check: PASS;
- Cloudflare production build/config: PASS;
- direct Full commerce contract: PASS;
- browser/accessibility development suite: PASS;
- production/minified runtime-error suite: PASS;
- Lighthouse baseline: PASS.

## Current acceptance gate

Run a fresh `cloudflare-staging` workflow from `formalife/platform/main` at `eed488e409960c96be287c811cbc33b40de7f971` or later with:

`paid_acceptance = single`

The run must first prove web-to-commerce readiness, then reach the paid acceptance step and emit a live hosted Stripe Sandbox Checkout URL.

After the founder completes that Sandbox Checkout, the run must prove:

1. Stripe Session `complete` / `payment_status=paid`;
2. canonical SINGLE amount EUR 80 / 8000 minor units;
3. genuine signed `checkout.session.completed` delivery to the public Formalife webhook;
4. correct linked Order/reservation;
5. reservation consumed rather than released;
6. Order `PAID`;
7. exactly one PaymentRecord;
8. exactly one Enrollment for SINGLE;
9. capacity 12 -> 11 consumed correctly;
10. public server-side state `PAID`, `verified: true`.

Only then may SINGLE successful payment be promoted to RESULT/PASS.

After SINGLE passes, repeat the same gate for `paid_acceptance = couple` and require EUR 120 / two Enrollments / two consumed seats.

## P6 status

P6 remains **OPEN**.

Still open after this write-back:

- real successful-payment SINGLE acceptance;
- real successful-payment COUPLE acceptance;
- duplicate/reordered real delivery acceptance;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery.
