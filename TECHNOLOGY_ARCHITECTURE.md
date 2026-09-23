# Formalife Technology Architecture

Status: CURRENT DECISION BASELINE
Date: 2026-09-23

Purpose: record the founder-approved initial technology architecture for Formalife while preserving clear boundaries between business truth, implementation code and operational systems.

## 1. Governing principle

Use the smallest integrated stack that can support the current Formalife commercial architecture without creating unnecessary duplicate systems.

Technology choices are implementation decisions, not substitutes for positioning, offer, demand, economics or operating rules.

## 2. Founder-approved stack

### Public website

- **Astro** — public site framework.
- **Tailwind CSS v4** — styling system.
- **Cloudflare** — DNS/CDN/edge hosting for the public site.
- **Cloudflare R2** — public/static object storage where needed.

### Commerce / communications / analytics

- **Stripe Checkout** — payment checkout layer.
- **Brevo** — email/lifecycle execution layer.
- **PostHog** — product/web analytics, conditional on remaining economically appropriate at the required usage level; start on free tier where available.

### CRM / internal operating system

- **Twenty** — selected CRM / internal operating-system platform.
- Preferred initial direction: **self-host Twenty rather than use Twenty Cloud**, provided production-grade hosting, backup, restore, monitoring and upgrade safety can be achieved at acceptable cost/risk.
- Start with the free/open-source self-hosted edition where its capabilities are sufficient.
- Commercial Twenty features remain optional and must be purchased only if a concrete requirement justifies them (for example advanced row-level permissions or other commercially licensed capabilities).
- Twenty is not the canonical source of company strategy. Layer 2 remains canonical for company decisions/current truth; Twenty is operational truth for CRM/relationship/process records.

## 3. UI / design-system decision

Selected direction for the public site:

- **shadcn/ui** as owned/open-code component scaffolding;
- **Base UI** as the default primitive layer for new interactive components;
- **React only as selective Astro islands**, not as the site-wide rendering model;
- **Tabler Icons for Astro** as the default icon system;
- motion hierarchy: **CSS first -> Astro-native transitions where useful -> Motion for selective higher-quality interaction**;
- GSAP is not a baseline dependency and may be introduced only for a specific interaction whose visual value justifies the extra complexity/weight.

Rationale:

- Base UI and Radix are both unstyled/headless primitive layers, so visual quality is determined primarily by Formalife's own tokens, typography, spacing, geometry and component styling rather than by the primitive implementation.
- shadcn supports both and intentionally keeps visual output equivalent across bases; therefore aesthetics are not a reason to prefer Radix.
- For a new project, Base UI is the current shadcn default and recommended starting point, while Radix remains a mature fallback if a specific component or compatibility issue warrants it.
- The design system must avoid generic/template appearance. shadcn defaults are starting code, not the Formalife visual identity.

## 4. Design-system implementation rules

The Formalife site should define and own semantic design tokens for at least:

- color roles;
- typography;
- spacing;
- radii;
- elevation/shadows;
- content widths;
- motion duration/easing;
- focus states;
- component states.

Most marketing/editorial sections should remain native Astro/HTML/CSS with zero client JavaScript. React/shadcn/Base UI should be used only where genuine interaction requires it.

## 5. Quality gates

Public-site implementation should include automated quality gates before production release:

- browser/integration tests with Playwright;
- automated accessibility checks using axe-core where appropriate;
- Lighthouse CI or equivalent performance/accessibility/SEO regression checks;
- explicit reduced-motion support;
- image optimization and layout-stability checks.

## 6. Open infrastructure decision — Twenty hosting

Self-hosting Twenty is the current preferred direction, but the final infrastructure provider is not frozen yet.

Requirements before production use:

- sufficient CPU/RAM for Twenty server, worker, PostgreSQL and Redis;
- persistent storage;
- automatic off-host database backups;
- tested restore procedure;
- HTTPS and controlled ingress;
- uptime/health monitoring;
- pinned Twenty release rather than unreviewed `latest` updates;
- staging/upgrade procedure before production migration;
- rollback/recovery plan.

A zero-cost host is acceptable only if it meets these operational requirements. Free infrastructure is not treated as a constraint if it creates unacceptable outage, data-loss or maintenance risk.

## 7. Deferred / conditional components

Do not introduce these unless a measured requirement appears:

- Supabase / Neon as second operational database;
- Payload or another CMS;
- Activepieces;
- Trigger.dev;
- Metabase;
- GSAP;
- a custom Formalife OS frontend outside Twenty;
- customer portal/auth infrastructure.

The default rule is to exhaust the approved core stack before adding another system.

## 8. Systems-of-record boundary

- Layer 1 `formalife/merenda-business-core` — doctrine / how to reason.
- Layer 2 `formalife/formalife-company-os` — company decisions and current business truth.
- Future implementation repository — source of truth for code, schemas, infrastructure-as-code and tests.
- Twenty — CRM / operational relationship and process records.
- Stripe — payment-event/payment-status truth.
- Brevo — communication execution.
- PostHog — behavioral/product analytics.
- Google Drive — human workspace/source assets where appropriate.
- Cloudflare/R2 — public delivery/infrastructure assets.

## 9. Revision condition

Reopen a technology decision only when a concrete requirement, measured limitation, security/privacy need, cost threshold or operational failure shows that the current component is no longer sufficient.

## 10. Canonical implementation roadmap

Execution order, phase gates, Release 1 definition of done and the current immediate queue are canonical in:

`PLATFORM_IMPLEMENTATION_ROADMAP.md`

`FORMALIFE_BUILD_SEQUENCE.md` continues to govern business construction order. The implementation roadmap must support that sequence rather than supersede it.