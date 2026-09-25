# P2 / P6 Cloudflare Staging Proof

Status: CURRENT RESULT
Date: 2026-09-25

## Scope

This record captures the first accepted real GitHub-driven Cloudflare staging deployment for the Formalife public web application and commerce boundary.

It supersedes the earlier Layer 2 statement that the P2 Cloudflare preview exit gate was still open. It does **not** claim production readiness or P6 end-to-end payment acceptance.

## Evidence

Implementation repository: `formalife/platform`

Accepted deployment:

- workflow: `cloudflare-staging`;
- run: `36168015326`;
- commit: `7c7a7e29236e3e2acf2088a20713cd75ec61c1c4`;
- conclusion: SUCCESS;
- evidence artifact: `10879116130`;
- artifact SHA-256: `f1e5c897cbd295b1b723a400d5d31d73b4b9ca169eabd926687eca3a8a1d8860`;
- deployed at: `2026-09-25T17:35:45.585Z`.

Deployed staging URLs:

- commerce boundary: `https://formalife-commerce-staging.formalife-preview.workers.dev`;
- web application: `https://formalife-web-staging.formalife-preview.workers.dev`.

## What the run proved

**RESULT — P2 REAL CLOUDFLARE STAGING DEPLOYMENT PROVEN.**

The GitHub Actions workflow successfully:

- validated Cloudflare, Twenty and Stripe staging configuration;
- built the Astro application;
- validated the Astro-generated deployable Wrangler configuration;
- deployed `formalife-commerce-staging`;
- deployed the `FormalifeCommerceCoordinator` Durable Object;
- kept `ALLOW_TEST_COMMANDS=false`;
- kept `REQUIRE_CAPACITY_RESERVATION=true`;
- installed the protected boundary/Twenty runtime secrets;
- deployed `formalife-web-staging` from the generated Astro/Cloudflare artifact;
- provisioned the required SESSION KV namespace;
- installed the web runtime secrets, including Twenty staging, Stripe Sandbox and commerce-boundary configuration;
- redeployed the web Worker with runtime configuration;
- passed public smoke checks against both the commerce `/health` endpoint and the web home page.

Therefore the real GitHub-driven Cloudflare staging/preview condition for P2 is satisfied.

## P6 consequence

**RESULT — REAL EXTERNAL STAGING SURFACE NOW EXISTS; P6 REMAINS OPEN.**

The direct-Full P6 stack now has a real externally deployed web application and protected commerce boundary. This satisfies the previously open Cloudflare deployment prerequisite for the next Stripe webhook acceptance step.

P6 is still not production/revenue accepted. At minimum, remaining external acceptance includes:

- deployed Stripe Sandbox webhook endpoint and endpoint-specific signing secret;
- real Stripe webhook delivery and raw-body signature verification;
- duplicate/reordered event handling under real delivery;
- successful-payment, failed/expired-payment and refund/reconciliation acceptance;
- final Twenty paid Order / PaymentRecord / Enrollment effects;
- real Brevo transactional delivery;
- real privacy-safe PostHog delivery;
- end-to-end Single and Couple acceptance.

## Provenance / failed attempts preserved

Two prior deployment attempts remain useful evidence rather than being erased:

1. run `36166112085` failed because Wrangler required mandatory commerce secrets on first Worker creation; PR #27 fixed bootstrap ordering with `--secrets-file`;
2. run `36166915299` successfully deployed the commerce Worker but failed the Astro web deploy because the source Wrangler config was used instead of the Astro-generated post-build config; PR #28 fixed this and added a permanent CI assertion for `apps/web/dist/server/wrangler.json`.

The accepted run `36168015326` passed after both defects were corrected.
