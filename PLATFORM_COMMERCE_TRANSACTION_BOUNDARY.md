# Formalife Commerce Transaction Boundary

Status: CURRENT DECISION — STAGING PROVEN
Date: 2026-09-25

## Trigger

P5 Twenty Cloud staging acceptance run `formalife/platform` GitHub Actions `36131378103` executed all scenarios S1-S11 against Twenty Cloud `v2.42.7`.

Observed first-run results:

- S1 PASS;
- S2 PASS;
- S3 PASS;
- S4 GAP / ARCHITECTURE REVIEW REQUIRED;
- S5 PASS;
- S6 PASS;
- S7 FAIL / ARCHITECTURE REVIEW REQUIRED;
- S8 FAIL / ARCHITECTURE REVIEW REQUIRED;
- S9 PASS;
- S10 PASS;
- S11 PASS.

The failures were not missing CRM fields. They exposed a missing guarded write boundary:

- duplicate Stripe provider identity was rejected by the Twenty unique index, but exactly-once multi-record side effects under retry/partial failure/reordering were not proven;
- a redeemed Training Credit could be changed back to `ACTIVE` through an ordinary Twenty API update;
- a second refresh Entitlement could be issued from the same origin Enrollment;
- an already redeemed Entitlement could be repointed to a second redemption through an ordinary Twenty API update.

## Decision

Formalife uses a **minimal Cloudflare commerce transaction boundary** before protected commerce mutations reach Twenty.

Current implementation: a **SQLite-backed Cloudflare Durable Object** used as the serialized command/invariant coordinator for Block 1 commerce operations.

This is a transaction/process boundary, not a replacement CRM and not a new broad business-data system.

### Source-of-truth split

- **Stripe:** payment-event and payment-status truth.
- **Commerce transaction boundary:** durable command receipt, idempotency, protected state-machine transitions, capacity/allocation invariants, retry/reconciliation state and minimal outbox/process truth.
- **Twenty:** operational CRM mirror, customer/household/course operational view, relations, history and reconciliation references.
- **PostHog:** behavioral analytics only; never payment/consent/transaction truth.

The boundary must not silently become a duplicate general-purpose customer database.

## Why Twenty alone is insufficient

P5 proved that Twenty can represent the operational domain well, but ordinary record mutations do not enforce all cross-record/state-transition invariants required by the business.

Adding isolated CRM workarounds would not solve the upstream problem:

- unique external payment identity alone does not make a multi-record write atomic or recoverable after partial failure;
- a select field does not itself enforce an allowed state transition;
- a relation does not itself guarantee single-use redemption;
- capacity derived from Enrollments still needs serialized allocation when two valid purchases compete for the same last seat.

Therefore protected commerce operations are commands, not arbitrary field edits.

## Protected command surface

The boundary owns or must own at least:

1. payment application / reconciliation from Stripe;
2. seat allocation and release;
3. transfer and cancellation mutations that affect capacity;
4. Training Credit issuance, redemption, expiry/reversal transitions;
5. refresh Entitlement issuance, redemption, expiry/termination transitions;
6. durable idempotency and retry bookkeeping for the above.

Ordinary Twenty users/API clients must not be the normal mutation path for fields whose correctness depends on those invariants.

## Implementation shape

For current Formalife volume, use one logically global Block-1 commerce coordinator rather than premature sharding.

The SQLite-backed Durable Object persists only the minimum required transaction/process state, including:

- command/event receipt identity and payload hash;
- processing state / attempt metadata;
- protected resource keys and transition state needed to reject invalid/replayed commands;
- minimal allocation/invariant state where serialized decisions are required;
- outbox/reconciliation effects for the Twenty mirror.

Processing pattern:

1. authenticate/verify the incoming command or Stripe event;
2. normalize it to a deterministic command/idempotency key;
3. enter the Durable Object;
4. atomically validate and persist the protected transition plus required outbox effects;
5. acknowledge only after durable commit;
6. mirror/reconcile Twenty idempotently;
7. retry downstream mirror failures from durable process state rather than from human memory/manual spreadsheets.

Twenty-side unique indexes remain useful defense-in-depth and reconciliation guards; they are not the transaction coordinator.

## P5 validation result

**RESULT: STAGING PROVEN; P5 REMEDIATION COMPLETE.**

The boundary prototype and Twenty defense-in-depth constraint were implemented in `formalife/platform` and merged via PR #18 at merge commit `e18214f4d310e6b15bc811f8271370ae3bbbd5ca`, preserving tested head `580edad04ea42d5c4446cf450a5b2ef1b9f8a38a`.

Focused real-staging run `36135292597` against Twenty Cloud `v2.42.7` passed all previously failing/gapped scenarios. Evidence artifact: `10863916308`; SHA-256 `3a0ac63b641318785e7e91cb5676258804d1615e539325f8e8c5d6d1477d638a`.

### S4 — payment idempotency / partial failure

**PASS.**

- an injected downstream `PAYMENT_CREATE` failure left durable pending reconciliation work;
- replay of the same idempotency key converged to `MIRRORED`;
- the failed payment mirror effect required two delivery attempts;
- repeated Stripe provider identity was deduplicated;
- conflicting payload under the same idempotency key returned `IDEMPOTENCY_CONFLICT`;
- final Twenty state contained exactly one PaymentRecord, exactly one Enrollment and Order `PAID`.

### S7 — Training Credit protected lifecycle

**PASS.**

- duplicate issuance for the same origin Order was rejected;
- a day-30 redemption used the approved EUR 29.90 Momentum value;
- second redemption was rejected with `INVALID_TRAINING_CREDIT_TRANSITION`;
- operational reactivation is not exposed by the supported command surface;
- explicit reversal reached `REVERSED` and mirrored an audit reason into Twenty.

### S8 — refresh Entitlement single issuance / single use

**PASS.**

- duplicate issuance was rejected by the transaction boundary;
- a direct duplicate Twenty write was independently rejected by the new UNIQUE Entitlement-origin-Enrollment constraint;
- first redemption was persisted;
- second redemption was rejected with `INVALID_ENTITLEMENT_TRANSITION`;
- the Twenty mirror remained attached to the first redemption target.

This proof closes the P5 remediation requirement for the **supported operational path**. It does not mean unrestricted direct administrative Twenty mutation is safe or intended.

## P6 integration/security boundary

Before production commerce traffic depends on this architecture:

- Stripe webhook signatures must be verified before commands enter the boundary;
- duplicate/reordered real Stripe events must be mapped to deterministic idempotency keys;
- direct protected Twenty mutation credentials must remain server-side behind the commerce boundary;
- the application/customer-facing path must not receive the administrative staging mutation capability used for validation;
- reconciliation/exception procedures must remain explicit and testable;
- the boundary must prove capacity behavior under the direct Full vertical slice, including 1- and 2-Caregiver purchases and failed-payment zero-seat behavior.

These are P6 production-integration requirements, not reasons to reopen P5 unless new evidence invalidates the staging proof.

## Cloudflare provider fit

CURRENT PROVIDER FACT / REVISION CONDITION, checked 2026-09-25:

- Cloudflare supports SQLite-backed Durable Objects on Workers Free and Paid plans;
- SQLite-backed Durable Object storage is strongly consistent/transactional within an object;
- Durable Object alarms provide at-least-once retry semantics and can support durable reconciliation work;
- new Durable Object namespaces are expected to use the SQLite backend.

This provider fit is not a promise that production infrastructure will always remain free. Re-review pricing/limits before production launch and when traffic materially changes.

## Rejected alternatives for this gate

### Keep all protection inside Twenty with manual procedures

Rejected. P5 directly proved mutable state-machine violations and this would hide business invariants in operator discipline.

### Add only more Twenty fields

Rejected. More fields improve representation but do not solve serialization, partial-failure recovery or guarded transitions.

### Introduce a second general-purpose operational database/CRM now

Rejected. It would create duplicate truth and unnecessary operational surface. The persistence remains narrow: transaction/process/invariant state only.

## Revision conditions

Revisit this decision if any of the following becomes true:

- later end-to-end Stripe testing contradicts the P5 staging proof;
- Cloudflare pricing/limits materially conflict with Formalife economics;
- throughput/availability requirements make a single coordinator inappropriate;
- Twenty introduces a native transactional/state-machine facility that proves the same invariants with less system complexity;
- legal/privacy requirements force a different persistence boundary.

Until then, this boundary is the current architecture for P6 commerce integration.
