# P6 Stripe Paid Acceptance

Status: CURRENT RESULT — REAL SINGLE + COUPLE SUCCESSFUL-PAYMENT ACCEPTANCE PASS; DELIVERY RESILIENCE ALSO CLOSED
Date: 2026-09-26

## Scope

This record is the canonical successful-payment history for P6 Direct Full. The separate current delivery-resilience result is in `P6_STRIPE_DELIVERY_RESILIENCE_2026-09-26.md`.

The signed-expiry path is independently closed / PASS in `P6_STRIPE_WEBHOOK_STAGING_DEPLOYMENT_2026-09-25.md`.

## Acceptance contract

The successful-payment gate requires together:

1. a real hosted Stripe Sandbox Checkout Session;
2. canonical amount/currency;
3. signed `checkout.session.completed` processing, with browser redirect non-authoritative;
4. linked Order `PAID`;
5. protected reservation consumed;
6. exactly one PaymentRecord;
7. exactly one Enrollment for SINGLE or two for COUPLE;
8. correct capacity consumption;
9. public server-side state `PAID`, `verified: true`;
10. replay-safe commerce command state.

## Harness — PR #37

**TEST — MERGED.**

- PR #37 — `Add real paid Stripe staging acceptance`;
- merge: `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`;
- `cloudflare-staging` input: `paid_acceptance = none | single | couple`.

The harness creates a fresh synthetic confirmed Twenty edition, starts checkout through the public deployed Formalife endpoint, requires a real hosted Stripe Sandbox Checkout Session, then verifies Stripe and all downstream operational state.

## Historical remediation chain

### Run #12 — readiness failure

**RESULT — FAILURE BEFORE PAYMENT ACCEPTANCE.**

- run `36233877518`;
- commit `5ca6e6dde73c289ed271b3d501b5f20d4ca9fca0`.

The existing signed-expiry step failed with a web -> commerce `401` during immediate secret propagation. No paid-path result was claimed.

PR #38 (`eed488e409960c96be287c811cbc33b40de7f971`) added a staging-only readiness path through web Worker -> Service Binding -> authenticated commerce Worker -> Durable Object.

### Run #14 — real SINGLE payment, post-payment verifier failure

**RESULT — REAL STRIPE PAYMENT CONFIRMED; OPERATIONAL PASS NOT YET PROVEN.**

- run `36237175160`;
- deployed commit `eed488e409960c96be287c811cbc33b40de7f971`;
- Checkout Session `cs_test_a19sAzGT729OJnl1Sfgrq8lpqVsJxCGbJlb4OodKutLkc3Mx0W2i7nRMll`;
- amount EUR 80 / `8000`;
- PaymentIntent `pi_3UJtGy620hE08wgd0M1xzPp0`.

The payment was real and complete, but the verifier exceeded Twenty's request limit. The same run exposed the obsolete redirect to `/corso-sicurezza-pediatrica/`.

PR #39 (`873d6c787584efa98400cb8aa0e486a63729ca9b`) fixed the post-checkout route, reduced polling, stopped rerunning the already-closed expiry gate during paid runs, and added recovery of an existing paid Session without a second payment.

### Runs #15-#16 — partial application and root cause

**RESULT — FAILURE; REAL PARTIAL DOWNSTREAM APPLICATION PROVEN.**

Run #15 (`36244785337`) recovered the paid SINGLE Order as `PAID` but found zero PaymentRecords and zero Enrollments.

PR #40 (`d50e430bebe197e803dd1648584c90df4bcd9d3c`) added persisted outbox diagnostics.

Run #16 (`36245617200`) proved the first upstream defect:

- canonical command `payment:pi_3UJtGy620hE08wgd0M1xzPp0` = `COMMITTED`;
- `ORDER_UPDATE` = `DELIVERED`;
- `PAYMENT_CREATE` remained pending because Twenty rejected internal id `c933cbec-a1f1-88e8-9249-5c2b66aa313d` as an invalid UUID;
- Enrollment retry churn was downstream of that incident.

PR #41 (`7a324e7ba6ea03c6719ab53477b62e16f6b0f72e`) preserved internal command/idempotency identities and normalized legacy internal UUIDv8-style ids to deterministic UUIDv5 only at the outbound Twenty create boundary.

## Run #17 — recovered SINGLE acceptance

**RESULT — PASS. SINGLE GATE CLOSED.**

- run `36246709032`;
- deployed commit `7a324e7ba6ea03c6719ab53477b62e16f6b0f72e`;
- evidence artifact `10907318816`;
- digest `sha256:0d23d8f90a2d6339759e4245f088618298668fc8de2b19f354860b13c80affcd`.

The original real payment, without a second SINGLE payment, proved:

- Stripe `complete` / `paid` / EUR 80;
- command `payment:pi_3UJtGy620hE08wgd0M1xzPp0` = `MIRRORED`;
- Order `PAID`;
- exactly `1` PaymentRecord;
- exactly `1` Enrollment;
- reservation consumed;
- capacity `12 -> 11`;
- public state `PAID`, `verified: true`.

## Run #18 — real COUPLE acceptance

**RESULT — PASS. COUPLE GATE CLOSED.**

- run `36248322670`;
- deployed commit `7a324e7ba6ea03c6719ab53477b62e16f6b0f72e`;
- job `108421548950`;
- evidence artifact `10907897999`;
- digest `sha256:32e6f012f4514a5662c99a1d242278992a74fee8e42241127a1d2e01f263653e`;
- Checkout Session `cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`;
- PaymentIntent `pi_3UJwa6620hE08wgd1wqZOsXI`.

Deployed evidence proves:

- Stripe `complete` / `paid` / EUR 120;
- linked Order `e616c4a0-5608-4d73-ac81-5edc2c072df3` = `PAID`;
- exactly `1` PaymentRecord;
- exactly `2` Enrollments;
- active reservations `0`;
- consumed reserved seats `2`;
- capacity `12 -> 10`;
- public state `PAID`, `verified: true`.

## Delivery resilience linkage

Duplicate / reordered delivery is now independently **CLOSED / PASS** by Cloudflare staging run #19 (`36249376747`) on platform `114ea45147037af3a04229094b15fd3e72694eec`.

Canonical evidence and provenance boundaries are recorded in `P6_STRIPE_DELIVERY_RESILIENCE_2026-09-26.md`. This supersedes the previous statement in this file that duplicate/reordered delivery was still open.

## Current gate

Successful payment for SINGLE and COUPLE is **CLOSED / PASS**.

Duplicate / reordered delivery is **CLOSED / PASS**.

The next P6 gate is **refund / reconciliation**.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- genuine signed expiry cancellation/release;
- real SINGLE successful payment;
- real COUPLE successful payment;
- verified Twenty PaymentRecord/Enrollment mirror;
- protected capacity consumption;
- public verified paid state;
- duplicate completed-event replay safety;
- stale out-of-order expiry after payment safely ignored.

Still open at minimum:

- refund / reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- retry/backoff hardening for persistent downstream failures.