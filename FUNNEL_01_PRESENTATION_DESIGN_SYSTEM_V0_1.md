# Funnel 01 — Presentation Design System V0.1

Status: CURRENT WORKING PRESENTATION SYSTEM — founder-directed; presentation-specific working identity, not yet the final Formalife corporate brand system. Corporate brand tokens remain provisional until the platform P3 brand phase is frozen.
Date: 2026-09-23

Purpose: provide one coherent visual system for the Full Course PowerPoint so the 69-slide production deck can be built from Slide Master/theme primitives rather than ad-hoc slide styling.

Canonical parents:

- `FUNNEL_01_FULL_SLIDE_BLUEPRINT_V2.md`
- `FUNNEL_01_INSTRUCTOR_SLIDE_OVERLAY_V1.md`
- `FUNNEL_01_SLIDE_CUE_SHEET_V1.md`
- `FUNNEL_01_PRESENTATION_DESIGN_RESEARCH_V1.md`

Platform note:

`formalife/platform/apps/web/src/styles/global.css` currently contains technical neutral tokens and explicitly states that brand color, typography, geometry, elevation and motion are frozen later in P3. Therefore this file defines a **presentation working system**, deliberately centralized so it can later be rethemed without rebuilding individual slides.

---

# 1. Working color system

Founder corrections supersede the earlier chat proposal:

- remove the previous teal-like Accent 2 because it was too similar to Accent 1;
- remove the previous slate/blue Accent 3 because it did not belong to the intended palette;
- reserve theme slots for coherent families: primary teal + light teal, warning amber + light amber, emergency red, positive/correct green;
- make the positive green fresher and more recognizably `correct`, avoiding a forest-green feel.

## 1.1 PowerPoint theme color slots

| PowerPoint slot | Role | HEX |
|---|---|---|
| Dark 1 | Main ink | `#172126` |
| Light 1 | Pure surface | `#FFFFFF` |
| Dark 2 | Deep Formalife teal | `#0B3F42` |
| Light 2 | Default canvas | `#F6F8F5` |
| Accent 1 | Formalife primary | `#0F6B70` |
| Accent 2 | Formalife primary light | `#D8ECEB` |
| Accent 3 | Warning amber light | `#F6E5C6` |
| Accent 4 | Warning amber | `#D99A3A` |
| Accent 5 | Emergency / 112 red | `#C8433E` |
| Accent 6 | Positive / correct green | `#0F8551` |

The old Accent 2 and Accent 3 are **replaced**, not retained.

## 1.2 Semantic use

### Accent 1 — Formalife primary

Use for:

- structure;
- chapter/current-focus indication;
- selected labels;
- active non-emergency path;
- brand continuity.

Do not treat it as `safe` or `correct` by default.

### Accent 2 — Primary light

Use for:

- quiet brand fields;
- completed course-rail segments;
- soft background geometry;
- low-emphasis structural panels.

Do not use light teal text on the default canvas.

### Accent 4 — Warning amber

Use for:

- `ATTENZIONE`;
- `NON FARE`;
- caution without immediate emergency activation.

### Accent 3 — Warning light

Use as the soft fill paired with Accent 4.

### Accent 5 — Emergency red

Use only for:

- `112`;
- emergency activation;
- critical escalation;
- truly time-critical danger.

Do not make normal emergency-related teaching slides globally red.

### Accent 6 — Positive / correct

Use for:

- correct branch;
- `continua`;
- appropriate action;
- resolved/appropriate state where useful.

Do not use as decorative brand green.

## 1.3 Component soft fills not consuming theme slots

When needed, use:

- Emergency soft: `#F7E6E4` with Accent 5 border/label;
- Positive soft: `#E4F2EA` with Accent 6 border/label;
- Neutral line: `#D9E0DD`;
- Muted text: `#657177`.

---

# 2. Typography baseline

Online presentation/accessibility guidance reviewed 2026-09-23 converges on:

- Microsoft: avoid sizes below 18 pt and go larger for distant audiences;
- multiple university accessibility programs: **24 pt minimum body** for in-person projection and **32 pt+ headings**;
- face-to-face guidance can reasonably push body toward 28 pt;
- Assertion-Evidence guidance historically uses 28 pt headlines / 18–24 pt body / 14 pt references, but Formalife should exceed those body sizes because this is a projected live course rather than a close-screen scientific talk.

Working Formalife baseline:

| Role | Working size |
|---|---:|
| Standard slide headline | **40–44 pt** |
| Short/high-impact headline | **44–52 pt** |
| Main explanatory label/body | **28–30 pt** |
| Secondary label | **24–26 pt** |
| Algorithm node | **26–32 pt** |
| Hero number/statistic | **56–80 pt** |
| Scenario facts | **28–32 pt** |
| Practice-hold instruction | **32–44 pt** |
| Course-rail chapter label | **12–14 pt** |
| Short source footer | **14–16 pt** |

Rules:

- instructional body text should not fall below **24 pt**;
- default target is **28 pt+** for projected live content;
- source footer is not instructional body content and may be smaller because the complete citation lives in notes/appendix;
- do not shrink font to rescue an overloaded slide; reduce content or split the slide;
- sans serif only in the working system;
- working production font: `Aptos Display` for display/headline and `Aptos` for supporting text until final corporate typography is frozen.

---

# 3. Default background system

Pure white is not the default canvas.

## 3.1 Standard technical canvas

Base:

**`#F6F8F5`**

This is a quiet near-white with enough warmth/softness to avoid the blank-office-document feel while retaining high contrast and colour fidelity for medical/data visuals.

## 3.2 Signature background geometry

Standard normal layouts may include one restrained Formalife field:

- one large ellipse/circle using Accent 2 `#D8ECEB`;
- approximately 14–18 cm diameter;
- placed mostly off-canvas in the upper-right corner;
- roughly 60–75% transparency depending on projector test;
- no border, shadow or texture.

Job: create a recognisable Formalife atmosphere without becoming an illustration.

It must be suppressible with `Hide Background Graphics` on:

- dense data charts;
- anatomy/mechanism slides where it competes with the medical visual;
- scenario photography;
- full-speed demo support;
- practice/QCPR holds;
- final bookend.

No global texture, pattern, stock medical motif or gradient is required.

## 3.3 Narrative / final backgrounds

Narrative/opening/final layouts may use:

- ordinary-life photography;
- controlled dark/deep-teal field;
- image + quiet colour overlay;

They are intentionally exceptions to the standard technical canvas.

---

# 4. Persistent course wayfinding

The former isolated `section marker` is replaced by a **Course Rail** on normal teaching slides.

Reason: a 4h10 course benefits from persistent orientation, but a large progress bar or repeated agenda would consume attention and make the deck feel like software UI.

## 4.1 Semantic chapters

The participant-facing rail uses seven chapters:

1. `PROBLEMA`
2. `METODO`
3. `SOFFOCAMENTO`
4. `RIANIMAZIONE`
5. `ALTRE EMERGENZE`
6. `APPLICAZIONE`
7. `CASA`

ACT 0 is absorbed into `PROBLEMA`; internal ACT numbering is not shown to participants.

## 4.2 Visual form

Normal slide top area contains:

- current chapter name, small but legible;
- seven short segments/dots/bars representing the course journey.

Suggested behavior:

- completed segments: Accent 2 light teal;
- current segment: Accent 1 primary teal;
- future segments: neutral line `#D9E0DD`;
- chapter text: Accent 1, Semibold.

Do **not** show numeric percentages or minutes remaining.

## 4.3 Visibility rule

Show on:

- normal explanatory slides;
- data slides;
- algorithm slides;
- ACT 5 topic slides.

Hide or strongly recede on:

- opening visual beat where it dilutes impact;
- live simulations;
- full-speed technique demos;
- practice-hold / QCPR activity;
- break slides;
- protected final bookend.

---

# 5. Logo rule

No large persistent branding block.

Working default for normal teaching slides:

- use the official Formalife logo **unmodified**;
- place it small in the **bottom-right footer zone**;
- keep source/citation material on the bottom-left;
- use the smallest size that remains clean when projected, approximately 2.2–2.8 cm wide if the current horizontal logo is used;
- preserve clear space and aspect ratio;
- do not recolour, distort or watermark the logo unless an official alternate logo asset exists.

Hide the logo when it competes with the learning moment:

- full-bleed narrative photography;
- simulations;
- full-speed demonstrations;
- practice screens;
- final slide.

The deck should remain recognisably Formalife through the system even when the logo is absent.

---

# 6. Master geometry — 16:9

PowerPoint widescreen base: approximately **33.87 × 19.05 cm**.

## 6.1 Safe margins

- Left: **1.40 cm**
- Right: **1.40 cm**
- Top safe: **0.35 cm** for course rail
- Bottom safe: **0.40 cm**

## 6.2 Course rail zone

Suggested y-range:

**0.35–0.80 cm**

Current chapter label left; seven micro-segments aligned to the right side of the safe width.

## 6.3 Headline zone

Suggested:

- X: **1.40 cm**
- Y: **1.05 cm**
- W: **31.07 cm**
- H: **1.90–2.00 cm**

Default headline starts below the course rail and is left-aligned.

## 6.4 Main visual/content zone

Suggested:

- X: **1.40 cm**
- Y: **3.20 cm**
- W: **31.07 cm**
- H: approximately **14.35 cm**

Individual layouts subdivide this area while preserving outer alignment.

## 6.5 Footer zone

Suggested y-range:

**18.00–18.60 cm**

- source/period/population: left;
- small Formalife logo: right on normal slides;
- keep the central footer visually empty.

---

# 7. Component styling

## 7.1 Neutral card

- Fill: `#FFFFFF`
- Border: none by default; optional 1 pt `#D9E0DD`
- Shadow: none
- Radius: moderate, consistent

Avoid SaaS-dashboard card overload.

## 7.2 Warning

- Soft fill: Accent 3 `#F6E5C6`
- Label/border/accent: Accent 4 `#D99A3A`
- Text: Dark 1 `#172126`

Use for `ATTENZIONE` / `NON FARE`.

## 7.3 Emergency / 112

- Strong: Accent 5 `#C8433E`
- Soft fill: `#F7E6E4`
- Text on strong red: white only where contrast/size remains acceptable
- Larger callouts: prefer soft red field + dark body + red `112` emphasis

## 7.4 Positive / correct

- Strong: Accent 6 `#0F8551`
- Soft fill: `#E4F2EA`
- Use text/icon/shape in addition to colour; never colour alone.

---

# 8. Chart style

Default chart language:

- no 3D;
- no chart shadow;
- no external plot border;
- direct labels instead of legends where possible;
- gridlines only when they support interpretation;
- Accent 1 for the protagonist series;
- Accent 2 / neutral greys for supporting/completed/background series;
- Accent 4 only for caution/warning meaning;
- Accent 5 and Accent 6 not used as arbitrary category colours because they are semantically reserved;
- observed data = solid fill;
- estimate/uncertain data = pattern/outline/ghost treatment in addition to textual labelling;
- source footer always names period/population when materially necessary.

---

# 9. Slide Master layout set

Create exactly these working layouts:

1. `01_NARRATIVE`
2. `02_ASSERTION_EVIDENCE`
3. `03_DECISION_ALGORITHM`
4. `04_MECHANISM`
5. `05_RETRIEVAL`
6. `06_SCENARIO`
7. `07_PRACTICE_HOLD`
8. `08_ACT5_GRAMMAR`
9. `09_BREAK_OPERATIONAL`
10. `10_FINAL`

The next production task is to construct these layouts one by one from this system, beginning with `01_NARRATIVE`.

---

# 10. Revision condition

This working presentation system should be revisited when either:

- Formalife platform P3 freezes the corporate brand system;
- projector/back-row rehearsal demonstrates a contrast/legibility problem;
- the real logo asset/clear-space system requires different footer geometry;
- full-course rehearsal shows that the Course Rail distracts during normal content rather than aiding orientation.

Until then, avoid manual per-slide styling that would make later retheming expensive.