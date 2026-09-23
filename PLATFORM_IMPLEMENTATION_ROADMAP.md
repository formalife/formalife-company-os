# Formalife Platform Implementation Roadmap

Status: CURRENT EXECUTION ROADMAP
Date: 2026-09-23
Owner: Founder / ChatGPT control plane / Codex execution plane

Purpose: turn the founder-approved technology architecture into an ordered implementation program that supports the current Formalife build sequence without allowing software work to outrun commercial proof.

This roadmap is subordinate to:

- `FORMALIFE_BUILD_SEQUENCE.md` for business construction order;
- `TECHNOLOGY_ARCHITECTURE.md` for the current approved technology stack and systems-of-record boundaries;
- current offer/funnel documents for customer-facing commercial rules.

The first economic objective is not to rebuild every Formalife digital asset. It is to make the choking-led B2C flagship engine technically reliable, measurable and repeatable.

---

## 1. Governing execution principle

Build the smallest complete commercial system that can produce, observe and service a real customer transaction end to end.

The first complete technical loop is:

**relevant traffic -> purpose-specific page -> offer / edition choice -> Stripe Checkout -> verified payment event -> Twenty operational record -> customer communication -> delivery state -> post-purchase state -> measurable analytics.**

Do not optimize isolated layers while this loop is incomplete.

The roadmap deliberately follows five rules:

1. **commercial causality before completeness** — the first production path must support Block 1 of `FORMALIFE_BUILD_SEQUENCE.md`;
2. **one source of truth per job** — avoid duplicate operational databases and parallel customer records;
3. **code for invariants, configuration for presentation** — financial/state-transition rules must be deterministic and testable;
4. **progressive sophistication** — do not introduce deferred tools until the approved stack demonstrates a concrete limitation;
5. **observable gates** — every phase has an exit condition; finishing tasks is not the same as proving the phase works.

---

## 2. Current frozen architecture

### Public web

- Astro;
- Tailwind CSS v4;
- shadcn/ui as owned component scaffolding;
- Base UI as default interactive primitive layer;
- React only as selective Astro islands;
- Tabler Icons for Astro;
- motion hierarchy: CSS -> Astro/native -> selective Motion;
- Cloudflare for DNS/CDN/edge deployment;
- Cloudflare R2 for object storage where needed.

### Operational / commercial systems

- Twenty self-hosted as CRM / operational operating-system platform;
- Stripe Checkout as payment checkout and Stripe as payment-event truth;
- Brevo as communication execution layer;
- PostHog Cloud free tier initially, while economically appropriate, for behavioral/product analytics.

### Deferred unless evidence requires them

- second operational database such as Neon/Supabase;
- Payload/other CMS;
- Activepieces;
- Trigger.dev;
- Metabase;
- custom Formalife OS frontend outside Twenty;
- customer portal/auth platform;
- GSAP as baseline dependency.

---

## 3. Repository and operating model

### Layer boundaries

- Layer 1 `formalife/merenda-business-core`: doctrine / decision discipline;
- Layer 2 `formalife/formalife-company-os`: company truth, decisions, architecture, tests/results at business level;
- implementation repository: code, schemas, application configuration, infrastructure-as-code, tests and deployment logic;
- Twenty: operational CRM/process records;
- Stripe: payment truth;
- Brevo: message execution;
- PostHog: behavioral analytics;
- Drive: human source assets/workspace;
- Cloudflare/R2: public infrastructure and object delivery.

### Working implementation repository name

Use `formalife/formalife-platform` unless a repository naming conflict appears before creation.

The implementation repository must not become a shadow Company OS. Business decisions are referenced from Layer 2 rather than silently re-decided in code.

### Control / execution split

**ChatGPT control plane**

- architecture and decision formation;
- business-rule translation;
- acceptance criteria;
- selective write-back to Layer 2;
- review of Codex outputs against current company truth.

**Codex execution plane**

- repository bootstrap;
- multi-file implementation;
- CI/CD;
- integration code;
- tests and debugging;
- migrations/refactors;
- infrastructure automation where appropriate.

Every substantial implementation change becomes current only after it exists in canonical GitHub and is reread from there.

---

# EXECUTION PHASES

## Phase 0 — Program bootstrap and guardrails

### Objective

Create an execution environment where later work is reproducible, reviewable and cannot silently drift from Layer 2.

### Work

1. Create the implementation repository `formalife/formalife-platform`.
2. Add root governance files:
   - `README.md`;
   - `AGENTS.md` with repo-specific Codex instructions;
   - `docs/architecture.md` linking Layer 2 decisions rather than copying strategy;
   - `docs/adr/` for implementation-level architecture decisions;
   - `.env.example` with names only, no secrets.
3. Select a package manager and pin it in the repository. Preferred default: `pnpm` unless an implementation constraint appears.
4. Pin Node/runtime versions.
5. Establish branch/PR workflow and required checks.
6. Define environments:
   - local;
   - preview/staging;
   - production.
7. Define secrets ownership and where each secret may exist.
8. Define naming conventions for Cloudflare resources, R2 buckets, environment variables and domains/subdomains.
9. Create a minimal implementation issue/backlog linked to this roadmap rather than an unbounded feature list.

### Exit gate P0

Phase 0 is complete only when:

- implementation repository exists;
- a clean checkout can install and run its baseline checks from documented commands;
- environments and secret boundaries are documented;
- business decisions are referenced back to Layer 2;
- no production credential is committed to Git.

---

## Phase 1 — Twenty self-hosting production-readiness spike

### Objective

Prove that the free/open-source self-hosted Twenty direction can safely serve as Formalife's operational CRM/OS before customer transactions depend on it.

### Work

#### 1. Hosting candidate test

Evaluate the preferred zero-cost infrastructure candidate first, then paid fallback only if the free option fails the operational gate.

For each candidate verify:

- architecture compatibility with the official Twenty images/build;
- CPU/RAM headroom under server + worker + PostgreSQL + Redis;
- persistent volume behavior;
- outbound email/API connectivity;
- DNS/TLS feasibility;
- restart behavior;
- predictable resource limits;
- provider inactivity/reclamation rules where applicable.

#### 2. Deployment discipline

- deploy a pinned Twenty version, never uncontrolled `latest` in production;
- run Twenty server and worker separately as required;
- use persistent PostgreSQL storage;
- configure Redis with persistent operational expectations appropriate to Twenty;
- use strong generated application/encryption secrets;
- expose only required public ingress;
- terminate TLS correctly;
- restrict administrative surface where practical.

#### 3. Backup / restore

- automated PostgreSQL backups to an off-host destination;
- retain multiple restore points;
- encrypt backup transport/storage where applicable;
- document the restore command/path;
- perform an actual restore into a disposable environment;
- record result, recovery steps and unresolved gaps.

R2 may be used as an off-host backup destination if the implementation is operationally sound; public R2 buckets must never be used for database backups.

#### 4. Upgrade / rollback

- create a staging/upgrade procedure;
- snapshot/backup before migrations;
- test the next Twenty version away from production;
- document rollback/recovery when migration cannot be reversed cleanly;
- do not auto-upgrade production.

#### 5. Monitoring

At minimum monitor:

- service availability;
- host disk use;
- database reachability;
- backup success/failure;
- Twenty health endpoint where available;
- abnormal restart loops.

#### 6. Security/privacy boundary

- do not store unnecessary clinical/sensitive-health details;
- restrict the model to operational/commercial information needed for Formalife processes;
- document roles and permission limitations of the free edition;
- record any capability that would require a commercial Twenty feature rather than silently working around it.

### Exit gate P1

Twenty self-hosting is accepted for production only when:

- the chosen host runs the pinned version reliably under expected current load;
- persistent data survives restart/redeploy tests;
- an off-host backup completes;
- a backup is successfully restored into a separate environment;
- monitoring detects a forced failure;
- an upgrade rehearsal is documented;
- no required current Formalife permission/security need depends on an unavailable feature.

If this gate fails, the response is to change hosting/Twenty edition — not to weaken backup, security or restore requirements.

---

## Phase 2 — Public-site technical foundation

### Objective

Create the smallest high-quality Astro application foundation before visual page production begins.

### Work

1. Bootstrap Astro with strict TypeScript.
2. Add Tailwind CSS v4.
3. Add React integration only for client islands.
4. Configure shadcn/ui using Base UI primitives.
5. Add Tabler Icons Astro as the default icon source.
6. Add Motion only in a lazy/minimal form when an implemented interaction requires it.
7. Configure Cloudflare deployment/adaptor as required by the selected Astro architecture.
8. Configure R2 binding/access pattern where needed.
9. Add source-level environment validation.
10. Add image handling with Astro `Image`/`Picture` patterns.
11. Add self-hosted font loading path; final font family is selected in Phase 3.
12. Define global security/quality defaults:
    - HTTPS-only production;
    - safe security headers appropriate to the app;
    - noindex for non-production environments;
    - canonical URL strategy;
    - error/404 handling;
    - privacy-safe logging.
13. Set baseline CI checks:
    - install reproducibility;
    - `astro check` / TypeScript checks;
    - lint/format if adopted;
    - unit tests where logic exists;
    - Playwright smoke tests;
    - axe accessibility checks on representative pages;
    - Lighthouse CI or equivalent regression checks.

### Performance budget direction

Prefer static/native Astro markup for marketing/editorial content.

Client JavaScript is justified only by user interaction. Do not hydrate whole page sections because one nested control is interactive.

Motion should generally prefer transform/opacity and must respect reduced-motion preferences.

### Exit gate P2

- preview deploy works from GitHub;
- production build is reproducible;
- one native Astro page and one Base UI React island pass CI;
- accessibility and performance checks execute automatically;
- no site-wide React hydration exists;
- environment/secrets separation is verified.

---

## Phase 3 — Formalife visual language and design system

### Objective

Create a distinctive, reusable Formalife visual system before building multiple pages, so page production does not degenerate into one-off styling or generic shadcn defaults.

### Work

#### 1. Visual direction

Build a small curated reference set covering:

- pediatric/family trust without childish visual language;
- medical/scientific authority without hospital coldness;
- premium educational/editorial quality;
- clear direct-response hierarchy;
- physical/practical training cues;
- calm treatment of emergency-preparedness topics without shock aesthetics.

The reference set is inspiration, not a template to copy.

#### 2. Semantic design tokens

Freeze tokens for:

- brand and functional color roles;
- text/background/border states;
- typography scale;
- heading/body/label roles;
- spacing scale;
- grid/container widths;
- radii;
- shadows/elevation;
- focus rings;
- state colors;
- motion duration/easing;
- z-index/layering conventions.

#### 3. Typography

Select self-hosted variable font(s) based on:

- Italian readability;
- authority/clarity;
- performance;
- sufficient weights/styles;
- licensing;
- visual distinction.

Avoid loading unnecessary font files/weights.

#### 4. Icon system

Use Tabler as default and define:

- standard sizes;
- default stroke width;
- icon/text gap;
- semantic use of color;
- when filled/custom SVG is allowed;
- prohibition on mixing icon families without explicit design reason.

Create custom SVG marks only where a brand/safety concept deserves a proprietary symbol.

#### 5. Primitive components

Own and style at minimum:

- Button / icon button;
- text link;
- badge/status;
- input/textarea;
- select/combobox where needed;
- checkbox/radio;
- accordion;
- dialog/sheet;
- tabs only if genuinely useful;
- tooltip only where content cannot be stated directly;
- alert/callout;
- card/container primitives.

#### 6. Commercial section patterns

Create reusable Formalife sections rather than page-specific visual hacks:

- hero;
- authority/provenance block;
- proof strip;
- testimonial/proof card;
- problem/education section;
- method/process section;
- practical-experience block;
- offer/package block;
- edition/availability card;
- price/household-choice block;
- guarantee/risk-reversal block;
- FAQ/objection block;
- final CTA;
- related resource/content block;
- scientific-review/provenance notation where useful.

#### 7. Motion system

Use three levels:

- Level 1: CSS microinteraction;
- Level 2: browser/Astro navigation transition where useful;
- Level 3: Motion inside specific interactive islands.

Do not introduce global scroll-animation on every section.

Every motion pattern must pass reduced-motion behavior.

### Exit gate P3

- design tokens are centralized and documented;
- representative page sections can be composed without local ad-hoc design values;
- desktop/mobile states exist;
- keyboard/focus states are correct;
- Base UI/shadcn components no longer look like unmodified defaults;
- icon and motion rules are demonstrably consistent;
- visual system has been reviewed against at least one real Formalife sales-page composition, not only an isolated component gallery.

---

## Phase 4 — Information architecture, page map and content model

### Objective

Define the public web as a commercial information system before implementing every page.

### Initial route architecture

Minimum current target routes:

- `/` — Formalife masterbrand + routing hub;
- `/soffocamento-pediatrico/` — choking/weaning problem/education entry;
- `/corso-sicurezza-pediatrica/` — current Full flagship sales page;
- `/guida-anti-panico/` — Guide page/front-end;
- `/edizioni/[slug]/` — individual live edition availability/decision page;
- `/esperti/...` — authority/expert pages;
- `/risorse/...` — educational/searchable content;
- `/partner/` — B2B/B2B2C entry;
- legal/privacy/cookie/terms pages as required.

The final URL wording can be refined before launch, but each route must have a defined commercial/search job.

### Content model

Start code-first using Astro content collections/MDX or the lightest native content model that supports:

- articles/resources;
- experts/authority profiles;
- testimonials/proof records;
- static offer content;
- FAQ structured content;
- edition references where appropriate.

Do not introduce a CMS until actual editing workflow demonstrates the need.

### SEO / migration preparation

- inventory current WordPress URLs that have traffic/backlinks/commercial value;
- preserve or redirect valuable paths;
- define canonical tags;
- sitemap/robots;
- Open Graph/social metadata;
- structured data where semantically correct;
- maintain a redirect manifest in code;
- do not migrate low-value legacy pages merely for parity.

### Medical/scientific publishing rule

Any customer-facing medical/scientific claim or educational material follows the current Formalife scientific review requirement before publication.

### Exit gate P4

Every initial route has:

- audience/state;
- purpose;
- primary CTA;
- proof requirement;
- source/content owner;
- measurable outcome;
- routing/fallback.

No page is approved merely because the company historically had that page in WordPress.

---

## Phase 5 — Twenty operational data model

### Objective

Make Twenty capable of representing the first real Formalife customer relationship and course transaction without using spreadsheets as hidden operational truth.

### Initial object model

Start with the minimum object graph needed for Block 1:

- `Person`;
- `Household`;
- `Partner`;
- `Lead/Commercial State` or equivalent relationship state;
- `Course Edition`;
- `Enrollment`;
- `Order`;
- `Payment Reference`/payment state mirror;
- `Training Credit`;
- `Entitlement`;
- `Consent/Permission` records or an explicitly designed consent representation;
- `Attribution Touch` / source fields sufficient for initial attribution;
- `Referral`;
- operational `Task`/follow-up state.

Do not create an object merely because data could theoretically exist.

### Required state-machine definitions

Before automation, define allowed states/transitions for at least:

#### Order

Example direction:

`draft -> checkout_created -> paid -> refunded/partially_refunded/cancelled`

Exact states are implementation detail and must match Stripe/business rules.

#### Edition

At minimum distinguish:

- draft;
- activation/pre-enrolment where used;
- confirmed;
- completed;
- cancelled.

#### Enrollment

At minimum distinguish:

- pending;
- confirmed;
- transferred;
- attended/completed;
- cancelled/no-show where relevant.

#### Credit / entitlement

Must have explicit:

- creation reason;
- value/benefit;
- activation date;
- expiry where applicable;
- redemption state;
- reversal/termination rules.

### Data ownership

For every field/object identify:

- system of record;
- who/what may create it;
- who/what may change it;
- whether it is derived;
- deletion/retention expectation;
- privacy classification.

### Historical import

Import historical customers only after schema and deduplication rules exist.

Preserve provenance/source where known and mark unknowns rather than fabricating precision.

### Exit gate P5

Using staging data, the model can represent without spreadsheet/manual side truth:

- one Single/1-Caregiver booking;
- one 2-Caregiver household booking;
- payment reference;
- edition capacity;
- transfer/cancellation;
- a Training Credit;
- refresh entitlement;
- source/partner attribution;
- consent state needed for communications.

---

## Phase 6 — First end-to-end commercial vertical slice: direct Full purchase

### Objective

Produce the first revenue-capable path before building the complete site.

### Scope

Use a confirmed live Full edition first. Do not make the first integration test depend on every activation/pre-enrolment exception.

### Customer flow

1. visitor reaches a relevant Full sales/edition page;
2. sees verified availability and offer terms;
3. chooses 1 Caregiver or 2 Caregivers;
4. checkout session is created server-side;
5. visitor completes Stripe Checkout;
6. Stripe webhook is signature-verified;
7. webhook processing is idempotent;
8. Twenty order/payment/enrollment/household records are created or updated;
9. seat/capacity state is updated deterministically;
10. transactional confirmation is sent;
11. PostHog records non-sensitive funnel events;
12. success page reflects verified state rather than trusting query-string success alone.

### Engineering requirements

- never trust browser redirect as proof of payment;
- verify Stripe webhook signatures;
- store Stripe identifiers needed for reconciliation;
- idempotency for duplicate/reordered events;
- explicit failure/retry path;
- manual reconciliation procedure for exceptional states;
- sandbox test data must be clearly separated from production;
- no card data handled directly by Formalife infrastructure.

### Initial events to observe

- sales page viewed;
- edition viewed;
- household option selected;
- checkout started;
- checkout completed/paid (server-confirmed where practical);
- enrollment confirmed;
- checkout failed/abandoned signal where legitimately observable.

### Exit gate P6

The vertical slice is complete only when automated/manual tests demonstrate:

- successful 1-Caregiver purchase;
- successful 2-Caregiver purchase;
- duplicate webhook does not duplicate enrollment/order;
- failed payment does not consume a confirmed seat;
- refund/reconciliation path is understood;
- correct Twenty record exists;
- correct customer communication is produced;
- source/UTM context survives into the customer/order record at the intended fidelity;
- PostHog shows the measurable path without exposing unnecessary personal/sensitive data.

---

## Phase 7 — Current Block-1 offer rules: Guide, Training Credit, activation and protection

### Objective

Extend the simple Full vertical slice to the founder-approved current commercial architecture.

### 7A — Guide purchase

Implement:

- Guide sales page;
- Stripe Checkout;
- shipping/contact data required for fulfilment;
- Guide order state;
- buyer/customer creation/update in Twenty;
- fulfilment status sufficient for current operations;
- economics/attribution fields needed for later analysis.

Do not build warehouse software. Manual fulfilment can remain an explicit human task while order volume is small.

### 7B — Training Credit

Implement current rules from Layer 2, including:

- credit creation from eligible Guide purchase;
- 1–90 day Momentum value;
- 91–365 day base value;
- expiry/state;
- household combination rules only to the extent currently approved/tested;
- no automatic duplicate Guide when the existing Guide counts toward the Full package;
- immutable audit trail of credit issuance/redemption/reversal.

### 7C — Protected pre-enrolment / activation editions

Implement only after the confirmed-edition path is stable.

Required states include:

- edition published as activation/pre-enrolment;
- published activation deadline/threshold;
- Guide/pre-enrolment payment;
- credit if activated;
- automatic/refund-operational path if not activated;
- customer keeps Guide according to current offer rule;
- capacity/activation calculation must not rely on editable presentation text.

### 7D — Total Protection / guarantee operationalization

Translate current policy into explicit operator states/tasks:

- refund;
- transfer;
- Formalife credit;
- late genuine emergency handling;
- no-show recovery where current rule applies;
- Formalife cancellation goodwill credit;
- Better-Than-Risk-Free refund and termination of future service entitlements.

Do not automate ambiguous exception judgment before the human escalation rule is clear.

### Exit gate P7

The system can execute/reconcile each founder-approved current transaction type without editing database rows manually as the normal process.

All exceptions have a documented human owner and recovery path.

---

## Phase 8 — Brevo communication layer and lifecycle routing

### Objective

Ensure customer communication follows operational state rather than becoming an independent email database with contradictory truth.

### Architecture

Twenty/business events decide relationship state.

Brevo executes approved communications.

### Required initial journeys

#### Lead / problem-aware contact

- deliver promised asset/value;
- proof/authority education;
- appropriate CTA;
- stop/change sequence after purchase or explicit state change.

#### Guide buyer

- order/fulfilment messaging;
- usage/value messaging;
- Training Credit explanation;
- Full offer when appropriate;
- do not repeatedly sell the Guide already purchased.

#### Full buyer

- purchase confirmation;
- logistics/pre-course preparation;
- reminders;
- post-course reference;
- feedback/proof request when appropriate;
- refresh entitlement explanation;
- referral/next state only where appropriate.

#### Activation pre-enrollee

- pre-enrolment receipt/state;
- edition activation progress only if useful/accurate;
- activation confirmation or failure/refund outcome;
- no misleading certainty before threshold is met.

### Consent boundary

Keep transactional/service communication operationally and legally distinct from marketing/editorial permission.

Do not subscribe a buyer to marketing merely because payment occurred if the lawful permission/state does not allow it.

### Synchronization rules

Define:

- authoritative fields sent to Brevo;
- identifiers/deduplication;
- unsubscribe handling;
- hard bounce/invalid address handling;
- what flows back to Twenty, if anything;
- retry and alert behavior.

### Exit gate P8

A test person can move through lead -> buyer -> post-purchase states without:

- receiving stale acquisition emails after purchase;
- duplicate transactional messages;
- losing unsubscribe/permission state;
- requiring manual list movement as the normal workflow.

---

## Phase 9 — PostHog measurement, attribution and privacy controls

### Objective

Make the first commercial engine diagnostically readable before buying/scaling cold traffic.

### Event taxonomy

Create a versioned analytics specification. Initial event families:

#### Acquisition

- landing/page view;
- source/UTM capture;
- resource CTA;
- lead/permission success where applicable.

#### Commercial intent

- offer viewed;
- edition viewed;
- household option selected;
- checkout started;
- checkout result.

#### Customer state

Prefer server-side/operational truth for actual purchase/enrollment where feasible rather than treating browser events as financial truth.

#### Experience

- key content engagement only when it changes a decision;
- avoid vanity-event proliferation.

### Identity

Define when anonymous activity becomes associated with a known person while respecting privacy/consent design.

Do not send unnecessary personal, child or health-related information to PostHog.

### Session replay

If enabled:

- mask sensitive fields;
- exclude payment surfaces not controlled by Formalife;
- review privacy settings before production;
- disable replay where risk exceeds diagnostic value.

### Initial dashboards

At minimum:

- source -> Full purchase funnel;
- source -> Guide purchase funnel;
- Guide -> Full progression;
- edition page -> checkout -> paid conversion;
- top landing pages/entry states;
- failure/drop-off monitoring;
- web performance/Core Web Vitals where available/useful.

### Exit gate P9

For a test/real transaction, Formalife can trace at intended fidelity:

**source -> page path -> checkout -> paid order -> customer/enrollment**

while keeping Stripe/Twenty, not PostHog, authoritative for payment/customer operational truth.

---

## Phase 10 — Production content, proof and first release candidate

### Objective

Turn the working platform into the minimum complete Block-1 public experience without expanding into unrelated future architecture.

### Minimum production content set

1. Home/masterbrand router.
2. Choking/weaning problem/education page.
3. Full flagship sales page.
4. Real edition pages.
5. Guide sales page.
6. Core expert/scientific authority page(s).
7. Proof/testimonial system sufficient for current claims.
8. Essential FAQ/objection/risk-reversal content.
9. Legal/privacy/cookie/terms content required for production.
10. Partner entry page may be minimal until Block 2 execution begins.

### Proof discipline

- distinguish proof from historical offer versus proof of materially rebuilt Full;
- use identifiable/specific proof where permission exists;
- do not stretch existing reviews into unsupported outcome claims;
- customer-facing medical/scientific claims receive required review.

### Release-candidate quality gate

Before cutover:

- responsive QA on representative devices/viewports;
- keyboard-only navigation;
- axe checks pass within defined exceptions;
- Lighthouse/performance budget meets agreed threshold;
- broken-link crawl;
- form/error states tested;
- Stripe test purchase/refund path rehearsed;
- Twenty backup verified current;
- Brevo journeys tested;
- PostHog events validated;
- structured metadata/canonical/sitemap reviewed;
- staging is noindex;
- security headers/CSP behavior tested where configured.

---

## Phase 11 — WordPress migration and domain cutover

### Objective

Replace the current public site without throwing away useful SEO, proof, attribution or rollback capability.

### Pre-cutover inventory

Capture from the current site:

- valuable URLs;
- search/traffic pages where data is available;
- backlinks/externally referenced URLs where observable;
- page titles/meta/canonicals;
- downloadable/media assets still required;
- existing forms/conversion points;
- analytics/tag configuration;
- redirects already in force;
- legal pages/current public wording that must be intentionally replaced or preserved.

### Redirect map

For each old valuable URL classify:

- retained unchanged;
- redirected 301 to a semantically equivalent new URL;
- intentionally removed with appropriate response;
- temporarily preserved.

Do not mass-redirect unrelated legacy pages to the homepage.

### Cutover

- take final backup/snapshot of old WordPress;
- freeze old-site content changes during final cutover window;
- deploy release candidate;
- apply DNS/routing;
- validate TLS;
- verify robots/canonical/sitemap;
- perform real production smoke checkout using an appropriately controlled transaction if needed;
- validate Stripe webhook production endpoint;
- validate Twenty/Brevo/PostHog production events;
- monitor 404/5xx and conversion-critical failures.

### Rollback

Document the exact rollback path before DNS/cutover. Do not discover rollback mechanics during an outage.

### Exit gate P11

- primary commercial routes work in production;
- critical old URLs resolve correctly;
- real production payment path is operational;
- CRM/message/analytics side effects are correct;
- no material SEO/functional regression remains unexplained;
- old site can be archived/decommissioned only after the new path is stable.

---

## Phase 12 — Block-1 commercial proof and optimization

### Objective

Stop treating the site as a design project and use it to produce the evidence required by `FORMALIFE_BUILD_SEQUENCE.md`.

### First demand priority

Before assuming paid cold acquisition is required, deliberately use/read:

- existing customers;
- referral;
- historical/dormant contacts where lawful and appropriate;
- pediatrician/trusted relationship traffic;
- high-intent organic/direct demand;
- then controlled paid acquisition tests.

### Core measurements

Track at least:

- revenue per edition;
- normalized contribution per edition;
- customers/participants per edition;
- order mix: 1 vs 2 Caregivers;
- source/customer;
- conversion by source where denominator is measurable;
- Guide -> Full progression;
- direct Full conversion;
- checkout abandonment/failure;
- refunds/transfers;
- Training Credit issuance/redemption;
- reason customers say they chose Formalife;
- perceived difference;
- referral/proof creation;
- CAC once paid traffic begins;
- payback/customer contribution as data becomes sufficient.

The current survival floor of 5 paying participants remains an operating cash floor, not proof of normalized economics. The target remains 10–12 paying participants per edition under the current Layer 2 decision.

### Optimization discipline

When a metric is weak:

1. locate the stage where the loss occurs;
2. check the nearest upstream cause;
3. change the smallest relevant variable;
4. measure again;
5. do not buy more traffic merely to conceal a conversion/process defect.

### Gate before Block 2 scale

The platform/commercial engine must make the Build Sequence Block-1 gate readable:

- revenue/contribution by edition;
- source/customer;
- conversion where measurable;
- Guide/direct-Full behavior;
- customer history;
- customer reason/difference evidence;
- reliable delivery/operational states.

---

# LATER BUSINESS BLOCKS — IMPLEMENT ONLY AFTER CURRENT GATES

## Phase 13 — Trusted B2B/B2B2C distribution

When Block 2 begins, extend rather than rebuild the platform:

- partner object/pipeline;
- partner attribution/referral codes/links;
- hosted Light event representation;
- partner-generated participants/customers;
- partner economics;
- repeat-initiative tracking;
- central payment/customer record where Formalife owns the transaction.

Do not mix partner-interest pipeline with proven producing partners.

## Phase 14 — Digital flagship

Only after the live flagship path is coherent and measurable:

- select video/content delivery infrastructure based on actual Digital product requirements;
- add Digital product/order/entitlement state;
- support Guide -> Digital, Digital -> Live and Hybrid only where economically justified;
- preserve Twenty as unified relationship record;
- add completion/usage signals only when useful for routing/customer success.

Do not build a broad digital catalog before one digital flagship monetizes successfully.

## Phase 15 — Owned demand / lifecycle and adjacent expansion

Extend the same data model toward:

- recurring useful public utilities;
- trigger-based lifecycle state;
- specialist partnership products;
- additional caregiver/family extension;
- future BLSD lanes;
- further safety verticals only when earlier economics justify them.

No membership, complex customer portal or large product catalog is required by this roadmap until the business model earns the need.

---

## 4. Cross-cutting engineering standards

These standards apply across phases.

### Reliability

- pinned dependencies for critical infrastructure;
- deterministic migrations;
- idempotent external-event handling;
- backup before destructive migration;
- explicit error/retry states;
- human reconciliation path for financial/operational exceptions.

### Security

- least-privilege credentials;
- secret rotation capability;
- no secrets in client bundles or Git;
- webhook signature verification;
- dependency/security update process;
- administrative surfaces not unnecessarily public.

### Privacy/data minimization

- collect only data that changes routing, fulfilment, compliance or customer value;
- no unnecessary clinical/health details;
- separate marketing permission from service necessity;
- documented deletion/retention logic as requirements mature;
- analytics must not become a shadow CRM.

### Accessibility

- semantic HTML first;
- keyboard support;
- visible focus;
- appropriate labels/errors;
- reduced-motion support;
- automated axe plus manual review for critical flows.

### Performance

- static/native Astro by default;
- responsive optimized images;
- fonts subset/minimized where appropriate;
- client islands only when interaction requires them;
- no decorative animation dependency loaded globally without demonstrated value;
- performance regressions treated as defects.

### Observability

For every critical integration know:

- what event happened;
- when;
- external/internal identifier;
- resulting state;
- failure/retry condition;
- owner for unresolved exceptions.

---

## 5. Decision and change-control rules

### A technology may be added only when

1. a concrete current requirement cannot be met adequately by the approved stack;
2. the gap is documented;
3. adding the tool reduces more risk/complexity than it introduces;
4. data ownership/export/exit is understood;
5. Layer 2 records the decision when material.

### Do not add technology because

- it is fashionable;
- it makes a demo faster while adding a permanent system;
- a competitor uses it;
- it duplicates Twenty/Cloudflare/PostHog/Brevo functionality without a measured reason;
- it avoids defining the business process first.

---

## 6. Definition of Done for Release 1

Formalife Platform Release 1 is not defined by a page count.

It is done when all of the following are true:

1. Astro public site is production-deployed through Cloudflare.
2. Formalife design system is consistently applied and accessible.
3. Full sales + edition path supports real 1/2-Caregiver selection.
4. Stripe Checkout works with verified/idempotent webhook handling.
5. Twenty receives correct customer/order/enrollment operational state.
6. Brevo sends correct state-dependent service communications.
7. PostHog can diagnose the public funnel without being payment/customer source of truth.
8. Guide purchase and current Training Credit rules are operational or explicitly staged immediately after the stable direct-Full path according to Phase 7.
9. backup/restore for Twenty has been tested.
10. current WordPress commercial URLs have an intentional migration/redirect outcome.
11. Playwright/accessibility/performance quality gates run in CI.
12. source -> purchase -> enrollment can be reconciled end to end.
13. the resulting data supports the current Block-1 commercial proof metrics.

---

## 7. Immediate execution queue — start here

Priority order from this decision:

### P0-A — implementation repository

- create `formalife/formalife-platform`;
- bootstrap governance/README/AGENTS/ADR structure;
- establish CI skeleton.

### P1-A — Twenty staging spike

- choose first zero-cost hosting candidate;
- verify architecture/image compatibility;
- deploy pinned Twenty staging;
- exercise persistent restart;
- configure first off-host backup;
- restore into disposable instance;
- record host/resource result.

### P2-A — Astro technical skeleton

- Astro + strict TypeScript;
- Tailwind v4;
- React islands;
- shadcn/Base UI;
- Tabler Icons;
- Cloudflare preview deploy;
- Playwright + axe + Lighthouse CI skeleton.

P1-A and P2-A may proceed in parallel after P0-A because they do not depend on each other's implementation. They converge before Phase 5/6 integration.

### P3-A — design-system sprint

Only after the Astro skeleton is stable:

- visual-reference research;
- typography decision;
- semantic tokens;
- icon/motion policy;
- primitive components;
- one representative Full sales-page composition for system validation.

### First integration milestone

Do not expand page count until this works in staging:

**Full page -> edition -> Stripe test checkout -> webhook -> Twenty -> confirmation -> PostHog event chain.**

That is the first technical proof milestone of this roadmap.

---

## 8. Layer 1 references

- `REASONING_KERNEL.md` — first bottleneck, minimum evidence-producing intervention, tools after process definition;
- `merenda/04_marketing/complessita-e-riduzione-variabili.md` — sophisticated integrated system while reducing failure variables;
- `merenda/05_acquisizione/funnel-e-conversione.md` — purpose-specific landing, measurable funnel stages, state-based routing/fallbacks, diagnose bottleneck before more traffic;
- `merenda/09_business/scalabilita-e-operativita.md` — avoid critical single points of failure, standardize/transfer processes, stress-test operational dependencies.

---

## 9. Revision condition

Revise this roadmap only when:

- Layer 2 business strategy/build order changes;
- a frozen technology fails a concrete security, reliability, capability or economic gate;
- measured commercial evidence changes the required funnel/product sequence;
- a newly discovered dependency makes the current order causally wrong.

Do not reorder the roadmap merely because a later feature is easier or more interesting to build.