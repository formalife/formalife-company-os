# P6 Stripe Delivery Resilience

Status: CURRENT RESULT — DUPLICATE / REORDERED DELIVERY ACCEPTANCE PASS; REFUND / RECONCILIATION NEXT
Date: 2026-09-26

## Context

The successful-payment acceptance is already closed / PASS for both Direct Full variants in `P6_STRIPE_PAID_ACCEPTANCE_2026-09-26.md`:

- SINGLE: real Stripe Sandbox EUR 80, one PaymentRecord, one Enrollment, one consumed seat, verified public `PAID`;
- COUPLE: Cloudflare staging run `36248322670` (#18), real Stripe Sandbox EUR 120, one PaymentRecord, two Enrollments, two consumed seats, verified public `PAID`.

This record covers the next P6 gate: duplicate and out-of-order Stripe webhook deliveries must not duplicate financial/seat effects or reverse an already-paid transaction.

## Governing evidence

Pure contract coverage already proved:

- repeated `checkout.session.completed` normalizes to the same `payment:<PaymentIntent>` idempotency key and deterministic `payment.apply` payload;
- a stale `checkout.session.expired` received after the linked Order is already `PAID` returns `ORDER_ALREADY_PAID` and does not release capacity or cancel the Order.

PR #42 added deployed staging acceptance without changing production commerce transitions.

## PR #42 — deployed delivery-resilience acceptance harness

**TEST — IMPLEMENTED, CI PASS, MERGED.**

- PR #42 — `Prove duplicate and reordered Stripe delivery resilience`;
- tested head: `f8c8e699f078875f1039c9a8523f8e4b40bac157`;
- merge commit / platform main: `114ea45147037af3a04229094b15fd3e72694eec`;
- bootstrap run `36249121802`: PASS;
- P6 commerce-contract run `36249121810`: PASS.

The recovery harness retrieves the original real `checkout.session.completed` Event for an already-paid Session, replays that exact event payload through the public deployed webhook with a fresh valid endpoint signature, then submits a deliberately stale signed `checkout.session.expired` payload derived from the same paid Session.

### Provenance boundary

The duplicate-completed portion uses the **actual Stripe Sandbox Event payload**, but retransmission is performed by the Formalife acceptance harness. It is a public-webhook replay test, not a claim that Stripe itself retransmitted the event during this run.

The stale-expiry portion is explicitly **synthetic/adversarial** and correctly signed. It is not a Stripe-emitted expiry event. The genuine signed expiry path is independently proven by run #11.

## Run #19 — deployed duplicate / reordered delivery acceptance

**RESULT — PASS. DUPLICATE / REORDERED DELIVERY GATE CLOSED.**

- workflow: `cloudflare-staging`;
- run: `36249376747`;
- run number: `19`;
- attempt: `1`;
- deployed commit: `114ea45147037af3a04229094b15fd3e72694eec`;
- job: `108424420676`;
- conclusion: `SUCCESS`;
- evidence artifact: `10908557725`;
- artifact digest: `sha256:c1238d2f33117d072216e7aa28a2e1e149603fadbe869c555e7d7d90d7749de5`;
- tested real COUPLE Checkout Session: `cs_test_a1RI6WlAB7PpAkqsqGMM0rYmOW1wF8m5eVzIQwj7SVFDZWLN5nlpStBFKP`;
- source real completed Event: `evt_1UJwa7620hE08wgdmEoKcnnQ`;
- PaymentIntent / canonical command: `pi_3UJwa6620hE08wgd1wqZOsXI` / `payment:pi_3UJwa6620hE08wgd1wqZOsXI`.

Deployed evidence proves together:

1. the real completed Event payload was replayed twice through the public webhook;
2. both deliveries returned replay semantics (`firstReplay = true`, `secondReplay = true`);
3. both resolved to the same canonical payment command;
4. command remained `MIRRORED`;
5. exactly `1` PaymentRecord remained;
6. exactly `2` Enrollments remained;
7. active reserved seats remained `0`;
8. consumed reserved seats remained `2`;
9. available seats remained `10` of `12`;
10. the signed stale-expiry adversarial event returned `ORDER_ALREADY_PAID`;
11. Order remained `PAID`;
12. public state remained `PAID`, `verified: true`.

No duplicate financial record, duplicate enrollment, release, cancellation or capacity mutation occurred.

## Current acceptance gate

Duplicate / reordered delivery is **CLOSED / PASS**.

The next P6 gate is **refund / reconciliation**. It must prove against a real paid Stripe Sandbox transaction that the refund is represented exactly once, financial and Order state reconcile correctly, downstream operational policy is explicit, and replay / duplicate refund delivery is idempotent.

## P6 status

P6 remains **OPEN**.

Closed / proven:

- genuine signed expiry cancellation/release path;
- real SINGLE successful-payment acceptance;
- real COUPLE successful-payment acceptance;
- duplicate completed-event delivery resilience;
- stale out-of-order expiry after payment is safely ignored;
- deployed Cloudflare web/commerce Service Binding path;
- verified Twenty payment/enrollment mirror for SINGLE and COUPLE.

Still open at minimum:

- refund / reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- retry/backoff hardening for persistent downstream failures.