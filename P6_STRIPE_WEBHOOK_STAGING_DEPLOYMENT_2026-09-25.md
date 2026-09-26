# P6 Stripe Webhook Staging Deployment

Status: CURRENT RESULT — BILLING BLOCK RESOLVED; PR #32 WORKER RUNTIME FIX MERGED; FRESH SIGNED-EXPIRY ACCEPTANCE PENDING
Date: 2026-09-26

## Scope

This record captures the Cloudflare staging deployment of the public Formalife Stripe webhook route, the endpoint-specific signing-secret installation, the real signed-expiry acceptance attempts, and the boundary between implementation/configuration proof and real signed-event acceptance.

It does **not** yet claim that a real Stripe webhook delivery has completed the Formalife operational path. That requires an actual Stripe Sandbox event linked to a real Formalife checkout/reservation and a passing deployed acceptance.

## Implementation lineage

Implementation repository: `formalife/platform`.

- PR #29 — `Add protected Stripe webhook endpoint`;
- PR #29 merge commit: `2ed5c4c303bdb375b7c88879c3ad2cdd026a846b`;
- public route: `POST /api/commerce/stripe/webhook`;
- PR #30 — `Wire optional Stripe webhook secret into staging`;
- PR #30 merge commit: `4e3419e819a842e135b4bd4049fa8c6aa405a746`;
- PR #31 — `Fix runtime UUIDs and add signed Stripe expiry acceptance`;
- PR #31 merge commit: `9da8497502b672e5b07005376a1d2d161557302f`;
- PR #32 — `Fix Worker fetch receiver in commerce runtime`;
- PR #32 tested head: `d7a4e079cd0c26669e3a46fae034586cb0cd0c98`;
- PR #32 merge commit: `57a219d66521cb58569f758cfa8a3b2d0b6e546e`.

PR #31 corrected the default public-checkout persisted IDs from prefixed strings to UUIDs and added a staging acceptance gate that creates a real Stripe Checkout Session, expires it through Stripe, and requires the signed `checkout.session.expired` delivery to cancel the Order and release capacity without creating financial or seat effects.

PR #32 corrected a Cloudflare Worker runtime defect exposed by the first real execution of that acceptance: the raw global `fetch` had been injected into adapter instances which later invoked it as `this.fetchImpl(...)`. In Cloudflare `workerd` this can violate the receiver/brand check and throw a native `TypeError`, while Node-based tests can remain green. The runtime now wraps the injected fetch once as a detached function before passing it to Twenty, commerce-boundary, Stripe and analytics adapters. A receiver-sensitive regression test was added.

## Initial public-route deployment evidence

- workflow: `cloudflare-staging`;
- run: `36174604916`;
- attempt: `1`;
- conclusion: `SUCCESS`;
- deployed commit: `4e3419e819a842e135b4bd4049fa8c6aa405a746`;
- evidence artifact: `10880994470`;
- artifact SHA-256: `a46cebb2e095f90fa539f29ae47a296c6be5e5e48436c97446cedac74918eea9`;
- evidence state: `stripeWebhookSecretConfigured: false`.

This attempt proved that the public webhook code surface was deployed while remaining fail-closed before signing-secret installation.

## Signing-secret installation evidence

After the founder created the Stripe Sandbox event destination and stored the endpoint-specific signing secret in GitHub Environment `staging` as `STRIPE_WEBHOOK_SECRET`, the exact same deployed commit/workflow was rerun to isolate configuration as the only changed variable.

- workflow run: `36174604916`;
- attempt: `2`;
- job: `108208003095`;
- conclusion: `SUCCESS`;
- deployed commit: `4e3419e819a842e135b4bd4049fa8c6aa405a746`;
- evidence artifact: `10881783840`;
- artifact SHA-256: `f3dfc876a9976cb65138b0eff6e8dc5ebd81b4767a69922e82266dd2b5eea641`;
- evidence state: `stripeWebhookSecretConfigured: true`;
- deployed at: `2026-09-25T18:55:42.981Z`.

The deployment logs explicitly showed Wrangler installing `STRIPE_WEBHOOK_SECRET` into `formalife-web-staging`; the secret value is not committed or emitted in evidence.

## Staging endpoints

- web: `https://formalife-web-staging.formalife-preview.workers.dev`;
- commerce boundary: `https://formalife-commerce-staging.formalife-preview.workers.dev`;
- Stripe webhook: `https://formalife-web-staging.formalife-preview.workers.dev/api/commerce/stripe/webhook`.

## What is proven before signed delivery

**RESULT — PUBLIC WEBHOOK CODE SURFACE AND ENDPOINT-SPECIFIC SIGNING SECRET ARE DEPLOYED.**

The route verifies the exact raw request body against `Stripe-Signature` before JSON interpretation, resolves the linked Twenty Order and protected reservation context, derives deterministic payment/enrollment identities for replay stability, and routes paid events through the guarded/idempotent commerce boundary.

`checkout.session.expired` is designed as a non-success capacity release path and cannot cancel an already-PAID Order.

The presence of the signing secret proves configuration, not delivery. A generic Stripe Dashboard test event is not sufficient because the processor correctly requires a Checkout Session already linked to a real Formalife Order and protected reservation.

## 2026-09-26 GitHub Actions billing block

**RESULT — RESOLVED.**

A new `workflow_dispatch` was launched on PR #31 main:

- workflow: `cloudflare-staging`;
- run: `36225365356`;
- run number: `5`;
- commit: `9da8497502b672e5b07005376a1d2d161557302f`;
- event: `workflow_dispatch`;
- attempt 1 job: `108358235428`;
- attempt 1 conclusion: `FAILURE` before any workflow step;
- attempt 2 job: `108361292919`;
- attempt 2 conclusion: `FAILURE` before any workflow step.

A control rerun of `bootstrap` on the same commit also failed before any step. The founder then inspected the GitHub UI, which explicitly reported that recent account payments had failed or the spending limit needed to be increased. This confirmed a GitHub billing/spending control block upstream of repository checkout or application execution.

After the founder corrected the account billing/spending condition, attempt 3 of the same run was accepted and a GitHub-hosted runner started normally. Therefore the billing/runner block is **closed**.

## 2026-09-26 attempt 3 — first real application execution

**RESULT — FAIL AT PUBLIC CHECKOUT BEFORE STRIPE EXPIRY.**

Run `36225365356`, attempt 3, job `108364518006` successfully completed:

- runner setup;
- repository checkout;
- dependency install;
- repository contract;
- staging configuration validation;
- Astro production build;
- Cloudflare commerce-boundary deployment;
- Cloudflare public-web deployment;
- runtime-secret installation;
- web redeployment;
- deployed-service smoke test.

The signed-expiry acceptance then reached the real public commerce route and failed on the initial Formalife checkout request with HTTP `500` and the public error code `INTERNAL_ERROR` / `The commerce service could not complete the request`.

The acceptance therefore did **not** yet reach:

- Stripe Checkout Session expiry;
- Stripe signed `checkout.session.expired` delivery;
- webhook signature verification;
- Order cancellation;
- reservation release.

This is application/runtime evidence, not webhook-failure evidence.

**Failure classification:** application/source defect in the deployed Worker runtime, upstream of Stripe expiry delivery.

No evidence artifact was produced for the signed-expiry scenario because the acceptance failed before the evidence-writing stage.

## Root cause and remediation — PR #32

The public handler returns `INTERNAL_ERROR` only for an exception outside the structured `DirectFullContractError` path. Investigation found that `createCommerceRuntime` passed the raw global `fetch` into adapters which stored it and invoked it as an instance method. This can change the receiver of the Web API function under Cloudflare `workerd` and cause a native `TypeError: Illegal invocation`, a failure mode not reproduced by Node's more permissive fetch implementation.

**REMEDIATION:** PR #32 wraps the runtime fetch as a detached function before injection into downstream adapters and adds a regression test with a deliberately receiver-sensitive fetch implementation.

PR #32 validation on tested head `d7a4e079cd0c26669e3a46fae034586cb0cd0c98`:

- bootstrap run `36227941282`: `SUCCESS`;
- P6 commerce-contract run `36227941336`: `SUCCESS`;
- Stripe Sandbox run `36227941297`: `SUCCESS`;
- full web run `36227941292`: `SUCCESS`;
- Astro type-check/build: PASS;
- direct Full commerce tests including new fetch-receiver regression: PASS;
- browser/accessibility development suite: PASS;
- production-build browser/runtime-error suite: PASS;
- Lighthouse baseline: PASS.

PR #32 was merged to `main` as `57a219d66521cb58569f758cfa8a3b2d0b6e546e`.

**Important:** CI success proves the remediation contract and regressions; it does not substitute for rerunning the deployed signed-expiry acceptance.

## Current next acceptance step

Launch a **new** `cloudflare-staging` workflow on current `formalife/platform/main` at `57a219d66521cb58569f758cfa8a3b2d0b6e546e` or a later main that includes PR #32. Do not rerun `36225365356` as proof of the fix because that run is permanently anchored to pre-fix commit `9da8497`.

The fresh workflow must prove the real expiry scenario:

1. create a synthetic CONFIRMED Twenty edition;
2. create a real public Formalife SINGLE checkout through Cloudflare;
3. verify one active reserved seat;
4. expire the real Stripe Sandbox Checkout Session through Stripe API;
5. receive the genuine signed `checkout.session.expired` delivery at the public webhook;
6. require Twenty Order `CANCELLED`;
7. require reservation release / capacity restored;
8. require zero PaymentRecord and zero Enrollment;
9. require public checkout status `CANCELLED`, verified server-side.

Only an actual PASS of this gate should promote signed-expiry handling from code/configuration proof to real external-delivery proof.

## P6 status

P6 remains **OPEN**.

Current closed prerequisites:

- public webhook route deployed;
- endpoint-specific Stripe Sandbox signing secret installed;
- raw-body verifier and processor code proven in CI;
- public runtime UUID defect corrected and merged in PR #31;
- GitHub Actions billing/spending runner block resolved;
- deployed public-checkout `fetch` receiver defect identified and remediated in merged PR #32 with full CI regression proof.

Still open at minimum:

- fresh deployed signed-expiry acceptance including PR #32;
- real successful-payment `checkout.session.completed` operational effects;
- duplicate/reordered delivery acceptance under real Stripe delivery;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- full SINGLE and COUPLE end-to-end acceptance.
