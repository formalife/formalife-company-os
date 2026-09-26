# P6 Stripe Delivery Resilience

Status: CURRENT TEST — DUPLICATE / REORDERED DELIVERY HARNESS MERGED; DEPLOYED PROOF PENDING
Date: 2026-09-26

## Context

The successful-payment acceptance is already closed / PASS for both Direct Full variants in `P6_STRIPE_PAID_ACCEPTANCE_2026-09-26.md`:

- SINGLE: real Stripe Sandbox EUR 80, one PaymentRecord, one Enrollment, one consumed seat, verified public `PAID`;
- COUPLE: Cloudflare staging run `36248322670` (#18), real Stripe Sandbox EUR 120, one PaymentRecord, two Enrollments, two consumed seats, verified public `PAID`.

The next P6 gate is delivery resilience: duplicate and out-of-order Stripe webhook deliveries must not duplicate financial/seat effects or reverse an already-paid transaction.

## Governing evidence

Current production behavior already has pure contract coverage:

- repeated `checkout.session.completed` normalizes to the same `payment:<PaymentIntent>` idempotency key and deterministic `payment.apply` payload;
- a stale `checkout.session.expired` received after the linked Order is already `PAID` returns `ORDER_ALREADY_PAID` and does not release capacity or cancel the Order.

That pure coverage is necessary but not sufficient for the P6 deployed gate. The missing proof is the public Cloudflare staging path with real persisted Twenty/commerce state.

## PR #42 — deployed delivery-resilience acceptance harness

**TEST — IMPLEMENTED, CI PASS, MERGED. NOT YET A RESULT.**

Implementation repository: `formalife/platform`.

- PR #42 — `Prove duplicate and reordered Stripe delivery resilience`;
- tested head: `f8c8e699f078875f1039c9a8523f8e4b40bac157`;
- merge commit / platform main: `114ea45147037af3a04229094b15fd3e72694eec`;
- bootstrap run `36249121802`: PASS;
- P6 commerce-contract run `36249121810`: PASS;
- syntax validation: PASS;
- direct Full pure contract tests: PASS;
- commerce Worker bundle: PASS;
- local serialized capacity invariants: PASS.

PR #42 changes the recovery acceptance harness only. It does not change production commerce transitions, Stripe processing rules, capacity behavior or Twenty schema.

The extended recovery test, when pointed at an already-paid real Checkout Session, now:

1. retrieves the real Stripe Checkout Session from the Sandbox API;
2. resolves the original real `checkout.session.completed` Event for that Session from the Stripe Events API;
3. posts that exact Event payload twice through the deployed public Formalife webhook with a fresh valid signature generated using the configured endpoint signing secret;
4. requires both completed deliveries to resolve to the same canonical `payment:<PaymentIntent>` command as replays;
5. verifies after replay that the Order remains `PAID`, PaymentRecord count remains one, Enrollment count remains unchanged, consumed capacity remains unchanged, command remains `MIRRORED`, and public state remains `PAID`, `verified: true`;
6. sends an intentionally stale signed `checkout.session.expired` payload derived from the same paid real Session;
7. requires the stale expiry to be ignored as `ORDER_ALREADY_PAID`;
8. verifies again that no Order, payment, enrollment or capacity state changed.

### Provenance boundary

The duplicate-completed portion uses the **actual Stripe Sandbox Event payload** resolved from the account, but the replay request is submitted by the acceptance harness with a fresh valid endpoint signature. It is therefore a public-webhook replay test, not a claim that Stripe itself retransmitted the event during the test.

The stale-expiry portion is explicitly **synthetic/adversarial**: it is a correctly signed out-of-order payload derived from the real paid Session. It must never be described as a genuine Stripe-emitted expiry event. The genuine signed expiry path is already independently proven by run #11.

## Current acceptance gate

Use the real already-paid COUPLE transaction from run #18 so no additional payment is created:

- Checkout Session: `cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`;
- PaymentIntent: `pi_3UJwa6620hE08wgd1wqZOsXI`;
- Order: `e616c4a0-5608-4d73-ac81-5edc2c072df3`;
- baseline expected state: Order `PAID`, one PaymentRecord, two Enrollments, zero active reserved seats, two consumed reserved seats, ten available seats, public `PAID` / `verified: true`.

Run `cloudflare-staging` from `formalife/platform/main` at `114ea45147037af3a04229094b15fd3e72694eec` or later with:

- `paid_acceptance = none`
- `paid_recovery_session_id = cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`

Promote duplicate/reordered to **RESULT / PASS** only if deployed evidence proves:

1. two completed-event replays both return replay semantics against the same canonical payment command;
2. exactly one PaymentRecord remains;
3. exactly two Enrollments remain;
4. reservation remains consumed, not released;
5. available capacity remains 10 of 12;
6. stale expiry returns `ORDER_ALREADY_PAID`;
7. Order remains `PAID`;
8. canonical payment command remains `MIRRORED`;
9. public state remains `PAID`, `verified: true`.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- genuine signed expiry cancellation/release path;
- real SINGLE successful-payment acceptance;
- real COUPLE successful-payment acceptance;
- deployed Cloudflare web/commerce Service Binding path;
- verified Twenty payment/enrollment mirror for SINGLE and COUPLE.

Open after this TEST at minimum:

- deployed duplicate/reordered delivery RESULT;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- retry/backoff hardening for persistent downstream failures.