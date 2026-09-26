# P6 Stripe Paid Acceptance

Status: CURRENT TEST — REAL SINGLE STRIPE PAYMENT CONFIRMED; FINAL OPERATIONAL RECOVERY PROOF PENDING
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

**REMEDIATION — MERGED AND PROVEN SUFFICIENT TO REACH PAID CHECKOUT.**

- PR #38 — `Gate Stripe staging acceptance on web-commerce readiness`;
- tested head: `c53d4d74931d96bdcf775374246c84eff2eb67df`;
- merge commit: `eed488e409960c96be287c811cbc33b40de7f971`.

PR #38 adds a staging-only, non-mutating web-side readiness route:

`GET /api/commerce/health`

The route traverses the actual deployed path:

**web Worker -> native `COMMERCE_BOUNDARY` Service Binding -> authenticated commerce Worker -> Durable Object coordinator**

and returns ready only after that path can execute the commerce contract with the propagated runtime token. Outside `APP_ENV=staging` it returns `404`.

The signed-expiry acceptance waits for this web-to-commerce readiness proof before creating commerce state.

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

## Run #14 — real SINGLE payment

**RESULT — STRIPE PAYMENT CONFIRMED; FINAL OPERATIONAL ACCEPTANCE NOT YET PROMOTED TO PASS.**

Fresh paid SINGLE staging run:

- workflow: `cloudflare-staging`;
- run: `36237175160`;
- run number: `14`;
- attempt: `1`;
- deployed commit: `eed488e409960c96be287c811cbc33b40de7f971`;
- job: `108390960373`;
- conclusion: `FAILURE` after payment during verification;
- evidence artifact: `10904760565`;
- artifact digest: `sha256:7cbc48761a2ee536d580e1d532ebf527068493d41c6373867614a81f1b0ec50f`.

Pre-payment evidence:

- option: `SINGLE`;
- synthetic confirmed edition: `253cd608-a72d-463b-ad99-cb522ea2347c`;
- Twenty Order code: `WEB-20260926105500-969AD706`;
- Twenty Order id: `757235e4-f225-44f8-a058-e77b969ad706`;
- protected reservation active before payment;
- active reserved seats: `1`;
- consumed reserved seats: `0`;
- available seats: `11`.

Real Stripe evidence:

- Checkout Session: `cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`;
- Session status: `complete`;
- payment status: `paid`;
- amount: `8000` minor units / EUR 80;
- currency: `eur`;
- real PaymentIntent: `pi_3UJtGy620hE08wgd0M1xzPp0`.

This proves that the real hosted Stripe Sandbox SINGLE payment completed. It does **not** by itself close the successful-payment acceptance gate, because the harness failed while verifying downstream operational effects.

The failure was Twenty API rate limiting during the post-payment polling loop:

`Rate limit exceeded for apiKey: 100 requests per 60s.`

The original verifier queried Order, PaymentRecord/Enrollment and capacity every 1.5 seconds, which could itself exceed the Twenty API allowance. Therefore this failure is classified as a **TEST HARNESS RATE-LIMIT FAILURE AFTER CONFIRMED PAYMENT**, not as evidence that Stripe payment or webhook processing failed.

### Browser redirect defect exposed by the same payment

After payment Stripe redirected the browser to:

`/corso-sicurezza-pediatrica/`

That route did not exist in the deployed staging application and returned `404`.

This redirect defect is separate from payment truth. The authoritative payment state remains the signed webhook plus server-side Formalife state. The redirect path nevertheless constituted a real product defect and required remediation before subsequent paid acceptance.

## Post-payment / recovery remediation — PR #39

**REMEDIATION — MERGED; RECOVERY PROOF PENDING.**

- PR #39 — `Fix post-checkout redirect and recover paid Stripe acceptance`;
- tested head: `eef87ab1dcb053612f8ed497280c689de0a8850c`;
- merge commit / current platform main at write-back: `873d6c787584efa98400cb8aa0e486a63729ca9b`.

PR #39 changes only the acceptance/return mechanics, not the commercial truth model:

1. Stripe `success_url` now returns to `/checkout/?session_id={CHECKOUT_SESSION_ID}`;
2. Stripe `cancel_url` now returns to `/checkout/?checkout=cancelled`;
3. `/checkout/` is a real page that polls the existing server-side checkout-status endpoint and only renders payment confirmation when Formalife reports `PAID`, `verified: true`;
4. browser redirect/query parameters remain non-authoritative;
5. the paid verifier post-webhook polling interval is reduced from 1.5s to 5s, keeping its normal request rate below the observed Twenty limit;
6. paid runs no longer re-run the already-closed signed-expiry acceptance before the money-path test;
7. `cloudflare-staging` now supports `paid_recovery_session_id` for an already-paid Checkout Session;
8. recovery verifies Stripe payment, linked Twenty Order, PaymentRecord, Enrollment count, consumed reservation/capacity and public verified state without creating or charging a second Checkout Session.

PR #39 CI on tested head:

- bootstrap run `36238086349`: SUCCESS;
- P6 commerce-contract run `36238086234`: SUCCESS;
- Stripe sandbox run `36238086235`: SUCCESS;
- web run `36238086193`: SUCCESS;
- Astro type-check: PASS;
- Cloudflare production build/config: PASS;
- direct Full commerce contract: PASS;
- browser/accessibility development suite: PASS;
- production/minified runtime-error suite: PASS;
- Lighthouse baseline: PASS.

## Current acceptance gate

Do **not** create another SINGLE payment yet.

Run a fresh `cloudflare-staging` workflow from `formalife/platform/main` at `873d6c787584efa98400cb8aa0e486a63729ca9b` or later with:

- `paid_acceptance = none`
- `paid_recovery_session_id = cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`

The recovery must prove from the already-paid session:

1. Stripe Session remains `complete` / `payment_status=paid`;
2. amount is EUR 80 / 8000 minor units;
3. Stripe metadata resolves to the correct Twenty Order;
4. the linked Order is `PAID`;
5. exactly one PaymentRecord exists;
6. exactly one Enrollment exists for SINGLE;
7. the protected reservation is consumed, not released;
8. capacity is `11` available after the consumed SINGLE seat;
9. public server-side checkout state is `PAID`, `verified: true`.

Only after this recovery passes may the SINGLE successful-payment gate be promoted to **RESULT / PASS**.

After SINGLE passes, run the same corrected acceptance for `paid_acceptance = couple` and require EUR 120, two Enrollments and two consumed seats.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- real signed expiry delivery and cancellation/release path;
- real hosted Stripe Sandbox SINGLE payment completion itself;
- redirect defect identified and remediated in PR #39;
- paid validator rate-limit defect identified and remediated in PR #39.

Still open at minimum:

- final operational recovery proof for the already-paid SINGLE session;
- real successful-payment COUPLE acceptance;
- duplicate/reordered real delivery acceptance;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery.
