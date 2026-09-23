# Formalife Platform Implementation Status

Status: CURRENT
Date: 2026-09-23

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

Current infrastructure observation: OCI Always Free/A1 remains suitable as a zero-cost spike candidate but is not frozen as production hosting merely because it is free; provider reclamation/inactivity risk must be included in the operational gate.

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

**RESULT: REPOSITORY MODEL + VERSIONED SCHEMA COMPLETE; STAGING EXIT GATE OPEN.**

The P5 domain/state contract was frozen through `formalife/platform` PR #13 at squash commit `f38dbea7238ffc34f7e129e02ef0d8c5b4a4d55d`.

The versioned Twenty application schema was then implemented and validated through `formalife/platform` PR #14 at squash commit `6558de84321ddc9c38195945b90e7223e748c10f`.

Current implementation decisions/results:

- Twenty standard `People`, `Companies`, `Tasks` and `Notes` are reused where semantics match;
- no duplicate custom `Lead` or `Partner` object exists;
- one human remains one Person while purchaser, participant and customer roles are represented by relations/state;
- current custom domain objects are `Household`, `CourseEdition`, `Enrollment`, `Order`, `PaymentRecord`, `TrainingCredit`, `Entitlement` and `ConsentRecord`;
- a 2-Caregiver Full order is represented by two Enrollment records;
- transfer preserves audit history by linking a new Enrollment rather than destructively moving the original record;
- edition seat consumption is derived from Enrollment state rather than a manually maintained seat counter;
- Stripe remains payment-event/payment-status truth while Twenty holds the operational mirror and reconciliation identifiers;
- marketing consent history is append-oriented and distinct from transactional/service communication state;
- initial attribution remains deliberately light: Order-level source/UTM/partner/referrer plus first-acquisition summary on Person;
- no child clinical/health profile or speculative sensitive-data object is part of the initial schema;
- unique schema constraints cover edition code, order code, Stripe Checkout Session, Stripe provider object and one Training Credit per origin Order;
- the Formalife Twenty application is pinned to Twenty SDK `2.41.0` and passes frozen dependency install, repository contract, TypeScript validation and `twenty dev:build` manifest generation in read-only CI;
- the shared web regression suite also remained green after the workspace/lockfile change.

Implementation documentation:

- `formalife/platform/docs/operations/twenty-domain-model.md`;
- `formalife/platform/docs/operations/twenty-state-machines.yaml`;
- `formalife/platform/docs/operations/p5-staging-scenarios.md`;
- `formalife/platform/packages/twenty-app/`.

**P5 IS NOT COMPLETE OPERATIONALLY YET.**

The remaining exit gate is to install/sync the versioned app into a real staging Twenty instance and execute the documented scenarios proving, without spreadsheet/manual side truth:

- 1-Caregiver booking;
- 2-Caregiver household booking;
- payment reference/mirror;
- edition capacity;
- transfer/cancellation;
- Training Credit;
- refresh entitlement;
- source/partner/referral attribution;
- consent chronology/current eligibility;
- duplicate/idempotency behavior relevant to the P6 integration boundary.

P6 also retains one explicit architecture proof: do not assume Twenty alone is sufficient for atomic receipt/reordering of Stripe webhook events until the integration test proves it. If it is insufficient, make a separate explicit persistence decision rather than hiding the invariant in manual operations.

## Current next execution block

### P5 staging installation and acceptance proof

The next move is not to add more CRM entities. It is to install the current versioned schema into staging and run the P5 scenario contract against real Twenty behavior.

This operational proof is coupled to the still-open P1 real-host gate. A temporary staging instance may be used if it proves the schema/runtime behavior honestly, but local/static manifest success must not be relabeled as operational completion.

## Open parallel infrastructure gates

The remaining independent external gates are:

1. **P1 real Twenty host proof** — requires access to/provisioning of the selected real VM/host and must prove persistence, off-host backup, restore, monitoring/failure behavior and upgrade rehearsal.
2. **P2 real Cloudflare preview deploy** — requires Cloudflare account credentials/integration capable of creating the preview deployment.

Neither gate may be silently marked complete from local simulation alone.

## Revision condition

Update this status file when a phase exit gate materially changes, a current implementation decision is superseded, or new evidence changes the active queue. Do not use it to rewrite business strategy; business strategy remains in the appropriate Layer 2 business documents.
