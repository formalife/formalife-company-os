# Personalized Sales Asset v1

Status: CURRENT TEST ARCHITECTURE — founder-authorized
Date: 2026-09-26

## Purpose

Use AI to create **prospect-specific commercial experiences** without collapsing qualification into premature proposal generation.

The system should make Formalife unusually relevant and easy to evaluate for a buyer while preserving the current sales discipline: pre-education and evidence can be personalized early; commercial prescription, scope and price must wait until the relevant facts are known.

## 1. Core rule

**Personalize to the prospect state, not merely to the prospect name.**

A polished page built on unknown assumptions is worse than a generic page because it creates false confidence and may distribute value before a real buying process exists.

Therefore the asset type is selected from prospect state.

## 2. Asset ladder

### Stage P0 — researched, no reply / no qualification

Allowed asset: **Fit Brief**.

Job:

- show that Formalife understands the organization's public mission/context;
- explain the specific hypothesis for why pediatric-safety family education may fit;
- make Formalife's relevant assets concrete;
- reduce the amount of standard explanation required in the next exchange;
- ask one easy qualification question or support routing to the right owner.

Must not:

- state that budget is available;
- claim procurement/eligibility fit;
- prescribe exact scope;
- provide final institutional pricing;
- imply an agreed problem the prospect has not stated.

### Stage P1 — initial reply / light qualification

Allowed asset: **Exploration Brief**.

May add:

- what the prospect has explicitly said;
- known timing;
- known beneficiary configuration;
- open qualification gaps;
- two or three plausible program configurations clearly labeled as examples, not proposals.

### Stage P2 — qualified opportunity

Allowed asset: **Interactive Proposal**.

Gate requires sufficient clarity on:

- problem/program fit;
- buyer/beneficiary configuration;
- budget/eligibility or credible purchasing path;
- scope;
- decision process/stakeholders;
- procurement requirements;
- timing.

The asset may then contain:

- agreed situation and desired result;
- recommended program configuration;
- exact deliverables;
- implementation path;
- relevant proof;
- investment/pricing;
- assumptions and exclusions;
- approval/procurement support materials;
- explicit next decision.

### Stage P3 — internal approval/procurement

Allowed asset: **Decision Pack**.

Job:

- help the sponsor communicate internally;
- provide concise scope, economics, requirements, scientific/operating credentials and requested procurement documents;
- map each known decision criterion to the relevant evidence.

Do not add sales theater. The asset is designed to make an already-qualified decision easier to evaluate and defend internally.

## 3. Prospect dossier

Every personalized asset begins from a dossier that separates evidence from inference.

Minimum fields:

- organization and specific program/unit;
- verified public mission/services;
- target population/geography;
- relevant current initiatives;
- explicit public funding/project facts when material;
- known contact/role;
- current relationship stage;
- prospect statements from correspondence;
- known decision/procurement facts;
- Formalife assets potentially relevant;
- possible fit hypotheses;
- explicit unknowns;
- source and last-checked date for every material external fact.

## 4. Claim classes

Every material sentence in a generated asset should be classifiable as:

- `PROSPECT_VERIFIED` — public/retrieved evidence about the prospect;
- `FORMALIFE_VERIFIED` — current canonical Formalife fact/asset;
- `PROSPECT_STATED` — explicitly said by the prospect;
- `FIT_HYPOTHESIS` — Formalife interpretation that must be phrased as a hypothesis;
- `PROPOSAL_TERM` — a commercial term authorized for this opportunity;
- `UNKNOWN` — not to be silently filled.

The generation layer may improve wording but may not change claim class.

## 5. Generation pipeline

```text
PROSPECT / TRIGGER
      ↓
PUBLIC + CANONICAL RESEARCH
      ↓
PROSPECT DOSSIER
      ↓
STAGE CLASSIFIER
      ↓
ASSET CONTRACT
      ↓
COPY / STRUCTURE GENERATOR
      ↓
EVIDENCE + CLAIM VALIDATOR
      ↓
BUSINESS-RULE VALIDATOR
      ↓
BRAND / SCIENTIFIC GUARD WHEN REQUIRED
      ↓
DRAFT
      ↓
HUMAN GATE
      ↓
PUBLISH / SEND
      ↓
OUTCOME CAPTURE
```

## 6. Evidence validator

Before an asset can pass:

- each prospect-specific factual claim must have a source or be removed/softened;
- stale or contradictory evidence must be surfaced;
- inferred need must not be rewritten as prospect-stated need;
- public funding must not be rewritten as remaining available budget;
- program existence must not imply procurement eligibility;
- named stakeholder role must not imply final decision authority without evidence.

## 7. Business-rule validator

For Centri per la Famiglia first-wave assets, reject drafts that:

- pitch a generic standalone course/book instead of the current program hypothesis;
- send fixed institutional pricing before qualification;
- ask for a call as the default first-wave CTA;
- claim rendicontabilità/admissibility without evidence;
- pretend the Center lacks services that public evidence shows it already offers;
- skip routing/qualification logic;
- represent Formalife consumer education as professional certification or medical advice.

## 8. Page structure — P0 Fit Brief

Preferred minimal structure:

1. **Context-specific opening** — one verified reason the organization is relevant.
2. **The fit hypothesis** — one short paragraph explaining why the Formalife program may complement its current family/prevention work.
3. **What the program can include** — modular, non-prescriptive components.
4. **Why Formalife is a plausible partner** — only current verified assets/proof relevant to this buyer.
5. **How it could integrate** — examples framed conditionally, tied to known Hub/Spoke, family, 0–6 or territorial model where verified.
6. **What we would need to understand first** — one or two high-value unknowns, not a questionnaire.
7. **Single next action** — reply/routing question consistent with the current outreach test.

Avoid generic hero-copy inflation, fake urgency, speculative ROI and decorative personalization.

## 9. Technical delivery

Initial implementation should be intentionally lightweight:

- source dossier: YAML/JSON/Markdown;
- generated content: structured Markdown/JSON;
- render target: Formalife platform component/template;
- unique prospect slug or token only when publication is authorized;
- no public indexation by default for prospect-specific pages;
- no sensitive/private prospect information in public assets;
- track page use only when lawful and useful, without turning a view into buying intent.

A page is a pre-education/decision-support asset, not evidence that the prospect is qualified.

## 10. Reusable template vs bespoke generation

Do not generate an entirely new website per prospect.

Use:

**stable design/system + stable claim rules + stable stage templates + variable evidence-backed content.**

This keeps production cheap, prevents visual/offer drift and allows the generator to concentrate on relevance rather than arbitrary design.

## 11. Metrics

Compare personalized assets against the current baseline using stage-appropriate outcomes.

P0/P1:

- reply quality;
- routing to correct owner;
- qualification information obtained;
- time to meaningful next step;
- founder minutes required per asset.

P2/P3:

- proposal-to-decision progression;
- missing stakeholder/procurement surprises after proposal;
- revision cycles;
- time from qualified opportunity to decision;
- economics of won work.

Do not optimize primarily for page views or time-on-page.

## 12. First test — Family Polo

Use Family Polo — Elefanti Volanti, Brescia as the first P0 benchmark because it is already inside the founder-authorized first wave.

Current verified/public context includes:

- Family Polo is a Centro per la Famiglia serving families in Ambito 1 Brescia;
- it uses an Hub + two Spoke structure;
- its stated activities include orientation, socio-educational work, prevention and promotion;
- its public model emphasizes territorial networks and collaboration among public/private/community actors.

The first asset must therefore position the `Programma Sicurezza Pediatrica per le Famiglie` as a possible structured addition to that prevention/family-support architecture, not assert that Family Polo needs another isolated first-aid event.

The test ends at a validated internal draft. Founder decides whether/how it becomes customer-facing.

## 13. Success criterion for v1

This architecture earns continuation only if the first assets are:

- materially more specific than a normal personalized email;
- factually clean;
- fast enough to produce repeatedly;
- useful in advancing qualification or decision;
- clearly better than sending a generic brochure;
- not dependent on founder rewriting each page from scratch.

## Provenance

- Founder authorization: 2026-09-26 Project conversation.
- Current Centri first-wave and qualification rules: `CENTRI_FAMIGLIA_OUTREACH_TEST_V1.md`.
- Layer 1 sales discipline: `merenda/06_vendita/prequalifica-e-handoff-marketing-vendita.md` and `merenda/06_vendita/processo-decisionale-e-stakeholder.md`.
- Mission runtime: `MISSION_CONTROL_V1.md`.
