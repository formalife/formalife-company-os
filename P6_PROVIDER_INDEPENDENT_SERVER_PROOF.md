# P6 Provider-Independent Server Proof

Status: CURRENT RESULT — P6 REMAINS OPEN
Date: 2026-09-25

## Scope

This record captures the provider-independent portion of the direct Full P6 vertical slice through `formalife/platform` PR #23.

It does **not** claim Stripe, Brevo, PostHog or Cloudflare external integration acceptance.

## Current implementation evidence

### PR #22 — server/application seams

Merged implementation:

- `formalife/platform` PR #22;
- merge commit `8a57bb98c63905715db9f424ef4a003aeee98ff3`;
- exact tested head preserved in `main`: `324c380eee76d53f8acc6da29001f66c29fe992f`.

CI / staging evidence on the tested head:

- bootstrap run `36153778830`: SUCCESS;
- P6 commerce-contract run `36153778933`: SUCCESS;
- commerce artifact `10872123902`, SHA-256 `23882f34aef84a5808a7f2ac9babe21a5c2a4bebcb20acf0122cf3174d35a052`;
- Twenty Cloud application run `36153778945`: SUCCESS against Twenty `v2.42.8`;
- Twenty artifact `10873130747`, SHA-256 `25c64d5755dc8d7ebb002d173d07e3034e5f42c7f56389e7ca719ceb9a6e503a`;
- full web regression `36153779029`: SUCCESS, including typecheck, production build, browser/accessibility tests, production runtime-error tests and Lighthouse;
- commerce unit contracts: 50 PASS / 0 FAIL.

### PR #23 — HTTP endpoint surface

Merged implementation:

- `formalife/platform` PR #23;
- merge commit `7c81bc40dd087355af19ecf12474eb24b0fafa8f`;
- exact tested head preserved in `main`: `764c833b9976c3ecf1c9d1cbfd8503ef786a00a0`.

CI evidence:

- bootstrap run `36154797392`: SUCCESS;
- P6 commerce-contract run `36154797505`: SUCCESS;
- commerce artifact `10873112089`, SHA-256 `10b65dde6fceab711e3e445cd8f87845ae9ec1d2007471ff003c519afe3607a1`;
- full web regression `36154797400`: SUCCESS, including Astro typecheck, Cloudflare production build, commerce tests, browser/accessibility tests, production runtime-error tests and Lighthouse.

## Founder decision — direct Stripe integration path

**DECISION — CURRENT, 2026-09-25:** do not make the ChatGPT Stripe plugin/OAuth connector a prerequisite for Formalife P6.

The current Stripe plugin OAuth callback is unreliable in the founder environment. Formalife will therefore integrate Stripe through normal server-side Stripe credentials and webhook signing secrets stored directly in the deployment/CI secret stores, never in chat or source control.

For staging/new integration work, use a dedicated Stripe Sandbox as the payment environment. The plugin may be connected later for operational convenience, but it is not part of the runtime architecture or P6 exit gate.

This decision does not authorize live-mode payment processing yet. P6 must first pass the real Sandbox acceptance path below.

## Results now proven

### Payment configuration boundary

**RESULT:** missing real payment-provider configuration fails before durable commerce or Twenty identity mutations.

A test-only fake provider remains explicit and injected; it is not a runtime fallback.

### Purchaser identity

**RESULT:** the server resolves purchaser identity in Twenty by normalized primary email, not by name.

- no matching Person -> create Person;
- one matching Person -> reuse it;
- multiple matching People -> fail closed as ambiguous;
- no automatic name-based merge is allowed.

Real Twenty Cloud staging proved creation and replay without duplicate Person creation.

### Household identity

**RESULT:** an active purchaser Household is reused when already linked. A recoverable partial setup can be repaired when the Person is the Household primary contact but the reverse Person relation is absent.

A new purchaser with no Household receives one active Household and is linked to it.

Real Twenty Cloud staging proved purchaser Person + Household replay stability.

### Two-caregiver identity — implementation rule for current P6 slice

For the current direct-Full implementation slice, two occupied seats require two distinct caregiver Person identities.

The companion is resolved by email and can be linked to the purchaser Household. If an existing companion Person already belongs to another active primary Household, the system fails closed rather than silently reassigning the Person.

This is an **implementation rule for P6 identity integrity**, not a permanent commercial doctrine about what contact fields every future offer must collect.

Real Twenty Cloud staging proved companion creation and replay stability in the same Household.

### Protected checkout context

**RESULT:** the Durable Object seat reservation now persists the minimum protected operational context needed by later payment processing:

- Household ID;
- ordered participant Person IDs.

This context stays behind the commerce boundary rather than depending on browser state or provider metadata containing customer PII.

Identical reservation replay is idempotent. Changed Household/participant context under the same reservation identity is rejected.

Payment application can validate that final Enrollment participant/Household effects match the protected checkout context.

### Failure ordering / rollback

**RESULT:** provider-independent checkout orchestration has explicit failure behavior.

- reservation failure -> pre-checkout Order cancellation;
- provider checkout creation failure -> reservation release + Order cancellation;
- Twenty session-link failure -> first attempt to invalidate/expire provider checkout;
- if provider invalidation is confirmed -> release reservation + cancel Order;
- if provider invalidation cannot be confirmed -> do **not** release capacity optimistically; retain the protected reservation until expiry/reconciliation and raise an explicit reconciliation-required state.

This prevents an uncertain still-payable checkout from releasing the same last seat to another buyer.

### Verified success state

**RESULT:** customer-facing success state is derived from server-side Twenty Order state linked to the checkout session. Browser redirect parameters do not establish payment success.

### HTTP application surface

**RESULT: CODE/CI PROVEN; NOT EXTERNALLY DEPLOYED.**

The Astro/Cloudflare application now exposes:

- `POST /api/commerce/full/checkout`;
- `GET /api/commerce/full/status`.

The checkout endpoint requires a stable `Idempotency-Key`, accepts JSON only, emits `no-store`, sanitizes internal errors and delegates all business state to the provider-independent server/application layer.

Current production runtime intentionally instantiates `UnavailablePaymentProvider`; therefore checkout returns HTTP 503 before Person, Household, Order or capacity mutation until a real Stripe adapter is installed and configured.

The status endpoint reads server-side verified operational state rather than trusting browser redirect data.

Astro typecheck and the Cloudflare production build both pass with the current `cloudflare:workers` runtime environment binding pattern.

This proves the endpoint/runtime contract only. No real Cloudflare preview/staging deployment has yet been accepted.

### Brevo contract

**RESULT: CONTRACT-TESTED ONLY.**

The implementation has:

- fail-closed unavailable messenger;
- Brevo transactional adapter using the transactional email API contract;
- deterministic test messenger;
- template-based confirmation contract.

No real Brevo delivery has been proven yet.

### PostHog contract

**RESULT: CONTRACT-TESTED ONLY.**

The implementation has:

- fail-closed unavailable analytics sink;
- PostHog HTTP capture adapter;
- deterministic test analytics sink;
- explicit privacy-safe allowlist for commerce properties;
- person-profile processing disabled for the server commerce event contract.

No real deployed PostHog event delivery has been proven yet.

## P6 remains open

The direct Full vertical slice is **not production/revenue accepted** until external proof covers at least:

1. real Stripe Sandbox Checkout Session creation for 1-Caregiver and 2-Caregiver options;
2. raw-body Stripe webhook signature verification;
3. duplicate and reordered real Stripe event handling;
4. successful payment, failed payment and refund/reconciliation paths;
5. real provider session lifetime wired to reservation consume/release/expiry;
6. final paid Twenty Order / PaymentRecord / Enrollment effects;
7. real Brevo transactional confirmation delivery;
8. real privacy-safe PostHog event delivery;
9. real Cloudflare staging/preview deployment of the public app and commerce boundary;
10. deployed end-to-end Single and Couple acceptance.

## Governing boundary

Stripe remains payment truth. The commerce Durable Object remains protected command/idempotency/capacity/process truth. Twenty remains the operational CRM mirror. PostHog remains analytics only.

Do not broaden into Guide/Training Credit public purchase flows or activation/pre-enrolment exceptions before direct Full P6 passes its exit gate.
