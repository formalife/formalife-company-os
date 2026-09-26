# P6 Stripe Webhook Staging Deployment

Status: CURRENT RESULT — RPC HYPOTHESIS REJECTED; REMOTE COMMERCE COORDINATOR FAILURE PENDING DIAGNOSTIC DEPLOY
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
- PR #32 merge commit: `57a219d66521cb58569f758cfa8a3b2d0b6e546e`;
- PR #33 — `Route P6 Durable Object boundary over HTTP`;
- PR #33 tested head: `d1dfa1cc666c57667c46a2738471c577837c3f14`;
- PR #33 merge commit: `012df300717f1e2299c937dc4d5ba9fa343678cf`;
- PR #34 — `Add deployed P6 coordinator diagnostics`;
- PR #34 tested head: `683a46b7f040833fc7aac8251d461f155d6dcb92`;
- PR #34 merge commit: `341cf7c1d45350eef22c265258336c6e9e3e09e7`.

PR #31 corrected the default public-checkout persisted IDs from prefixed strings to UUIDs and added a staging acceptance gate that creates a real Stripe Checkout Session, expires it through Stripe, and requires the signed `checkout.session.expired` delivery to cancel the Order and release capacity without creating financial or seat effects.

PR #32 corrected a Cloudflare Worker runtime defect exposed by the first real execution of that acceptance: the raw global `fetch` had been injected into adapter instances which later invoked it as `this.fetchImpl(...)`. In Cloudflare `workerd` this can violate the receiver/brand check and throw a native `TypeError`, while Node-based tests can remain green. The runtime now wraps the injected fetch once as a detached function before passing it to Twenty, commerce-boundary, Stripe and analytics adapters. A receiver-sensitive regression test was added.

PR #33 removed Durable Object RPC from the P6 commerce-boundary request/response hop and routed the same protected command contract through Durable Object `fetch()`. It preserved the Durable Object class name, SQLite storage, command/state logic, capacity rules and secrets. Run #8 later showed that this transport substitution did not resolve the deployed failure, so RPC is no longer the current root-cause hypothesis.

PR #34 adds evidence-producing diagnostics rather than asserting another root cause: authenticated coordinator transport/initialization exceptions are converted into a bounded structured diagnostic response, and staging now probes the deployed Durable Object command path before attempting public checkout. It also closes a CI coverage gap by adding the P6 HTTP entrypoint to the commerce-contract workflow path triggers.

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

Run `36225365356`, attempt 3, job `108364518006` successfully completed runner setup, checkout, dependency install, repository contract, staging configuration validation, Astro production build, both Cloudflare deployments, runtime-secret installation, redeployment and deployed-service smoke.

The signed-expiry acceptance then reached the real public commerce route and failed on the initial Formalife checkout request with HTTP `500` and public error code `INTERNAL_ERROR` / `The commerce service could not complete the request`.

The acceptance therefore did **not** yet reach Stripe Checkout Session expiry, signed delivery, webhook signature verification, Order cancellation or reservation release.

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
- Astro type-check/build, commerce tests, browser/accessibility, production runtime-error suite and Lighthouse baseline: PASS.

PR #32 was merged to `main` as `57a219d66521cb58569f758cfa8a3b2d0b6e546e`.

## 2026-09-26 run #7 — second deployed checkout failure

**RESULT — PR #32 MOVED THE FAILURE DOWNSTREAM; COMMERCE DURABLE-OBJECT BOUNDARY FAILED BEFORE STRIPE EXPIRY.**

Fresh workflow dispatch:

- workflow: `cloudflare-staging`;
- run: `36228409250`;
- run number: `7`;
- deployed commit: `57a219d66521cb58569f758cfa8a3b2d0b6e546e`;
- job: `108366752057`;
- conclusion: `FAILURE`;
- runner/build/deploy/secret-install/redeploy/service-smoke steps: `SUCCESS`;
- signed-expiry acceptance step: `FAILURE`.

The public checkout no longer returned the generic native-runtime `INTERNAL_ERROR`. It reached the commerce-boundary client and failed with HTTP `502`:

`{"error":"COMMERCE_BOUNDARY_NON_JSON","message":"The commerce service could not complete the request"}`

This is positive localization evidence: PR #32 corrected or bypassed the earlier fetch-receiver failure sufficiently for the request to reach the next dependency. The current first observed bottleneck is the protected commerce boundary `/commands` hop to the Durable Object.

All ordinary `FormalifeCommerceCoordinator.execute()` application errors are converted into structured serializable error results. Therefore a non-JSON boundary response is evidence of failure outside the normal command-error contract, plausibly Durable Object initialization/transport/serialization. It is **not** evidence of a Stripe failure and the acceptance still did not reach Stripe Checkout Session expiry or signed webhook delivery.

The deployed `/health` smoke did not detect this because `/health` does not exercise the Durable Object command path.

## Hypothesis and test — PR #33

**HYPOTHESIS — deployed Durable Object RPC is the failing transport layer.**

The outer commerce Worker used Durable Object RPC (`stub.execute()` / `stub.inspect()`) while the same command contract passed locally through Wrangler. Cloudflare supports Durable Object HTTP `fetch()` request/response flows, so replacing the RPC hop with `stub.fetch()` was a clean discriminating test that did not require changing commerce semantics.

PR #33 validation on tested head `d1dfa1cc666c57667c46a2738471c577837c3f14`:

- bootstrap run `36228900505`: `SUCCESS`;
- P6 commerce-contract run `36228900462`: `SUCCESS`;
- local configured Worker startup: PASS;
- real local `/commands` traversal through the new `stub.fetch()` path: PASS;
- serialized capacity/protected-context invariants: PASS;
- full web run `36228900432`: `SUCCESS`;
- type-check/build, commerce tests, browser/accessibility, production runtime-error suite and Lighthouse baseline: PASS.

PR #33 was merged to `main` as `012df300717f1e2299c937dc4d5ba9fa343678cf`.

## 2026-09-26 run #8 — RPC hypothesis rejected

**RESULT — REPLACING RPC WITH DURABLE OBJECT HTTP FETCH DID NOT CHANGE THE DEPLOYED FAILURE.**

Fresh workflow dispatch:

- workflow: `cloudflare-staging`;
- run: `36229376954`;
- run number: `8`;
- deployed commit: `012df300717f1e2299c937dc4d5ba9fa343678cf`;
- job: `108369492920`;
- conclusion: `FAILURE`;
- runner/build/deploy/secret-install/redeploy/service-smoke steps: `SUCCESS`;
- signed-expiry acceptance step: `FAILURE`.

The public checkout again failed with the same HTTP `502` response:

`{"error":"COMMERCE_BOUNDARY_NON_JSON","message":"The commerce service could not complete the request"}`

The failure persisted after the internal hop changed from Durable Object RPC to the officially supported `stub.fetch(Request)` request/response path, while the equivalent local configured Worker `/commands` contract remained green.

**RESULT — RPC ROOT-CAUSE HYPOTHESIS REJECTED AT THIS TEST RESOLUTION.**

The current evidence does not support further RPC-specific remediation. The earliest unresolved bottleneck remains the deployed Durable Object command path outside the normal structured application-error contract. Candidate classes include remote coordinator construction/initialization, persistent storage/schema interaction, stub/request transport failure not specific to RPC, or another deployed-only platform/runtime condition. None is yet promoted to FACT.

The run still did not reach Stripe Checkout Session expiry or signed webhook delivery.

## Diagnostic test — PR #34

**TEST — MAKE THE REMOTE COORDINATOR FAILURE OBSERVABLE BEFORE PUBLIC CHECKOUT.**

PR #34 adds three narrow diagnostics without changing commerce semantics:

1. authenticated coordinator transport/initialization exceptions are caught at the outer commerce Worker and returned as bounded JSON `COMMERCE_COORDINATOR_TRANSPORT_ERROR` diagnostics rather than an opaque non-JSON response;
2. `cloudflare-staging` now performs a deployed `/commands` probe immediately after ordinary service smoke, sending a deliberately unsupported command and requiring the normal structured `400 UNSUPPORTED_COMMAND` result; this forces real remote coordinator instantiation/request handling before the public checkout acceptance;
3. `commerce-worker-p6-http.mjs` is added to the `p6-commerce-contract` workflow path triggers, closing the CI coverage gap discovered while diagnosing run #8.

PR #34 validation on tested head `683a46b7f040833fc7aac8251d461f155d6dcb92`:

- bootstrap run `36229762551`: `SUCCESS`;
- P6 commerce-contract run `36229762454`: `SUCCESS`;
- configured local Worker startup: PASS;
- local `/commands` traversal and serialized capacity invariants: PASS;
- full web run `36229762466`: `SUCCESS`;
- type-check/build, commerce tests, browser/accessibility, production runtime-error suite and Lighthouse baseline: PASS.

PR #34 was merged to `main` as `341cf7c1d45350eef22c265258336c6e9e3e09e7`.

PR #34 is an evidence-producing test, not proof that any specific remote coordinator cause has been fixed.

## Current next acceptance step

Launch a **new** `cloudflare-staging` workflow on current `formalife/platform/main` at `341cf7c1d45350eef22c265258336c6e9e3e09e7` or a later main containing PR #34.

The new deployed coordinator probe is now the first discriminating gate:

- expected healthy result: authenticated `/commands` probe returns structured HTTP `400` with `UNSUPPORTED_COMMAND`, proving remote coordinator construction and request handling work;
- if transport/initialization fails: the workflow should stop before checkout and emit the bounded `COMMERCE_COORDINATOR_TRANSPORT_ERROR` message needed to identify the next root-cause candidate;
- only if the coordinator probe passes should the workflow continue to public checkout and then the real Stripe expiry scenario.

If the coordinator probe passes, the intended expiry acceptance remains:

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

Current closed prerequisites / resolved findings:

- public webhook route deployed;
- endpoint-specific Stripe Sandbox signing secret installed;
- raw-body verifier and processor code proven in CI;
- public runtime UUID defect corrected and merged in PR #31;
- GitHub Actions billing/spending runner block resolved;
- deployed public-checkout fetch-receiver defect remediated in merged PR #32 with full CI regression proof;
- deployed commerce-boundary failure localized to the Durable Object command hop;
- Durable Object RPC root-cause hypothesis tested and rejected by run #8;
- deployed coordinator diagnostic gate merged in PR #34;
- P6 HTTP entrypoint now covered by the commerce-contract CI trigger.

Still open at minimum:

- deployed coordinator diagnostic run including PR #34;
- exact root cause of the remote Durable Object command-path failure;
- real signed-expiry acceptance;
- real successful-payment `checkout.session.completed` operational effects;
- duplicate/reordered delivery acceptance under real Stripe delivery;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- full SINGLE and COUPLE end-to-end acceptance.
