# P6 Stripe Paid Acceptance

Status: CURRENT INCIDENT — REAL SINGLE STRIPE PAYMENT AND ORDER PAID CONFIRMED; PAYMENT/ENROLLMENT OUTBOX FAILURE UNDER DIAGNOSIS
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

The original verifier queried Order, PaymentRecord/Enrollment and capacity every 1.5 seconds, which could itself exceed the Twenty API allowance. Therefore run #14 alone was classified as a **TEST HARNESS RATE-LIMIT FAILURE AFTER CONFIRMED PAYMENT** rather than evidence of the final downstream state.

### Browser redirect defect exposed by the same payment

After payment Stripe redirected the browser to:

`/corso-sicurezza-pediatrica/`

That route did not exist in the deployed staging application and returned `404`.

This redirect defect is separate from payment truth. The authoritative payment state remains the signed webhook plus server-side Formalife state. The redirect path nevertheless constituted a real product defect and required remediation before subsequent paid acceptance.

## Post-payment / recovery remediation — PR #39

**REMEDIATION — MERGED.**

- PR #39 — `Fix post-checkout redirect and recover paid Stripe acceptance`;
- tested head: `eef87ab1dcb053612f8ed497280c689de0a8850c`;
- merge commit: `873d6c787584efa98400cb8aa0e486a63729ca9b`.

PR #39 changes only the acceptance/return mechanics, not the commercial truth model:

1. Stripe `success_url` now returns to `/checkout/?session_id={CHECKOUT_SESSION_ID}`;
2. Stripe `cancel_url` now returns to `/checkout/?checkout=cancelled`;
3. `/checkout/` is a real page that polls the existing server-side checkout-status endpoint and only renders payment confirmation when Formalife reports `PAID`, `verified: true`;
4. browser redirect/query parameters remain non-authoritative;
5. the paid verifier post-webhook polling interval is reduced from 1.5s to 5s;
6. paid runs no longer re-run the already-closed signed-expiry acceptance before the money-path test;
7. `cloudflare-staging` supports `paid_recovery_session_id` for an already-paid Checkout Session;
8. recovery verifies the already-paid session without creating or charging a second Checkout Session.

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

## Run #15 — recovery of the already-paid SINGLE session

**RESULT — FAILED; REAL PARTIAL PAYMENT APPLICATION CONFIRMED.**

- workflow: `cloudflare-staging`;
- run: `36244785337`;
- run number: `15`;
- attempt: `1`;
- deployed commit: `873d6c787584efa98400cb8aa0e486a63729ca9b`;
- job: `108411869916`;
- conclusion: `FAILURE`;
- evidence artifact: `10907296023`;
- artifact digest: `sha256:1e8d28bf23ea04620824bf674ce10b39d55625ca6823883aeef70f917041c2a9`;
- recovery Session: `cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`.

Deployment/build/readiness/capacity probes all passed. The expiry and new paid-checkout steps were correctly skipped; the recovery step alone evaluated the existing paid Session.

The recovery failed with:

`Paid checkout does not have exactly one PaymentRecord`

and evidence:

- `paymentRecords: 0`;
- `enrollments: 0`.

The recovery assertion order is material evidence. Before reaching the failing PaymentRecord assertion it had already required and passed:

1. Stripe Session `complete`;
2. Stripe `payment_status=paid`;
3. EUR canonical amount;
4. real PaymentIntent;
5. linked Formalife Order exists;
6. Stripe amount matches Order seat quantity;
7. **linked Order state is `PAID`**;
8. Stripe metadata `orderId` matches that Order.

Therefore the current classification is not a Stripe failure and not merely a validator-rate issue. It is a **REAL PARTIAL DOWNSTREAM PAYMENT APPLICATION**: the signed payment path advanced the Order to `PAID`, while the expected PaymentRecord and Enrollment mirror effects were not materialized in Twenty.

The commerce coordinator architecture explains the partial state. `payment.apply` commits an outbox in this order:

1. `ORDER_UPDATE`;
2. `PAYMENT_CREATE`;
3. `ENROLLMENT_CREATE` for each seat.

Since the Order is `PAID` but both downstream record counts are zero, `ORDER_UPDATE` was delivered while one or more later outbox effects remain pending/failing. The canonical persisted command key is:

`payment:pi_3UJtGy620hE08wgd0M1xzPp0`

The exact downstream `last_error` was not included in run #15 evidence, so the specific schema/transport cause must **not** be guessed or silently repaired.

## Outbox diagnostic remediation — PR #40

**REMEDIATION — MERGED; DIAGNOSTIC RECOVERY REQUIRED.**

- PR #40 — `Expose paid recovery outbox diagnostics`;
- tested head: `a76019b864100798a5aa7e3d0a7385507aa8cff4`;
- merge commit / current platform main at write-back: `d50e430bebe197e803dd1648584c90df4bcd9d3c`;
- bootstrap run `36245383187`: SUCCESS;
- P6 commerce-contract run `36245383182`: SUCCESS.

PR #40 does **not** synthesize or repair financial records. It adds observability only:

- derives `payment:<PaymentIntent>` from the existing real Stripe Session;
- inspects the authenticated persisted commerce command/outbox before repeated Twenty reads;
- persists each outbox effect `status`, `attempts` and `last_error` in the recovery artifact;
- persists Order, PaymentRecord/Enrollment counts and capacity snapshots before final assertions;
- captures a best-effort public checkout-status probe even when the recovery fails.

The PaymentRecord and Enrollment payloads were compared with the current Twenty custom-object declarations and no deterministic contract mismatch was established from static inspection alone. The next decision therefore depends on the actual persisted outbox error.

## Current acceptance gate

Do **not** create another SINGLE payment.

Run `cloudflare-staging` from `formalife/platform/main` at `d50e430bebe197e803dd1648584c90df4bcd9d3c` or later with the same recovery inputs:

- `paid_acceptance = none`
- `paid_recovery_session_id = cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`

The immediate purpose is diagnostic: capture the persisted outbox state and exact `last_error` for `PAYMENT_CREATE` / `ENROLLMENT_CREATE` under command `payment:pi_3UJtGy620hE08wgd0M1xzPp0`.

After that evidence exists:

1. fix only the first upstream downstream-effect defect;
2. reconcile/replay the same canonical already-paid transaction without a second payment;
3. require exactly one PaymentRecord and one Enrollment;
4. require consumed reservation and correct capacity;
5. require public server-side `PAID`, `verified: true`;
6. only then promote SINGLE to **RESULT / PASS**.

After SINGLE passes, run the corrected acceptance for `paid_acceptance = couple` and require EUR 120, two Enrollments and two consumed seats.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- real signed expiry delivery and cancellation/release path;
- real hosted Stripe Sandbox SINGLE payment completion itself;
- linked SINGLE Order advanced to `PAID`;
- redirect defect identified and remediated in PR #39;
- paid validator rate-limit defect identified and remediated in PR #39;
- recovery now exposes persisted outbox diagnostics via PR #40.

Still open at minimum:

- root cause and reconciliation of missing SINGLE PaymentRecord / Enrollment;
- final operational SINGLE successful-payment acceptance;
- real successful-payment COUPLE acceptance;
- duplicate/reordered real delivery acceptance;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery.
