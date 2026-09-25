# P6 Stripe Webhook Staging Deployment

Status: CURRENT RESULT — DEPLOYED, SIGNING SECRET NOT YET INSTALLED
Date: 2026-09-25

## Scope

This record captures the first accepted Cloudflare staging deployment that contains the public Formalife Stripe webhook route after `formalife/platform` PR #29 and the optional signing-secret deployment wiring from PR #30.

It does **not** claim real Stripe webhook acceptance yet. The endpoint is intentionally deployed without `STRIPE_WEBHOOK_SECRET`, so signature verification remains fail-closed until Stripe generates an endpoint-specific `whsec_...` signing secret and that secret is installed through the GitHub `staging` environment and a subsequent Cloudflare deploy.

## Implementation lineage

Implementation repository: `formalife/platform`.

- PR #29 — `Add protected Stripe webhook endpoint`;
- PR #29 merge commit: `2ed5c4c303bdb375b7c88879c3ad2cdd026a846b`;
- public route: `POST /api/commerce/stripe/webhook`;
- PR #30 — `Wire optional Stripe webhook secret into staging`;
- PR #30 merge commit / deployed `main`: `4e3419e819a842e135b4bd4049fa8c6aa405a746`.

## Accepted deployment evidence

- workflow: `cloudflare-staging`;
- run: `36174604916`;
- run number: `4`;
- conclusion: `SUCCESS`;
- deployed commit: `4e3419e819a842e135b4bd4049fa8c6aa405a746`;
- evidence artifact: `10880994470`;
- artifact SHA-256: `a46cebb2e095f90fa539f29ae47a296c6be5e5e48436c97446cedac74918eea9`;
- deployed at: `2026-09-25T18:38:20.935Z`.

Staging URLs remain:

- web: `https://formalife-web-staging.formalife-preview.workers.dev`;
- commerce boundary: `https://formalife-commerce-staging.formalife-preview.workers.dev`.

Webhook URL for Stripe Sandbox:

`https://formalife-web-staging.formalife-preview.workers.dev/api/commerce/stripe/webhook`

## What the deployment proves

**RESULT — PUBLIC WEBHOOK CODE SURFACE DEPLOYED.**

The deployed Astro/Cloudflare bundle includes the webhook module and the staging deployment completed successfully with the protected commerce boundary and Twenty/Stripe Sandbox runtime configuration intact.

The route implementation verifies the exact raw request body against `Stripe-Signature` before JSON interpretation, resolves the linked Twenty Order and protected reservation context, derives deterministic payment/enrollment identities for replay stability, and routes paid events through the guarded/idempotent commerce boundary.

`checkout.session.expired` is handled as a non-success capacity release path and cannot cancel an already-PAID Order.

## Current configuration state

**FACT:** deployment evidence reports `stripeWebhookSecretConfigured: false`.

Therefore no real Stripe webhook event can yet be accepted as authentic. This is deliberate and fail-closed.

The workflow now supports an optional GitHub `staging` Environment secret named `STRIPE_WEBHOOK_SECRET`. When present, it must match `whsec_*`; the workflow installs it into the Cloudflare web Worker without writing the value to source control or deployment evidence.

## Next acceptance step

Create one Stripe Sandbox webhook/event destination for the public staging URL and subscribe only to the currently supported event types:

- `checkout.session.completed`;
- `checkout.session.expired`.

Do not subscribe the endpoint to all events. Refund and delayed-payment failure/success acceptance remain separate P6 work because the current deployed normalizer does not yet implement those Stripe event families.

After Stripe creates the endpoint:

1. copy the endpoint-specific signing secret into GitHub Environment `staging` as `STRIPE_WEBHOOK_SECRET`;
2. rerun `cloudflare-staging`;
3. verify deployment evidence reports `stripeWebhookSecretConfigured: true`;
4. send real Stripe Sandbox deliveries and validate raw-body signature verification plus idempotent operational effects.

## P6 status

P6 remains OPEN.

This result closes the "public webhook route deployed" prerequisite only. Real signed delivery, duplicate/reordered acceptance, successful-payment operational effects, expiry behavior under real delivery, refund/reconciliation, Brevo, PostHog, and full Single/Couple end-to-end acceptance remain to be proven.
