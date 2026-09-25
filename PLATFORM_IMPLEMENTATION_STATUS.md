# Formalife Platform Implementation Status

Status: CURRENT
Date: 2026-09-25

Purpose: record execution reality against `PLATFORM_IMPLEMENTATION_ROADMAP.md` without rewriting the roadmap itself after every implementation step.

## Canonical implementation repository

**FACT / CURRENT:** the implementation repository is `formalife/platform`.

The earlier roadmap placeholder `formalife/formalife-platform` is superseded by the founder-selected repository name `formalife/platform`.

This repository is the implementation source of truth for application code, infrastructure configuration, implementation ADRs and automated tests. It is not the source of truth for Formalife business decisions; Layer 2 remains canonical for those.

## Current phase status

### P0 — Program bootstrap and guardrails

**RESULT: COMPLETE.**

Evidence:

- implementation repository initialized;
- governance / `AGENTS.md` / architecture / ADR / environment boundaries present;
- Node and pnpm pinned;
- branch/PR workflow established;
- baseline CI and repository contract operational.

Merged implementation evidence: `formalife/platform` PR #1, merge lineage beginning at `958814f`.

### P1 — Twenty self-hosting production-readiness spike

**RESULT: REPOSITORY PREPARATION COMPLETE; OPERATIONAL EXIT GATE OPEN.**

Prepared in `formalife/platform`:

- pinned Twenty self-hosting configuration;
- PostgreSQL / Redis / ingress boundary;
- backup and restore scripts toward R2;
- health-check and runbook material;
- static CI validation including architecture/image compatibility checks.

The production-readiness gate is **not** passed until a real host/VM proves persistence, off-host backup, restore into a separate environment, forced-failure monitoring and upgrade rehearsal.

Current infrastructure evidence:

- Railway Hobby was tested as disposable external staging and rejected for reliable Twenty staging because its observed 1 GB per-replica memory limit caused a real first-boot Node/V8 heap OOM; this was classified as a host/infrastructure capacity failure, not a Formalife schema failure;
- OCI Always Free/A1 remains a zero-cost self-hosting spike candidate in principle, but provisioning friction/availability has not produced an accepted host and it is not frozen as production hosting;
- a Twenty Cloud trial workspace is the proven P5 staging target. This removes P1 self-hosting from the critical path for implementation testing, but it does **not** by itself decide Formalife production hosting.

### P2 — Public-site technical foundation

**RESULT: FOUNDATION COMPLETE; CLOUDFLARE PREVIEW EXIT GATE OPEN.**

Implemented:

- Astro + strict TypeScript;
- Tailwind CSS v4;
- selective React islands;
- Base UI integration path;
- Tabler icon packages;
- Cloudflare adapter / Wrangler production build;
- frozen lockfile and reproducible install;
- Playwright, axe/runtime-error and Lighthouse CI.

The remaining P2 exit condition is a **real GitHub-driven Cloudflare preview deployment** with actual Cloudflare credentials/environment wiring. Local/CI Wrangler production preview does not substitute for this external deployment proof.

Merged foundation evidence: `formalife/platform` PR #4, merge commit lineage including `72ebda2`.

### P3 — Formalife visual language and design system

**RESULT: COMPLETE.**

Merged through `formalife/platform` PR #7 at squash commit `16d74717079721561c9258fd70148a6a812b9e3d`.

Current visual-system decisions implemented and validated:

- Figtree Variable for interface/body and Source Serif 4 Variable for selective editorial/display use;
- warm ivory canvas, deep blue-black ink, restrained deep-teal brand/action color and controlled coral accent;
- semantic typography, spacing, container, radius, elevation, focus, state, motion and z-index/layer tokens;
- shadcn `base-nova` only as owned-code scaffold;
- Base UI as default interactive primitive layer;
- Tabler as baseline icon family;
- Astro/native marketing composition with React limited to genuine interactive islands;
- CSS-first motion with explicit reduced-motion handling;
- reusable Full commercial patterns and owned controls.

Validation evidence:

- repository contract and frozen-lockfile install passed;
- Astro/TypeScript check passed;
- Cloudflare production build passed;
- runtime/accessibility/browser checks pass in development and against the production/minified Wrangler preview;
- keyboard modal/focus-return and reduced-motion behavior are explicitly tested;
- Lighthouse thresholds pass on both `/` and `/design/full` against the production preview;
- React runtime errors (`pageerror` / `console.error`) are CI-failing conditions.

After P3, the browser gate was further expanded through `formalife/platform` PR #10 to Chromium, Firefox and WebKit. The suite passed 15/15 checks in development and 15/15 against the production/minified Wrangler preview across the three engines; Lighthouse remained green. Merge: `a37f7212ef996eab634ed88a08a3357939785226`.

**PROVENANCE CORRECTION:** a React `#185` message seen during the ChatGPT working session was initially misclassified as a founder/user observation from the Formalife application. The founder clarified that ChatGPT itself emitted the error. Therefore it is **not Formalife product evidence**, does not represent an open Formalife runtime defect, and no component fix is required from that report. `formalife/platform` issue #9 is closed as `not_planned` with the corrected provenance.

Implementation documentation: `formalife/platform/docs/design/formalife-visual-system.md`.

### P4 — Information architecture, page map and content model

**RESULT: COMPLETE.**

Merged through `formalife/platform` PR #11 at squash commit `67977191b61047912d3aa040da9c76e35a0824a4`.

The frozen P4 implementation architecture now defines:

- purpose-specific route contracts for `/`, choking education, Full, Guide, editions, experts, resources, partner and legal/policy surfaces;
- explicit audience/state, job, CTA, proof requirement, owner, measurable outcome and fallback for every initial route;
- direct high-intent routing to Full/edition decision surfaces without forcing Guide-first behavior;
- the existing public Guide path `/guida-antipanico-soffocamento/` retained by default unless first-party SEO/backlink evidence justifies a later canonical-path change;
- future Digital/Hybrid/BLSD/lifecycle destinations kept dormant until the corresponding product/job exists;
- code-first editorial boundaries for resources, experts, proof and reusable FAQ data;
- explicit separation between editorial content and operational truth such as edition availability, payment state and customer records;
- versioned scientific-review metadata for medical/scientific publishing;
- evidence-based WordPress migration dispositions and redirect policy, with no homepage catch-all redirects;
- current legal/privacy paths preserved while final content remains contingent on the actual Release-1 processing/commerce stack and appropriate review.

Implementation documentation:

- `formalife/platform/docs/web/information-architecture.md`;
- `formalife/platform/docs/web/route-contracts.yaml`;
- `formalife/platform/docs/web/wordpress-migration-inventory.md`.

P4 does not authorize a CMS or broad future-product page families. Runtime content schemas and route implementation may now be built only against the frozen route/source-of-truth contract.

### P5 — Twenty operational data model

**RESULT: COMPLETE — REAL TWENTY CLOUD ACCEPTANCE AND GUARDED COMMERCE INVARIANTS PROVEN.**

The domain/state contract was frozen through `formalife/platform` PR #13 (`f38dbea7238ffc34f7e129e02ef0d8c5b4a4d55d`) and the versioned Twenty application schema through PR #14 (`6558de84321ddc9c38195945b90e7223e748c10f`). Repository/runtime proof was extended through PR #16 and Twenty Cloud 2.42 compatibility through PR #17 (`7d4fa5ff895475b2eed0cb7db053f2ec2b1f898b`).

Current model/result:

- Twenty standard `People`, `Companies`, `Tasks` and `Notes` are reused where semantics match;
- custom objects are `Household`, `CourseEdition`, `Enrollment`, `Order`, `PaymentRecord`, `TrainingCredit`, `Entitlement` and `ConsentRecord`;
- one human remains one Person while purchaser/participant/customer roles are represented by relations and state;
- two-caregiver Full purchase creates two independent Enrollment seat records;
- transfer preserves history through linked Enrollments rather than destructive edition reassignment;
- capacity is derived from Enrollment state, not a manually typed seat counter;
- Stripe remains payment truth; Twenty remains the operational CRM mirror;
- consent history is append-oriented; attribution keeps Order-level source/UTM/partner/referrer plus Person first-acquisition summary;
- no child clinical/health profile is part of the initial CRM model;
- defense-in-depth UNIQUE constraints include edition code, order code, Stripe Checkout Session/provider identity, one Training Credit per origin Order and one refresh Entitlement per origin Enrollment.

Real Twenty Cloud evidence, 2026-09-25:

- staging workspace reports Twenty `v2.42.7`;
- run `36127598842` proved authentication, frozen install/build, real additive schema apply and post-apply zero drift;
- full S1-S11 acceptance run `36131378103` executed with synthetic staging-only data; evidence artifact `10861549369`, SHA-256 `05580151bf4d191f71e85bcad021091e621699c924df2eaab1f6ca91dfeb2141`;
- that first run correctly exposed S4 as a transaction-boundary gap and S7/S8 as unsupported direct-mutation invariant failures rather than silently weakening the test;
- the remediation implemented a minimal SQLite-backed Cloudflare Durable Object commerce coordinator for idempotency, protected state-machine transitions and durable outbox/reconciliation, while keeping Twenty as CRM mirror;
- direct Twenty staging proof confirms the new Entitlement-origin UNIQUE constraint rejects a duplicate write;
- focused real-staging remediation run `36135292597` passed S4, S7 and S8 against Twenty Cloud; artifact `10863916308`, SHA-256 `3a0ac63b641318785e7e91cb5676258804d1615e539325f8e8c5d6d1477d638a`.

Remediation results:

- **S4 PASS** — a deliberately injected downstream `PAYMENT_CREATE` failure left durable pending work; replay converged to `MIRRORED`; the payment effect required two attempts; repeated provider identity deduplicated; conflicting payload under the same idempotency key returned `IDEMPOTENCY_CONFLICT`; final operational state was exactly one PaymentRecord, one Enrollment and Order `PAID`;
- **S7 PASS** — duplicate Training Credit issuance was blocked; day-30 redemption used the EUR 29.90 Momentum value; second redemption was rejected; operational reactivation is not exposed by the supported command surface; explicit reversal produced `REVERSED` with an audit reason mirrored into Twenty;
- **S8 PASS** — duplicate Entitlement issuance was blocked by the coordinator and independently by the Twenty UNIQUE constraint; first redemption persisted; second redemption was rejected and did not repoint the Twenty mirror.

Implementation was merged through `formalife/platform` PR #18 using merge commit `e18214f4d310e6b15bc811f8271370ae3bbbd5ca`, preserving the exact tested head `580edad04ea42d5c4446cf450a5b2ef1b9f8a38a` in `main` history. PR regression checks were green: bootstrap, full web/browser/accessibility/runtime/Lighthouse suite, and isolated Twenty schema apply/idempotence.

**P5 exit gate is satisfied for the supported operational path.**

Important boundary: the staging proof used an administrative Twenty API key to observe/mirror records. Production must still keep direct protected Twenty mutation credentials behind the commerce boundary rather than expose them as an ordinary application/user mutation path. That is a P6 integration/security prerequisite, not evidence that P5 failed.

Implementation documentation/evidence:

- `formalife/platform/docs/operations/twenty-domain-model.md`;
- `formalife/platform/docs/operations/twenty-state-machines.yaml`;
- `formalife/platform/docs/operations/p5-staging-scenarios.md`;
- `formalife/platform/packages/twenty-app/`;
- Layer 2 architecture decision: `PLATFORM_COMMERCE_TRANSACTION_BOUNDARY.md`;
- `formalife/platform` issue #12 / PR #18.

## Current next execution block

### P6 — First end-to-end commercial vertical slice: direct Full purchase

P5 no longer blocks integration. The next sequential build target is the smallest revenue-capable direct Full path from the current roadmap.

Required sequence:

1. expose a confirmed Full edition and verified availability to the purchase flow;
2. implement server-side Stripe Checkout Session creation for 1-Caregiver and 2-Caregiver options;
3. verify Stripe webhook signatures and normalize duplicate/reordered events into the guarded commerce transaction boundary;
4. drive Twenty Order / PaymentRecord / Household / Enrollment mirror effects through that boundary rather than arbitrary protected-field edits;
5. send the correct transactional confirmation through Brevo;
6. record privacy-safe PostHog funnel events and preserve intended source/UTM context;
7. make the success page depend on verified server/payment state rather than the browser redirect alone;
8. prove successful 1- and 2-Caregiver purchases, duplicate webhook idempotency, failed-payment zero-seat behavior, refund/reconciliation understanding, correct Twenty records and correct communications.

Do not broaden into Guide/Training Credit public purchase flows, activation/pre-enrolment exceptions or unrelated site completeness before this direct Full vertical slice passes its exit gate.

## Open parallel infrastructure gates

The remaining independent external gates are:

1. **P1 production-hosting decision/proof** — self-hosting preparation exists but no real self-host has passed the gate. If Twenty Cloud is later approved as Formalife production hosting, revise/supersede the self-hosting gate explicitly rather than pretending it passed.
2. **P2 real Cloudflare preview deploy** — requires Cloudflare account credentials/integration capable of creating the preview deployment.

Neither gate may be silently marked complete from local simulation alone.

## Revision condition

Update this status file when a phase exit gate materially changes, a current implementation decision is superseded, or new evidence changes the active queue. Do not use it to rewrite business strategy; business strategy remains in the appropriate Layer 2 business documents.
