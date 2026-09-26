# P6 Stripe Webhook Staging Deployment

Status: CURRENT RESULT — REAL SIGNED STRIPE EXPIRY ACCEPTANCE PASS; SUCCESSFUL-PAYMENT ACCEPTANCE NEXT
Date: 2026-09-26

## Scope

This record captures the Cloudflare staging deployment of the public Formalife Stripe webhook route, the endpoint-specific signing-secret installation, the real signed-expiry acceptance attempts, and the boundary between implementation/configuration proof and real signed-event acceptance.

The real signed `checkout.session.expired` path has now passed on deployed Cloudflare staging with a real Stripe Sandbox Checkout Session linked to a Formalife Order and protected reservation. This does **not** yet prove the successful-payment `checkout.session.completed` path, duplicate/reordered real delivery, refund/reconciliation, Brevo/PostHog delivery or final SINGLE/COUPLE closure.

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
- PR #34 merge commit: `341cf7c1d45350eef22c265258336c6e9e3e09e7`;
- PR #35 — `Probe deployed P6 capacity boundary before Stripe`;
- PR #35 tested head: `f06ab2c78160d1e2cc51a73e8be037b051cdfe16`;
- PR #35 merge commit: `cab92eaff713c9017459447305559a8b3477eb27`;
- PR #36 — `Use Cloudflare service binding for commerce boundary`;
- PR #36 tested head: `964b409f8f716243db6712e5000c982d0754c062`;
- PR #36 merge commit: `4b3fced69b58a9050d93f8e95f30fb6aa896c819`.

PR #31 corrected the default public-checkout persisted IDs from prefixed strings to UUIDs and added a staging acceptance gate that creates a real Stripe Checkout Session, expires it through Stripe, and requires the signed `checkout.session.expired` delivery to cancel the Order and release capacity without creating financial or seat effects.

PR #32 corrected a Cloudflare Worker runtime defect exposed by the first real execution of that acceptance: the raw global `fetch` had been injected into adapter instances which later invoked it as `this.fetchImpl(...)`. In Cloudflare `workerd` this can violate the receiver/brand check and throw a native `TypeError`, while Node-based tests can remain green. The runtime now wraps the injected fetch once as a detached function before passing it to Twenty, commerce-boundary, Stripe and analytics adapters. A receiver-sensitive regression test was added.

PR #33 removed Durable Object RPC from the P6 commerce-boundary request/response hop and routed the same protected command contract through Durable Object `fetch()`. It preserved the Durable Object class name, SQLite storage, command/state logic, capacity rules and secrets. Run #8 later showed that this transport substitution did not resolve the deployed failure, so RPC is no longer the current root-cause hypothesis.

PR #34 added evidence-producing diagnostics rather than asserting another root cause: authenticated coordinator transport/initialization exceptions are converted into a bounded structured diagnostic response, and staging probes the deployed Durable Object command path before attempting public checkout. It also closed a CI coverage gap by adding the P6 HTTP entrypoint to the commerce-contract workflow path triggers.

PR #35 reuses the existing serialized P6 capacity contract as a direct deployed staging probe before Stripe acceptance. The smoke test generates unique UUID-backed identities so it is repeatable against persistent staging storage. This is a diagnostic test, not a commerce-semantic change.

PR #36 fixes the actual deployed web-to-commerce transport defect identified by run #10: the public web Worker was using global HTTP `fetch()` against the commerce Worker's `workers.dev` URL. Cloudflare does not allow same-zone Worker-to-Worker fetches to a `workers.dev`/route target through the public URL path; that platform response is not the commerce Worker's JSON contract and was surfaced by the application as `COMMERCE_BOUNDARY_NON_JSON`. The staging web Worker now receives a native `COMMERCE_BOUNDARY` Service Binding to `formalife-commerce-staging`; `createCommerceRuntime` prefers that binding and retains the URL transport only as local/test fallback. The authenticated boundary token remains in use.

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

The public commerce URL remains useful for external staging probes. Web-to-commerce runtime traffic now uses the Cloudflare Service Binding rather than the public `workers.dev` URL.

## What is proven before signed delivery

**RESULT — PUBLIC WEBHOOK CODE SURFACE AND ENDPOINT-SPECIFIC SIGNING SECRET ARE DEPLOYED.**

The route verifies the exact raw request body against `Stripe-Signature` before JSON interpretation, resolves the linked Twenty Order and protected reservation context, derives deterministic payment/enrollment identities for replay stability, and routes paid events through the guarded/idempotent commerce boundary.

`checkout.session.expired` is designed as a non-success capacity release path and cannot cancel an already-PAID Order.

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

This was initially localized to the protected commerce dependency. Later runs showed that the commerce Worker and Durable Object themselves were healthy when called externally; the remaining transport difference was the web Worker's same-zone `workers.dev` subrequest.

The run still did not reach Stripe Checkout Session expiry or signed webhook delivery.

## Hypothesis and test — PR #33

**HYPOTHESIS — deployed Durable Object RPC is the failing transport layer.**

The outer commerce Worker used Durable Object RPC (`stub.execute()` / `stub.inspect()`) while the same command contract passed locally through Wrangler. Replacing the RPC hop with `stub.fetch()` was a clean discriminating test that did not require changing commerce semantics.

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

- workflow run: `36229376954`;
- run number: `8`;
- deployed commit: `012df300717f1e2299c937dc4d5ba9fa343678cf`;
- job: `108369492920`;
- conclusion: `FAILURE`;
- deploy/smoke: `SUCCESS`;
- signed-expiry acceptance: `FAILURE` before Stripe expiry.

The public checkout again failed with HTTP `502` / `COMMERCE_BOUNDARY_NON_JSON` after RPC was removed.

**RESULT — RPC ROOT-CAUSE HYPOTHESIS REJECTED.**

## Diagnostic test — PR #34

PR #34 added a generic authenticated deployed `/commands` probe and structured transport diagnostics without changing commerce semantics.

Validation on tested head `683a46b7f040833fc7aac8251d461f155d6dcb92`:

- bootstrap run `36229762551`: `SUCCESS`;
- P6 commerce-contract run `36229762454`: `SUCCESS`;
- configured local Worker startup: PASS;
- local `/commands` traversal and serialized capacity invariants: PASS;
- full web run `36229762466`: `SUCCESS`.

PR #34 was merged to `main` as `341cf7c1d45350eef22c265258336c6e9e3e09e7`.

## 2026-09-26 run #9 — generic coordinator path proven healthy

**RESULT — DEPLOYED COORDINATOR CONSTRUCTION AND GENERIC COMMAND HANDLING PASS; CHECKOUT-SPECIFIC FAILURE REMAINS.**

- workflow run: `36230106062`;
- run number: `9`;
- deployed commit: `341cf7c1d45350eef22c265258336c6e9e3e09e7`;
- job: `108371521145`;
- conclusion: `FAILURE`;
- deploy/smoke: `SUCCESS`;
- deployed generic coordinator probe: `SUCCESS`;
- signed-expiry acceptance: `FAILURE` before Stripe expiry.

The authenticated probe returned the expected structured `400 UNSUPPORTED_COMMAND`. This proved that the deployed binding resolves, coordinator construction works, the outer commerce Worker can call the Durable Object, and structured JSON can return through that deployed transport.

## Diagnostic test — PR #35

PR #35 made the existing serialized P6 capacity smoke replay-safe for persistent staging storage and added it before the Stripe acceptance.

Validation on tested head `f06ab2c78160d1e2cc51a73e8be037b051cdfe16`:

- bootstrap run `36230670899`: `SUCCESS`;
- P6 commerce-contract run `36230670869`: `SUCCESS`;
- configured local Worker startup: PASS;
- serialized capacity invariants using unique IDs: PASS.

PR #35 was merged to `main` as `cab92eaff713c9017459447305559a8b3477eb27`.

## 2026-09-26 run #10 — deployed commerce subsystem proven; root cause localized to web-to-commerce same-zone fetch

**RESULT — GENERIC COORDINATOR AND FULL DEPLOYED SERIALIZED CAPACITY PATH PASS; PUBLIC WEB CHECKOUT ALONE FAILS.**

Fresh workflow dispatch:

- workflow: `cloudflare-staging`;
- run: `36230897496`;
- run number: `10`;
- deployed commit: `cab92eaff713c9017459447305559a8b3477eb27`;
- job: `108373731398`;
- conclusion: `FAILURE`;
- build/deploy/secrets/redeploy/service smoke: `SUCCESS`;
- generic coordinator probe: `SUCCESS`;
- deployed serialized capacity path: `SUCCESS`;
- public signed-expiry acceptance: `FAILURE` on public checkout before Stripe expiry.

The deployed capacity probe exercised configuration, reserve, inspect, overflow rejection, replay/conflict, release, TTL expiry and fail-closed payment behavior through the real external commerce Worker URL. Every check passed, including SQLite-backed Durable Object state and protected participant context.

Immediately afterward, the public web Worker still returned `502 COMMERCE_BOUNDARY_NON_JSON` while trying to use the same commerce subsystem.

This comparison isolates the relevant difference: GitHub runner → commerce Worker public URL succeeds, while web Worker → commerce Worker `workers.dev` URL fails. Cloudflare's documented routing model explains this exact split: same-zone Worker-to-Worker fetches targeting a `workers.dev`/Worker route are not supported through ordinary global `fetch()` and must use a Service Binding (or a different explicitly supported routing mode). The non-JSON payload is the Cloudflare platform error response, not the commerce application's JSON contract.

**RESULT — ROOT CAUSE IDENTIFIED: INVALID SAME-ZONE PUBLIC WORKER-TO-WORKER FETCH ARCHITECTURE.**

Stripe still was not reached in run #10.

## Root-cause remediation — PR #36

**REMEDIATION — USE A CLOUDFLARE SERVICE BINDING FOR WEB → COMMERCE TRAFFIC.**

PR #36:

1. makes `createCommerceRuntime` prefer `env.COMMERCE_BOUNDARY.fetch()` when the native service binding is available;
2. sends the existing authenticated commerce request through that binding using an internal synthetic URL, preserving the boundary token and HTTP contract;
3. retains `COMMERCE_BOUNDARY_URL` only as local/test fallback;
4. configures the generated staging Astro Wrangler deployment with `COMMERCE_BOUNDARY -> formalife-commerce-staging` before web deploy;
5. fails the staging deploy if Wrangler does not report the service binding;
6. adds a regression test proving that commerce calls use the Service Binding rather than public fetch when the binding exists;
7. records `commerceServiceBindingConfigured` in successful deployment evidence.

PR #36 validation on tested head `964b409f8f716243db6712e5000c982d0754c062`:

- bootstrap run `36231178534`: `SUCCESS`;
- P6 commerce-contract run `36231178545`: `SUCCESS`;
- Stripe Sandbox run `36231178550`: `SUCCESS`;
- full web run `36231178552`: `SUCCESS`;
- runtime Service Binding regression: PASS;
- Astro type-check/build, direct Full commerce tests, browser/accessibility, production runtime-error suite and Lighthouse baseline: PASS.

PR #36 was merged to `main` as `4b3fced69b58a9050d93f8e95f30fb6aa896c819`.

## 2026-09-26 run #11 — real signed Stripe expiry acceptance PASS

**RESULT — REAL DEPLOYED `checkout.session.expired` PATH PASSED END TO END.**

Fresh workflow dispatch on the PR #36 merge commit:

- workflow: `cloudflare-staging`;
- run: `36231789069`;
- run number: `11`;
- attempt: `1`;
- deployed commit: `4b3fced69b58a9050d93f8e95f30fb6aa896c819`;
- job: `108376201500`;
- conclusion: `SUCCESS`;
- Cloudflare service binding configured and reported by Wrangler: `env.COMMERCE_BOUNDARY (formalife-commerce-staging) Worker`;
- generic coordinator probe: PASS;
- deployed serialized-capacity probe: PASS;
- signed-expiry acceptance: PASS;
- deployment evidence artifact: `10903060489`;
- artifact digest: `sha256:1b21ec4858163ccdabe276c93d1d0d24e1583a3efb9c472c0a65b556409e6ff4`.

Acceptance evidence from the run:

- scenario prefix: `P6EXP-36231789069-1`;
- Stripe mode: sandbox;
- synthetic confirmed edition: `cb13853d-6161-4f99-8d8a-c9dd7a307cb6`;
- real Stripe Checkout Session: `cs_test_a1gvZ6u6LlOhsLyBEIA6L5sXe7hwiyrv0jrzxNnqsiTZWProMCvhbjPwxN`;
- Twenty Order code: `WEB-20260926090744-2B38248B`;
- Twenty Order id: `c2694c63-c5a6-480c-b6e3-31862b38248b`;
- before expiry: one active protected reservation, available seats `11`;
- Stripe API expiry confirmed: `stripeSessionExpired: true`;
- genuine signed `checkout.session.expired` delivery was accepted by the public Formalife webhook under the configured endpoint signing secret;
- Order state after processing: `CANCELLED`;
- after processing: active reserved seats `0`, available seats restored to `12`;
- PaymentRecord count: `0`;
- Enrollment count: `0`;
- public checkout state: `CANCELLED`, `verified: true`;
- acceptance completed at `2026-09-26T09:07:48.489Z`.

This is the first real external-delivery proof of the deployed Stripe webhook operational path. The earlier same-zone transport defect is therefore not only diagnosed and CI-remediated; the Service Binding fix is proven on Cloudflare staging by the real commerce flow.

**RESULT — SIGNED EXPIRY GATE CLOSED / PASS.**

## Current next acceptance step

The first remaining money-path bottleneck is the real successful-payment path. Implement and execute a repeatable Stripe Sandbox acceptance that starts from the same public Formalife checkout and proves a genuine successful payment / `checkout.session.completed` delivery linked to the real Order and protected reservation.

The success gate must prove at minimum:

1. public Formalife checkout creates a real Stripe Sandbox Checkout Session for the intended caregiver option;
2. the Session is actually paid in Stripe Sandbox rather than simulated by a generic detached test event;
3. the genuine signed `checkout.session.completed` event reaches the deployed webhook and raw-body signature verification passes;
4. the event resolves the correct Twenty Order and protected reservation;
5. expected amount and currency match before financial/seat mutation;
6. replay/idempotency identity is stable;
7. reservation is consumed rather than released;
8. Order becomes `PAID`;
9. exactly one PaymentRecord is mirrored;
10. SINGLE creates exactly one Enrollment and COUPLE exactly two;
11. capacity reflects the consumed seat count correctly;
12. public server-side verified checkout state is `PAID` and browser redirect/session parameters are not treated as truth.

The acceptance must remain repeatable/automatable in sandbox and must not commit payment credentials or card data to the repository.

After successful-payment proof, continue with duplicate/reordered real delivery, final SINGLE/COUPLE acceptance, refund/reconciliation, Brevo transactional delivery and privacy-safe PostHog delivery.

## P6 status

P6 remains **OPEN**.

Current closed prerequisites / resolved findings:

- public webhook route deployed;
- endpoint-specific Stripe Sandbox signing secret installed;
- raw-body verifier and processor code proven in CI;
- public runtime UUID defect corrected and merged in PR #31;
- GitHub Actions billing/spending runner block resolved;
- deployed public-checkout fetch-receiver defect remediated in PR #32;
- Durable Object RPC root-cause hypothesis tested and rejected by run #8;
- generic deployed coordinator construction/request handling proven by run #9;
- complete deployed serialized-capacity subsystem proven by run #10;
- web-to-commerce same-zone public `workers.dev` fetch identified as the actual `COMMERCE_BOUNDARY_NON_JSON` root cause;
- Cloudflare Service Binding remediation merged in PR #36 with full CI regression proof;
- Service Binding remediation proven on deployed Cloudflare staging;
- real Stripe Sandbox `checkout.session.expired` delivery and full cancellation/release/no-financial-effects path proven end to end in run #11.

Still open at minimum:

- real successful-payment `checkout.session.completed` operational effects;
- duplicate/reordered delivery acceptance under real Stripe delivery;
- final SINGLE and COUPLE end-to-end acceptance;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery.