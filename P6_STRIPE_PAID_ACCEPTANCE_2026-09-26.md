# P6 Stripe Paid Acceptance

Status: CURRENT RESULT — REAL SINGLE SUCCESSFUL-PAYMENT ACCEPTANCE PASS; COUPLE NEXT
Date: 2026-09-26

## Context

The signed-expiry gate is already closed / PASS in `P6_STRIPE_WEBHOOK_STAGING_DEPLOYMENT_2026-09-25.md` from Cloudflare staging run `36231789069` (#11), proving a genuine signed `checkout.session.expired` path with Order cancellation, reservation release, restored capacity, zero PaymentRecord, zero Enrollment and verified public `CANCELLED` state.

This record covers the next money-path gate: a genuine Stripe Sandbox `checkout.session.completed` linked to a real Formalife Order and protected capacity reservation, followed by correct downstream operational effects.

## Acceptance contract

The successful-payment gate requires all of the following together:

1. a real hosted Stripe Sandbox Checkout Session;
2. canonical amount/currency for the selected offer;
3. signed `checkout.session.completed` processing, with browser redirect non-authoritative;
4. linked Order resolved and `PAID`;
5. protected reservation consumed, not released;
6. exactly one PaymentRecord;
7. exactly one Enrollment for SINGLE, two for COUPLE;
8. correct capacity consumption;
9. public server-side checkout status `PAID`, `verified: true`;
10. idempotent/replay-safe commerce command state.

## Harness — PR #37

**TEST — IMPLEMENTED AND MERGED.**

- PR #37 — `Add real paid Stripe staging acceptance`;
- merge commit: `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`;
- script: `scripts/p6-stripe-paid-webhook-acceptance.mjs`;
- `cloudflare-staging` input: `paid_acceptance = none | single | couple`;
- bootstrap run `36232644170`: PASS;
- P6 commerce-contract run `36232644167`: PASS.

The harness creates a fresh synthetic confirmed Twenty edition, starts checkout through the deployed public Formalife endpoint, requires a real Stripe Sandbox Checkout Session, exposes the hosted Checkout URL for the one interactive sandbox payment, then verifies Stripe and downstream Formalife state.

## Run #12 — deployment readiness failure

**RESULT — FAILED BEFORE PAID ACCEPTANCE; NO SUCCESSFUL-PAYMENT CLAIM.**

- run: `36233877518` (#12);
- commit: `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`;
- job: `108381999964`;
- conclusion: `FAILURE`.

The existing signed-expiry step failed before the paid step with:

`Public checkout failed (401): {"error":"UNAUTHORIZED","message":"Commerce boundary HTTP 401"}`

The direct commerce probe accepted the newly generated boundary token immediately before the public checkout. Diagnosis: a deployment readiness / secret-propagation race between web and commerce Workers, not a Stripe-payment failure.

The founder explicitly launched this attempt intending `paid_acceptance=single`. A later artifact field showing `paidAcceptanceRequested: none` was step-scope fallout because the paid step never ran; it is not evidence that the dispatch input was `none`.

## Readiness remediation — PR #38

**REMEDIATION — MERGED AND PROVEN SUFFICIENT.**

- PR #38 — `Gate Stripe staging acceptance on web-commerce readiness`;
- tested head: `c53d4d74931d96bdcf775374246c84eff2eb67df`;
- merge commit: `eed488e409960c96be287c811cbc33b40de7f971`.

PR #38 added staging-only `GET /api/commerce/health`, traversing the real deployed path:

**web Worker -> native `COMMERCE_BOUNDARY` Service Binding -> authenticated commerce Worker -> Durable Object coordinator**.

CI: repository contract, Astro type-check, Cloudflare build/config, commerce tests, browser/accessibility, production runtime and Lighthouse all PASS.

## Run #14 — real SINGLE payment completed

**RESULT — STRIPE PAYMENT CONFIRMED; DOWNSTREAM VERIFICATION FAILED.**

- run: `36237175160` (#14);
- deployed commit: `eed488e409960c96be287c811cbc33b40de7f971`;
- job: `108390960373`;
- conclusion: `FAILURE` after payment;
- evidence artifact: `10904760565`;
- digest: `sha256:7cbc48761a2ee536d580e1d532ebf527068493d41c6373867614a81f1b0ec50f`.

Pre-payment evidence:

- option: `SINGLE`;
- edition: `253cd608-a72d-463b-ad99-cb522ea2347c`;
- Order code: `WEB-20260926105500-969AD706`;
- Order id: `757235e4-f225-44f8-a058-e77b969ad706`;
- protected reservation active;
- available seats: `11`.

Stripe evidence:

- Checkout Session: `cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`;
- Session: `complete`;
- payment status: `paid`;
- amount: `8000` minor units / EUR 80;
- PaymentIntent: `pi_3UJtGy620hE08wgd0M1xzPp0`.

This proved the real hosted Stripe Sandbox SINGLE payment itself. The verifier then hit Twenty's `100 requests / 60s` API limit, so run #14 was not promoted to final operational PASS.

The same payment exposed a separate product defect: Stripe redirected to nonexistent `/corso-sicurezza-pediatrica/`, producing 404. Redirect state remained non-authoritative.

## Post-payment / recovery remediation — PR #39

**REMEDIATION — MERGED.**

- PR #39 — `Fix post-checkout redirect and recover paid Stripe acceptance`;
- tested head: `eef87ab1dcb053612f8ed497280c689de0a8850c`;
- merge commit: `873d6c787584efa98400cb8aa0e486a63729ca9b`.

Changes:

- Stripe success -> `/checkout/?session_id={CHECKOUT_SESSION_ID}`;
- Stripe cancel -> `/checkout/?checkout=cancelled`;
- `/checkout/` verifies state server-side and only confirms payment when Formalife reports `PAID`, `verified: true`;
- paid verifier polling reduced;
- paid runs no longer repeat the already-closed expiry gate;
- `paid_recovery_session_id` can verify an already-paid Session without creating a second payment.

PR #39 CI: bootstrap, P6 commerce-contract, Stripe sandbox, full web, browser/runtime and Lighthouse PASS.

## Run #15 — partial downstream payment application

**RESULT — FAILED; REAL PARTIAL APPLICATION CONFIRMED.**

- run: `36244785337` (#15);
- commit: `873d6c787584efa98400cb8aa0e486a63729ca9b`;
- job: `108411869916`;
- conclusion: `FAILURE`;
- artifact: `10907296023`;
- digest: `sha256:1e8d28bf23ea04620824bf674ce10b39d55625ca6823883aeef70f917041c2a9`.

Recovery of the same paid Session proved before failure:

- Stripe `complete` / `paid` / EUR 80;
- real PaymentIntent;
- linked Order exists and is `PAID`;
- Stripe metadata matches the Order.

It then found:

- PaymentRecord: `0`;
- Enrollment: `0`.

The commerce outbox order is `ORDER_UPDATE -> PAYMENT_CREATE -> ENROLLMENT_CREATE`. Since Order was already `PAID`, the evidence established a real partial downstream application rather than a Stripe failure.

Canonical command key:

`payment:pi_3UJtGy620hE08wgd0M1xzPp0`

## Outbox diagnostics — PR #40

**REMEDIATION — MERGED.**

- PR #40 — `Expose paid recovery outbox diagnostics`;
- tested head: `a76019b864100798a5aa7e3d0a7385507aa8cff4`;
- merge commit: `d50e430bebe197e803dd1648584c90df4bcd9d3c`;
- bootstrap run `36245383187`: PASS;
- P6 commerce-contract run `36245383182`: PASS.

The recovery was extended to persist command/outbox status, attempts and `last_error`, plus Order, effect counts, capacity and public status. It did not synthesize financial records.

## Run #16 — root cause proven

**RESULT — FAILED; FIRST UPSTREAM DOWNSTREAM-EFFECT DEFECT PROVEN.**

- run: `36245617200` (#16);
- commit: `d50e430bebe197e803dd1648584c90df4bcd9d3c`;
- job: `108414146930`;
- conclusion: `FAILURE`;
- artifact: `10906419824`;
- digest: `sha256:51f49e0d213065f744727ca8685b767e754c576bb07a664b65f29bb9eb18a439`.

Run #16 reconfirmed:

- Stripe Session `complete` / `paid` / EUR 80;
- PaymentIntent `pi_3UJtGy620hE08wgd0M1xzPp0`;
- Order `757235e4-f225-44f8-a058-e77b969ad706` = `PAID`;
- consumed reservation = `1`;
- available seats = `11`;
- public status = HTTP 200 / `PAID` / `verified: true`;
- PaymentRecord = `0`;
- Enrollment = `0`.

Persisted command:

- key: `payment:pi_3UJtGy620hE08wgd0M1xzPp0`;
- type: `payment.apply`;
- status: `COMMITTED`;
- payment id: `c933cbec-a1f1-88e8-9249-5c2b66aa313d`;
- enrollment id: `f31dba2d-b10b-86d5-849b-befe2d1ade9d`.

Outbox:

1. `ORDER_UPDATE`: `DELIVERED`, 2 attempts;
2. `PAYMENT_CREATE`: `PENDING`, 2580 attempts, `Value "c933cbec-a1f1-88e8-9249-5c2b66aa313d" is not a valid UUID`;
3. `ENROLLMENT_CREATE`: `PENDING`, 2580 attempts, latest error rate-limited by Twenty.

**FACT / ROOT CAUSE:** the deterministic internal UUIDv8-style PaymentRecord id was rejected by the deployed Twenty API as an invalid UUID. The Enrollment rate-limit error was downstream retry churn, not promoted to a separate root cause.

## Twenty UUID reconciliation — PR #41

**REMEDIATION — MERGED AND DEPLOYED PROOF PASS.**

- PR #41 — `Recover P6 paid outbox with Twenty-compatible IDs`;
- tested head: `c90014f6a8dec335ef6cbd3a4f16fd68522c9779`;
- merge commit: `7a324e7ba6ea03c6719ab53477b62e16f6b0f72e`;
- bootstrap run `36246027993`: PASS;
- P6 commerce-contract run `36246027970`: PASS;
- web run `36246028073`: PASS.

The remediation does not mutate the already-committed `payment.apply` payload, provider identity, idempotency key or internal effect identities. Legacy internal UUIDv8 IDs are deterministically normalized to UUIDv5 only at the outbound Twenty create boundary:

- payment `c933cbec-a1f1-88e8-9249-5c2b66aa313d` -> `280a9983-3ce9-5a18-b7fe-4189b6f9a625`;
- enrollment `f31dba2d-b10b-86d5-849b-befe2d1ade9d` -> `be8f181d-c883-57b0-8a66-8861ecd37a10`.

This preserves replay/idempotency semantics while making the Twenty records creatable.

## Run #17 — recovered SINGLE acceptance

**RESULT — PASS. SINGLE SUCCESSFUL-PAYMENT GATE CLOSED.**

- workflow: `cloudflare-staging`;
- run: `36246709032`;
- run number: `17`;
- attempt: `1`;
- deployed commit: `7a324e7ba6ea03c6719ab53477b62e16f6b0f72e`;
- job: `108417133830`;
- conclusion: `SUCCESS`;
- evidence artifact: `10907318816`;
- artifact digest: `sha256:0d23d8f90a2d6339759e4245f088618298668fc8de2b19f354860b13c80affcd`;
- recovered Checkout Session: `cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`.

Deployed evidence proves together:

- Stripe Session: `complete`;
- Stripe payment status: `paid`;
- amount: EUR 80 / `8000`;
- PaymentIntent: `pi_3UJtGy620hE08wgd0M1xzPp0`;
- canonical commerce command: `payment:pi_3UJtGy620hE08wgd0M1xzPp0`;
- command status: `MIRRORED`;
- `ORDER_UPDATE`: `DELIVERED`;
- `PAYMENT_CREATE`: `DELIVERED`, `last_error = null`;
- `ENROLLMENT_CREATE`: `DELIVERED`, `last_error = null`;
- linked Order: `PAID`;
- exactly `1` PaymentRecord;
- exactly `1` Enrollment;
- active reserved seats: `0`;
- consumed reserved seats: `1`;
- available seats: `11` of `12`;
- public checkout endpoint: HTTP `200`, `PAID`, `verified: true`.

This is the same original real payment. No second SINGLE payment was created to close the gate.

The high retry counts on the two previously blocked effects (`2875` attempts each) are historical incident evidence. They no longer block the command: both effects are delivered and `last_error` is cleared. Retry/backoff hardening remains a separate resilience concern and does not invalidate the accepted SINGLE result.

## Current acceptance gate

SINGLE is **CLOSED / PASS**.

The next successful-payment acceptance is **COUPLE**. Run the corrected real paid acceptance with `paid_acceptance = couple` and require:

1. Stripe Sandbox EUR 120;
2. linked COUPLE Order `PAID`;
3. exactly one PaymentRecord;
4. exactly two Enrollments;
5. two consumed reserved seats;
6. correct post-payment capacity;
7. public server-side `PAID`, `verified: true`;
8. idempotent/replay-safe command state.

After COUPLE, P6 still requires duplicate/reordered delivery acceptance, refund/reconciliation, real Brevo transactional delivery, real privacy-safe PostHog delivery and final deployed Cloudflare verification for those paths.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- real signed expiry delivery and cancellation/release path;
- real hosted Stripe Sandbox SINGLE payment;
- signed successful-payment processing linked to the real Order/reservation;
- SINGLE Order `PAID`;
- exactly one PaymentRecord;
- exactly one Enrollment;
- reservation consumed and capacity `12 -> 11`;
- public SINGLE server-side state `PAID`, `verified: true`;
- redirect remediation;
- staging readiness remediation;
- Twenty UUID compatibility remediation;
- recovered outbox is fully `MIRRORED`.

Still open at minimum:

- real successful-payment COUPLE acceptance;
- duplicate/reordered real delivery acceptance;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- retry/backoff hardening for persistent downstream failures.