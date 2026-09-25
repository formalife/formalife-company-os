# P6 Stripe Webhook Staging Deployment

Status: CURRENT RESULT — DEPLOYED, SIGNING SECRET INSTALLED; REAL SIGNED DELIVERY ACCEPTANCE PENDING
Date: 2026-09-25

## Scope

This record captures the Cloudflare staging deployment of the public Formalife Stripe webhook route, the endpoint-specific signing-secret installation, and the boundary between configuration proof and real signed-event acceptance.

It does **not** yet claim that a real Stripe webhook delivery has completed the Formalife operational path. That requires an actual Stripe Sandbox event linked to a real Formalife checkout/reservation.

## Implementation lineage

Implementation repository: `formalife/platform`.

- PR #29 — `Add protected Stripe webhook endpoint`;
- PR #29 merge commit: `2ed5c4c303bdb375b7c88879c3ad2cdd026a846b`;
- public route: `POST /api/commerce/stripe/webhook`;
- PR #30 — `Wire optional Stripe webhook secret into staging`;
- PR #30 merge commit: `4e3419e819a842e135b4bd4049fa8c6aa405a746`;
- PR #31 — `Fix runtime UUIDs and add signed Stripe expiry acceptance`;
- PR #31 merge commit: `9da8497502b672e5b07005376a1d2d161557302f`.

PR #31 corrected the default public-checkout persisted IDs from prefixed strings to UUIDs and added a manual staging acceptance gate that creates a real Stripe Checkout Session, expires it through Stripe, and requires the signed `checkout.session.expired` delivery to cancel the Order and release capacity without creating financial or seat effects.

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

The deployment logs explicitly show Wrangler successfully installing `STRIPE_WEBHOOK_SECRET` into `formalife-web-staging`; the secret value is not committed or emitted in evidence.

## Staging endpoints

- web: `https://formalife-web-staging.formalife-preview.workers.dev`;
- commerce boundary: `https://formalife-commerce-staging.formalife-preview.workers.dev`;
- Stripe webhook: `https://formalife-web-staging.formalife-preview.workers.dev/api/commerce/stripe/webhook`.

## What is now proven

**RESULT — PUBLIC WEBHOOK CODE SURFACE AND ENDPOINT-SPECIFIC SIGNING SECRET ARE DEPLOYED.**

The route verifies the exact raw request body against `Stripe-Signature` before JSON interpretation, resolves the linked Twenty Order and protected reservation context, derives deterministic payment/enrollment identities for replay stability, and routes paid events through the guarded/idempotent commerce boundary.

`checkout.session.expired` is designed as a non-success capacity release path and cannot cancel an already-PAID Order.

The presence of the signing secret proves configuration, not delivery. A generic Stripe Dashboard test event is not sufficient because the processor correctly requires a Checkout Session already linked to a real Formalife Order and protected reservation.

## Next acceptance step

Deploy PR #31 through `cloudflare-staging` on `main`. Because `STRIPE_WEBHOOK_SECRET` is present, that workflow must automatically execute the real expiry acceptance scenario:

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

P6 remains OPEN.

Current closed prerequisites:

- public webhook route deployed;
- endpoint-specific Stripe Sandbox signing secret installed;
- raw-body verifier and processor code proven in CI;
- public runtime UUID defect corrected and merged in PR #31.

Still open at minimum:

- real signed expiry delivery acceptance on PR #31 deployment;
- real successful-payment `checkout.session.completed` operational effects;
- duplicate/reordered delivery acceptance under real Stripe delivery;
- refund/reconciliation;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- full SINGLE and COUPLE end-to-end acceptance.
