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

**RESULT: COMPLETE, WITH ONE OPEN NON-REPRODUCED RUNTIME OBSERVATION.**

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

Validation evidence on the P3 final PR head:

- repository contract and frozen-lockfile install passed;
- Astro/TypeScript check passed;
- Cloudflare production build passed;
- 5/5 Playwright/axe/runtime tests passed in development;
- 5/5 passed against production/minified Wrangler preview;
- keyboard modal/focus-return and reduced-motion behavior are explicitly tested;
- Lighthouse thresholds pass on both `/` and `/design/full` against the production preview;
- React runtime errors (`pageerror` / `console.error`) are CI-failing conditions.

#### OBSERVATION — React #185 reported after P3 merge

A user/founder-observed minified React error `#185` was reported after the P3 merge. React defines this error as a maximum update-depth loop.

Current evidence does **not** establish which Formalife route/action/environment produced it, and it has not been reproduced by the repository test suite.

Regression response:

- `formalife/platform` issue #9 remains open;
- PR #10 expanded the existing E2E/runtime-clean gate to Chromium, Firefox and WebKit;
- 15/15 tests passed in development across the three engines;
- 15/15 tests passed against the production/minified Wrangler preview across the three engines;
- Lighthouse remained green;
- the cross-browser gate was merged to `main` at `a37f7212ef996eab634ed88a08a3357939785226`.

Classification:

- **OBSERVATION:** a real minified `#185` message was reported;
- **RESULT:** current covered routes/actions do not reproduce it across Chromium/Firefox/WebKit;
- **OPEN QUESTION:** exact route + action + browser/environment that triggered the observed error;
- **OPERATING RULE:** do not claim the error is fixed and do not change component logic speculatively. Add a targeted reproducing test when the triggering context is known.

Implementation documentation: `formalife/platform/docs/design/formalife-visual-system.md`.

## Current next phase

### P4 — Information architecture, page map and content model

**STATUS: NEXT ACTIVE IMPLEMENTATION BLOCK, SUBJECT TO THE REACT #185 REGRESSION RULE ABOVE.**

P4 should define the public site as a commercial information system before page count expands.

The work must start from current Formalife commercial/funnel truth and assign each initial route:

- audience/state;
- commercial/search job;
- primary CTA;
- proof requirement;
- source/content owner;
- measurable outcome;
- routing/fallback.

It must also define the lightest code-first content model and the WordPress URL/redirect inventory process without introducing a CMS prematurely.

P4 architecture/documentation work may proceed because it does not depend on unresolved React component behavior. New interactive customer-facing React surfaces should not broaden until the #185 trigger is understood or the new surface has its own runtime-clean coverage.

## Open parallel infrastructure gates

P4 may proceed while these independent gates remain open, because neither changes the information architecture decision:

1. **P1 real Twenty host proof** — requires access to/provisioning of the selected real VM/host.
2. **P2 real Cloudflare preview deploy** — requires Cloudflare account credentials/integration capable of creating the preview deployment.

Neither gate may be silently marked complete from local simulation alone.

## Revision condition

Update this status file when a phase exit gate materially changes, a current implementation decision is superseded, or new evidence changes the active queue. Do not use it to rewrite business strategy; business strategy remains in the appropriate Layer 2 business documents.
