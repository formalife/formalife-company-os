# Formalife Commerce Transaction Boundary

Status: CURRENT DECISION
Date: 2026-09-25

## Trigger

P5 Twenty Cloud staging acceptance run `formalife/platform` GitHub Actions `36131378103` executed all scenarios S1-S11 against Twenty Cloud `v2.42.7`.

Observed results:

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

The failures are not missing CRM fields. They expose a missing guarded write boundary:

- duplicate Stripe provider identity is rejected by the Twenty unique index, but exactly-once multi-record side effects under retry/partial failure/reordering are not yet proven;
- a redeemed Training Credit can be changed back to `ACTIVE` through an ordinary Twenty API update;
- a second refresh Entitlement can be issued from the same origin Enrollment;
- an already redeemed Entitlement can be repointed to a second redemption through an ordinary Twenty API update.

## Decision

Formalife will introduce a **minimal Cloudflare commerce transaction boundary** before protected commerce mutations reach Twenty.

Initial implementation target: a **SQLite-backed Cloudflare Durable Object** used as the serialized command/invariant coordinator for Block 1 commerce operations.

This is a transaction/process boundary, not a replacement CRM and not a new broad business-data system.

### Source-of-truth split

- **Stripe:** payment-event and payment-status truth.
- **Commerce transaction boundary:** durable command receipt, idempotency, protected state-machine transitions, capacity/allocation invariants, retry/reconciliation state and minimal outbox/process truth.
- **Twenty:** operational CRM mirror, customer/household/course operational view, relations, history and reconciliation references.
- **PostHog:** behavioral analytics only; never payment/consent/transaction truth.

The boundary must not silently become a duplicate general-purpose customer database.

## Why Twenty alone is insufficient

P5 proves that Twenty can represent the operational domain well, but ordinary record mutations do not enforce all cross-record/state-transition invariants required by the business.

Adding isolated CRM workarounds would not solve the upstream problem:

- unique external payment identity alone does not make a multi-record write atomic or recoverable after partial failure;
- a select field does not itself enforce an allowed state transition;
- a relation does not itself guarantee single-use redemption;
- capacity derived from Enrollments still needs serialized allocation when two valid purchases compete for the same last seat.

Therefore protected commerce operations must be commands, not arbitrary field edits.

## Initial protected command surface

The boundary must own at least:

1. payment application / reconciliation from Stripe;
2. seat allocation and release;
3. transfer and cancellation mutations that affect capacity;
4. Training Credit issuance, redemption, expiry/reversal transitions;
5. refresh Entitlement issuance, redemption, expiry/termination transitions;
6. durable idempotency and retry bookkeeping for the above.

Ordinary Twenty users/API clients must not be the normal mutation path for fields whose correctness depends on those invariants.

## Initial implementation shape

For current Formalife volume, prefer one logically global Block-1 commerce coordinator rather than premature sharding.

The SQLite-backed Durable Object should persist only the minimum required transaction/process state, for example:

- command/webhook receipt identity and payload hash/reference;
- processing state / attempt metadata;
- protected resource keys and transition state needed to reject invalid/replayed commands;
- edition allocation/reservation state needed for serialized capacity decisions;
- an outbox/reconciliation queue for Twenty mirror effects.

Expected processing pattern:

1. authenticate/verify the incoming command or Stripe event;
2. normalize it to a deterministic command/idempotency key;
3. enter the Durable Object;
4. atomically validate and persist the protected transition plus required outbox effects;
5. acknowledge only after durable commit;
6. mirror/reconcile Twenty idempotently;
7. retry downstream mirror failures from durable process state rather than from human memory/manual spreadsheets.

Twenty-side unique indexes remain useful defense-in-depth and reconciliation guards; they are not the transaction coordinator.

## P5 remediation required before closure

P5 remains OPEN until the intended guarded write path/prototype is implemented and real staging acceptance re-proves the failing scenarios.

Required remediation/proof:

- S4: duplicate/replayed payment processing converges to one operational outcome and survives a simulated partial downstream failure/retry;
- S7: redeemed Training Credit cannot be operationally reactivated through the ordinary supported path, while reversal is explicit/auditable;
- S8: exactly one refresh Entitlement is issued per qualifying origin Enrollment and a redeemed Entitlement cannot be consumed twice through the ordinary supported path;
- defense-in-depth schema constraints are added where semantics are unambiguous, especially exactly-one Entitlement per origin Enrollment;
- protected Twenty mutation permissions/credentials are separated from the application command path so the admin-level staging API key is not representative of ordinary production operation.

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

Rejected. It would create duplicate truth and unnecessary operational surface. The new persistence must stay narrow: transaction/process/invariant state only.

## Revision conditions

Revisit this decision if any of the following becomes true:

- the Durable Object staging prototype cannot prove the P5 invariants;
- Cloudflare pricing/limits materially conflict with Formalife economics;
- throughput/availability requirements make a single coordinator inappropriate;
- Twenty introduces a native transactional/state-machine facility that proves the same invariants with less system complexity;
- legal/privacy requirements force a different persistence boundary.

Until then, this boundary is the current architecture direction for P5 remediation and P6 commerce integration.
