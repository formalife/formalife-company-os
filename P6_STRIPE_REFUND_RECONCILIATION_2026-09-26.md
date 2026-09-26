# P6 Stripe Refund Reconciliation

Status: CURRENT TEST — REFUND / RECONCILIATION IMPLEMENTED AND MERGED; REAL DEPLOYED REFUND PROOF PENDING
Date: 2026-09-26

## Context

P6 has already closed the following deployed gates:

- genuine signed Stripe expiry cancellation/release;
- real SINGLE successful payment;
- real COUPLE successful payment;
- duplicate completed delivery replay safety;
- stale expiry ignored after payment;
- verified Twenty payment/enrollment mirror;
- deployed Cloudflare web -> commerce Service Binding path.

The next money-path gate is refund / reconciliation.

The earlier P5 Twenty acceptance established that the data model can represent a cancellation/refund without destroying transaction history: preserve the original PAYMENT, create a separate REFUND PaymentRecord, cancel the relevant Enrollment(s), move the Order to `REFUNDED`, and release capacity. That evidence was model-level and did not prove a real Stripe refund path.

## Refund operating contract

**DECISION / IMPLEMENTED CONTRACT — P6 V1**

For Direct Full Stripe refunds:

1. the original succeeded PAYMENT record is never overwritten or deleted;
2. every succeeded Stripe Refund is mirrored as a separate PaymentRecord with `kind = REFUND`;
3. refund identity is keyed by the Stripe Refund object (`re_...`) and is idempotent;
4. refund reconciliation resolves the original Formalife payment from Stripe `payment_intent`, not from optional refund metadata;
5. partial refunds are financial-only: they do not cancel Enrollments, do not change the paid Order to `REFUNDED`, and do not release occupied seats;
6. successful refund amounts are accumulated serially in the commerce Durable Object;
7. only when cumulative successful refunds equal the original payment amount is the commercial purchase considered fully refunded;
8. on cumulative full refund:
   - existing Enrollments are updated to `CANCELLED` with `cancellationReasonCode = STRIPE_REFUND`;
   - Order moves to `REFUNDED`;
   - the previously consumed protected reservation moves to `REFUNDED`;
   - consumed seat capacity is restored;
9. cumulative refund amount greater than the original payment fails closed;
10. if an Enrollment has progressed beyond the safe `CONFIRMED` / already-`CANCELLED` refund boundary (for example `ATTENDED`), automatic refund operational reconciliation fails closed for manual reconciliation rather than silently rewriting history;
11. late checkout completed/expired deliveries cannot reverse a `REFUNDED` Order.

This contract separates money history from operational seat release and prevents a partial monetary adjustment from accidentally reopening the whole booking.

## PR #43 — implementation

**TEST — IMPLEMENTED, CI PASS, MERGED. NOT YET DEPLOYED REFUND RESULT.**

Implementation repository: `formalife/platform`.

- PR #43 — `Implement P6 Stripe refund reconciliation`;
- tested head: `df1e4905f0cc08859dcb292e8fe300d2169899b9`;
- merge commit / platform main: `33988b0c4e88892c199f73357b5158eaff6d1acb`.

Implemented capabilities:

- signed Stripe `refund.created`, `refund.updated`, `refund.failed` event normalization;
- original-payment / Order / Enrollment resolution from Twenty by PaymentIntent;
- serialized `refund.apply` command in the commerce Durable Object;
- persistent refund receipts and cumulative refund accounting;
- separate REFUND PaymentRecord outbox effect;
- full-refund Enrollment updates and Order `REFUNDED` transition;
- consumed-reservation refund transition and capacity restoration;
- refund replay/idempotency handling;
- late paid/expired checkout protection after refund;
- staging acceptance harness that creates a real Stripe Sandbox refund and waits for public signed-webhook reconciliation.

## CI evidence

Current PR head `df1e4905f0cc08859dcb292e8fe300d2169899b9`:

- bootstrap run `36254967786`: PASS;
- P6 commerce-contract run `36254967709`: PASS;
- Stripe sandbox run `36254967788`: PASS;
- web run `36254967740`: PASS.

The P6 commerce-contract specifically proves locally against the serialized Durable Object boundary:

- protected 2-seat payment consumes capacity;
- partial refund EUR 40 of EUR 120 records refund value but leaves reservation `CONSUMED` and capacity at 10 available;
- second refund completing EUR 120 cumulative marks the refund as full;
- consumed reservation moves to `REFUNDED` and capacity returns to 12 available;
- duplicate full-refund replay is idempotent;
- over-refund is rejected.

The web workflow also passed Astro type-check, Cloudflare build, commerce tests, production-runtime browser checks and Lighthouse. A WebKit design-demo accessibility test was reported flaky but the workflow concluded SUCCESS; it is not part of the refund path.

## Real deployed acceptance target

Use the existing already-paid COUPLE transaction from successful run #18 so no additional purchase is created:

- Checkout Session: `cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`;
- PaymentIntent: `pi_3UJwa6620hE08wgd1wqZOsXI`;
- Order: `e616c4a0-5608-4d73-ac81-5edc2c072df3`;
- baseline Order state: `PAID`;
- baseline PaymentRecords: exactly one original PAYMENT;
- baseline Enrollments: exactly two `CONFIRMED`;
- baseline protected reservation: two consumed seats;
- baseline available capacity: 10 of 12;
- baseline public checkout state: `PAID`, `verified: true`.

The staging acceptance harness performs an intentionally irreversible **full Stripe Sandbox refund of EUR 120** against that test transaction. It first validates the baseline, ensures the staging Stripe webhook endpoint subscribes to refund events, then creates the refund through the Stripe API. It does not synthesize the refund webhook.

## PASS criteria

Promote refund/reconciliation to **RESULT / PASS** only if one deployed staging run proves together:

1. Stripe Sandbox creates a real succeeded full Refund for the existing PaymentIntent;
2. Stripe Events API exposes the corresponding real `refund.created` Event;
3. the signed public Formalife webhook processes the refund;
4. original PAYMENT remains preserved;
5. exactly one separate succeeded REFUND PaymentRecord exists for the Stripe Refund id and EUR 120 amount;
6. Order is `REFUNDED`;
7. the two existing Enrollments are `CANCELLED` with `STRIPE_REFUND` provenance, without creating replacement Enrollment records;
8. protected reservation is `REFUNDED`;
9. active reserved seats = 0, consumed reserved seats = 0, available seats = 12;
10. canonical `refund:<RefundId>` command is `MIRRORED` and all outbox effects are delivered with no `last_error`;
11. public server-side checkout state is `REFUNDED`, `verified: true`.

Only after all eleven hold may the refund gate be closed.

## Current next action

Run `cloudflare-staging` from `formalife/platform/main` at `33988b0c4e88892c199f73357b5158eaff6d1acb` or later with:

- `paid_acceptance = none`
- `paid_recovery_session_id =` blank
- `refund_acceptance_session_id = cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`

This action intentionally refunds the existing Stripe Sandbox COUPLE transaction; it does not create a new payment and does not affect real money.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- signed expiry;
- real SINGLE payment;
- real COUPLE payment;
- duplicate/reordered delivery resilience;
- verified Twenty mirror and serialized capacity path.

Open at minimum:

- deployed real refund/reconciliation RESULT;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- retry/backoff hardening for persistent downstream failures;
- final P6 closure after remaining gates are proven.
