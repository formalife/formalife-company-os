# Formalife Commerce Transaction Boundary

Status: CURRENT DECISION — STAGING PROVEN; P6 CAPACITY CONTRACT PROVEN LOCALLY
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

## P6 provider-independent capacity proof

**RESULT: LOCAL/CI PROVEN; EXTERNAL PROVIDER PROOF STILL OPEN.**

`formalife/platform` PR #19 extends the same Durable Object boundary with serialized direct-Full seat reservations rather than relying only on a read-time availability check.

Merged implementation evidence:

- PR #19 merge commit `7831e00315eae75cdc7302585e81f7c94f64fbbd`;
- exact tested head preserved in `main`: `9dd2dbb85cc0b18ec1b8f47020de2c42e7e9c524`;
- P6 commerce-contract run `36148954303`: SUCCESS;
- evidence artifact `10869529274`, SHA-256 `04fc7b930f6a5e280b7b3226248e7cf563e7e4261affc96ad09b2bd3e40d1cf9`;
- full web regression run `36148954543`: SUCCESS;
- bootstrap run `36148954212`: SUCCESS.

The provider-independent P6 contract now proves in CI:

- canonical direct-Full options remain 1 Caregiver / one seat / EUR 80 and 2 Caregivers / two seats / EUR 120;
- only confirmed editions with sufficient observed capacity can enter checkout preparation;
- the Durable Object serializes the authoritative last-seat reservation decision;
- with edition capacity 12 and baseline occupancy 10, one two-seat reservation consumes the remaining capacity and a competing additional reservation is rejected with `INSUFFICIENT_CAPACITY`;
- identical reservation replay is idempotent while changed content under the same reservation identity is rejected;
- failed or expired checkout reservations release capacity;
- protected `payment.apply` can be configured to fail closed when no active reservation is supplied;
- successful payment commands carry the reservation identity and expected seat count into the guarded boundary;
- provider-object identity remains the payment idempotency key;
- browser redirect/session parameters alone never prove payment success;
- transactional confirmation requires verified `PAID` state;
- analytics contract excludes arbitrary customer PII.

Implementation note: `REQUIRE_CAPACITY_RESERVATION=false` remains the repository config default only to preserve P5/backward test compatibility. The P6 protected commerce runtime must set this requirement to `true`; the false default is **not** production acceptance evidence.

This local/CI proof resolves the known last-seat race at the application contract level. It does **not** prove real Cloudflare deployment, real Stripe behavior or production synchronization with Twenty.

## P6 integration/security boundary

Before P6 can close and before production commerce traffic depends on this architecture:

- a real Stripe test/sandbox account must create Checkout Sessions for both direct Full options;
- Stripe webhook signatures must be verified from the real raw request body before commands enter the boundary;
- duplicate/reordered real Stripe events must be mapped to deterministic idempotency keys and proven against the boundary;
- successful, failed and refund/reconciliation paths must be proven with real Stripe identifiers;
- the reservation lifecycle must be wired to the real checkout/session lifecycle with `REQUIRE_CAPACITY_RESERVATION=true`;
- direct protected Twenty mutation credentials must remain server-side behind the commerce boundary;
- the public application path must not receive the administrative staging mutation capability used for validation;
- Brevo transactional confirmation must be proven with a real staging/test delivery path;
- the commerce Worker and public web path must be deployed to real Cloudflare staging/preview infrastructure;
- success-state lookup must rely on verified server/payment/operational state, not the browser redirect;
- reconciliation/exception procedures must remain explicit and testable.

These are P6 integration requirements, not reasons to reopen P5 unless new evidence invalidates the staging proof.

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

- later end-to-end Stripe testing contradicts the P5/P6 contract proof;
- Cloudflare pricing/limits materially conflict with Formalife economics;
- throughput/availability requirements make a single coordinator inappropriate;
- Twenty introduces a native transactional/state-machine facility that proves the same invariants with less system complexity;
- legal/privacy requirements force a different persistence boundary.

Until then, this boundary is the current architecture for P6 commerce integration.
