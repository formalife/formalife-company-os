# B2C Funnel 01 — Choking / Weaning — Full Adaptive Architecture

Status: FOUNDER-DIRECTED PROVISIONAL TARGET OPERATING ARCHITECTURE — staged implementation required
Date: 2026-09-21

Purpose: define the complete direct-to-consumer Formalife funnel built around the initial choking / complementary-feeding wedge, including multiple entry points, awareness states, relationship states, educational progression, acceleration, backtracking, offers, upsells, cross-sells, downsells, automations, human escalation, post-sale, referral, tracking and economics.

This document supersedes `B2C_FUNNEL_01_CHOKING_WEANING_TO_FLAGSHIP.md` as the **complete target architecture** for Funnel 01. The earlier document remains useful as the narrower Block-1 implementation slice.

This document does **not** authorize building every component at once. `FORMALIFE_BUILD_SEQUENCE.md` remains canonical for execution order.

`Light / Serata Anti-Panico` is excluded from direct B2C. It remains a B2B/B2B2C partner-hosted product.

---

## 1. Founder direction incorporated

The Funnel 01 system must be sophisticated enough to serve people entering at materially different levels of readiness.

Founder direction now incorporated:

- multiple entry points are required;
- source does not determine awareness or readiness;
- less-ready prospects must be given enough time and enough useful material to become correctly educated before a strong sales ask;
- highly ready prospects must not be slowed down merely to make them respect a predefined funnel;
- the funnel must include explicit progression, acceleration, backtracking, recovery, upsell, cross-sell, downsell and long-horizon nurture;
- the system may be long, while each individual journey should be only as long as required by that person's state.

Core design principle:

**long system, variable individual path.**

---

## 2. Economic destination and scope

### Initial wedge

The strongest current Formalife evidence supports starting from:

**parent/caregiver entering or currently in complementary feeding / starting solids + material concern about pediatric choking.**

### Initial core economic destination

The first core monetization target remains:

**Live Full Course — broader pediatric safety / emergency preparedness.**

The Full Course is broader than choking. Choking is the acquisition wedge and one of the deepest practical blocks, not the entire meaning of Formalife.

### Mature Funnel 01 destination set

As the product system develops, Funnel 01 may monetize through:

- Guide / paid reference;
- Live Full Course;
- future Digital Emergency-Preparedness Flagship;
- future Hybrid Digital + Live bundle;
- future BLSD Formalife non-certified depth product;
- future private family/caregiver intensive;
- future refresh/reassessment;
- relevant specialist/lifecycle cross-sells only when a real need exists.

Current build order still limits what is implemented first.

---

## 3. Governing routing rules

### Rule A — state before step

The next action is selected from what the person currently knows, wants, has done and has already purchased.

### Rule B — source is not state

A person arriving from Google, Instagram, a pediatrician, a referral or a book may still be latent, problem-aware, solution-aware, product-aware or brand-aware.

### Rule C — materials educate before expensive selling

The lower the awareness and trust, the more work must be transferred to one-to-many educational material before expecting a high-commitment purchase.

### Rule D — every material has a next job

No article, video, guide, webinar, email or landing exists only to "create content".

Each piece must specify:

**state entering -> belief/understanding to create -> observable CTA -> next state.**

### Rule E — accelerate explicit intent

Explicit questions about dates, price, availability, Single/Couple, payment or course details can move a person directly to the appropriate offer/human help.

### Rule F — backtrack weak conversion

If a prospect is repeatedly exposed to a direct offer but does not advance, one hypothesis to test is that the funnel is speaking above their awareness level.

The system may move them backward from:

**product/offer -> solution/category education -> problem education -> waiting/timing state.**

### Rule G — do not equate technical activity with intent

Open, click, page view and video start are weak signals. Stronger evidence includes repeated meaningful consumption, explicit comparison, date/price interaction, checkout start, question, waitlist request or purchase.

### Rule H — sale suppresses acquisition

Once a product is purchased, the person exits acquisition for that same product and enters the relevant onboarding/post-sale state.

### Rule I — service problems override marketing

An unresolved service/support issue suppresses testimonial, referral, upsell and ordinary promotional automations until resolved.

### Rule J — no fake urgency

Only real course dates, seat capacity, valid expiry, shipping cutoffs or other genuine constraints may create deadlines.

---

## 4. State model — do not flatten everything into one tag

Routing should use several state dimensions rather than one generic `lead_status`.

### 4.1 Relationship state

- `UNKNOWN` — anonymous potential customer;
- `KNOWN_PROSPECT` — identified but no purchase;
- `ENGAGED_PROSPECT` — meaningful educational/offer interaction;
- `GUIDE_BUYER` — first paid information purchase;
- `FULL_CUSTOMER` — Full Course purchased;
- `ATTENDED_CUSTOMER` — Full Course delivered;
- `ADVOCATE` — satisfied and eligible for referral/proof requests;
- `PAST_CUSTOMER_WAITING` — no current relevant paid need;
- `LEAD_NO_SALE` — reached a genuine offer/decision point but did not buy;
- `SERVICE_RECOVERY` — unresolved issue overrides marketing;
- `DO_NOT_MARKET` — opt-out / permission or policy state prevents marketing contact.

### 4.2 Awareness state

Canonical working progression:

- `A0_UNAWARE_LATENT` — in target/life-stage but issue not yet salient;
- `A1_PROBLEM_AWARE` — recognizes choking concern / preparedness gap;
- `A2_SOLUTION_AWARE` — knows that training/reference/education can solve part of the problem;
- `A3_PRODUCT_AWARE` — evaluating a specific course/product category or Formalife product;
- `A4_BRAND_AWARE` — knows Formalife specifically and is evaluating whether to choose it.

### 4.3 Intent state

- `I0_PASSIVE`;
- `I1_ENGAGED`;
- `I2_ACTIVE_EVALUATION`;
- `I3_HIGH_INTENT`;
- `I4_CHECKOUT_INTENT`;
- `I5_PURCHASED`.

### 4.4 Trigger/context state

Store only what changes routing and is appropriate to collect:

- `STARTING_SOLIDS_SOON`;
- `STARTED_SOLIDS`;
- `CHOKING_CONCERN`;
- `AFTER_SCARE_OR_EVENT`;
- `SEARCHING_FOR_TRAINING`;
- `NEW_CAREGIVER_RESPONSIBILITY`;
- `RETURNING_PARENT_NEW_CHILD`;
- `OTHER_RELEVANT_CONTEXT`.

Do not collect unnecessary clinical details merely for marketing segmentation.

### 4.5 Timing/barrier state

- `READY_NOW`;
- `NEEDS_EDUCATION`;
- `TIMING_LATER`;
- `DATE_BARRIER`;
- `LOCATION_BARRIER`;
- `PRICE_VALUE_BARRIER`;
- `TRUST_PROOF_BARRIER`;
- `FORMAT_BARRIER`;
- `NON_FIT`.

### 4.6 Product state

- no purchase;
- Guide owned;
- Full owned;
- future Digital owned;
- future Hybrid owned;
- future depth/private/refresh product owned.

### Routing precedence

When states conflict, operational routing should normally prioritize:

1. service / permission / safety exception;
2. existing purchase and post-sale obligations;
3. checkout or explicit high intent;
4. explicit timing/barrier information;
5. awareness and education need;
6. source/campaign.

---

## 5. Entry-point architecture

Funnel 01 has multiple doors but converges into a controlled set of states and offers.

| Entry | Typical source | Likely initial state | First surface | Default job |
|---|---|---|---|---|
| EP01 | Brand/direct return | A4 + high intent | Full offer/date page | convert without delay |
| EP02 | Search: course / pediatric first aid / choking training | A2-A3 | solution/product landing | compare, prove, offer Full |
| EP03 | Search: choking concern / starting solids questions | A1-A2 | problem/solution resource | educate, route to F1/Guide/Full |
| EP04 | Customer referral | trust high; awareness variable | referral landing/router | borrow trust, assess readiness |
| EP05 | Professional/healthcare referral into central B2C | trust high; awareness variable | trust-transfer landing/router | route direct Full or education |
| EP06 | Organic/social life-stage content | A0-A1 | open trigger content | make problem relevant, earn next step |
| EP07 | Organic/social problem content | A1 | F0/F1 content | clarify problem and solution category |
| EP08 | Long-form video / YouTube / editorial | A1-A2 | content/VSL path | pre-educate before offer |
| EP09 | Guide direct discovery / publishing / QR | A2-A3 | Guide page | first paid relationship or direct Full |
| EP10 | Existing database lead | known state varies | resume-state campaign | continue, do not restart |
| EP11 | Prior Guide buyer | buyer + A2-A4 | Guide-to-core bridge | move to Full when relevant |
| EP12 | Previous customer / new child / new caregiver | customer + high trust | customer router | relevant next need, not acquisition from zero |
| EP13 | Retargeted/revisiting visitor | inferred only from real behavior | last-relevant stage | continue where evidence supports |
| EP14 | Offline material / event / physical QR | awareness varies | dedicated source page | identify source and route by state |

Important:

**entry source sets attribution; it does not automatically set awareness.**

A pediatrician referral can legitimately go straight to Full if ready, or into education if not.

---

## 6. Educational staircase — the long path for less-ready prospects

The funnel must give lower-readiness prospects enough time and enough material to understand the problem, the solution category and Formalife before asking repeatedly for a large commitment.

### E0 — Trigger relevance

Audience: `A0_UNAWARE_LATENT`.

Job:

- connect to a real life-stage moment such as starting solids;
- make pediatric safety preparedness relevant without fear inflation;
- earn attention for the next educational piece.

Formats:

- short social/video/editorial content;
- open article/resource;
- professional/referral content;
- stage-specific open checklist or tool once scientifically reviewed.

Primary CTA:

**consume the next useful resource / F1 asset.**

### E1 — Problem education

Audience: `A1_PROBLEM_AWARE`.

Job:

- clarify what the actual problem is;
- separate useful preparation from generic anxiety;
- make real consequences understandable and proportionate;
- establish that safe preparation is learnable without promising certainty.

Possible materials, all subject to scientific review where clinical:

- expert article/video;
- FAQ around common parent questions;
- myth/mistake clarification where evidence supports it;
- focused choking/start-solids mini-training.

Primary CTA:

**F1 opt-in / continue education / Guide / Full if already ready.**

### E2 — Solution-category education

Audience: `A2_SOLUTION_AWARE`.

Job:

- explain the distinct roles of information, reference and supervised practical training;
- teach the prospect how to evaluate credible training;
- explain why "watching something" and "being able to perform under supervised practice" are different jobs where scientifically appropriate;
- show when a reference book is useful and when practical training adds value.

Core asset candidate:

**M2 — Parent Buyer's Guide: how to choose pediatric choking / emergency-preparedness education.**

This is a marketing/editorial asset, not the paid Guide itself.

Primary CTA:

**choose the next format: paid Guide / Full Course / continue learning.**

### E3 — Product/category comparison

Audience: `A3_PRODUCT_AWARE`.

Job:

- answer "which type of course / provider / format should I choose?";
- make comparison criteria visible;
- show Formalife's actual operational difference once finalized;
- remove false comparability based only on hours/mannequins/price.

Materials:

- long-form article or VSL;
- comparison/criteria page;
- proof demonstrations;
- transparent scope page;
- FAQ.

Primary CTA:

**view Full Course offer/dates or buy Guide.**

### E4 — Formalife trust and offer education

Audience: `A4_BRAND_AWARE` or high-trust referral.

Job:

- show who is responsible for the education;
- show authentic proof;
- explain exact product scope and boundaries;
- answer objections;
- present price, Single/Couple, date, place, capacity and action clearly.

Primary CTA:

**book Full Course / select date.**

### E5 — Decision support

Audience: `I2-I4`.

Job:

- remove remaining uncertainty;
- answer a purchase question quickly;
- use real deadline/capacity only;
- recover an almost-buyer without automatically discounting.

Primary CTA:

**complete purchase / choose date / ask a specific question / join waitlist.**

---

## 7. Education time — operating defaults to test

These are **HYPOTHESIS / TEST CADENCES**, not universal doctrine.

The principle is that education time should expand when awareness is lower and compress when intent is explicit.

### Latent cohort

Suggested initial education horizon:

**approximately 21-45 days**, using multiple angles and formats rather than daily sales pressure.

Possible pattern:

- first 7 days: trigger/problem value;
- next 7-14 days: solution-category education;
- next 7-21 days: proof, criteria, Guide/Full invitation;
- if still not ready: move to long-horizon useful nurture.

### Problem-aware cohort

Suggested initial horizon:

**approximately 14-30 days**.

Use a substantive sequence before treating silence as rejection.

### Solution-aware cohort

Suggested initial horizon:

**approximately 7-14 days** with stronger comparison/proof/offering.

### Product/brand-aware or referral cohort

Can move immediately or within a short real decision window.

### Long-horizon nurture

If no purchase and no strong negative signal:

- slow the cadence;
- keep the material useful;
- re-enter a stronger commercial sequence when a real trigger, course-date response or explicit behavior appears.

The system must not keep a low-readiness prospect permanently inside a daily autoresponder.

---

## 8. Core content/material inventory for Funnel 01

Each material below has a distinct job.

### M0 — Open Trigger Content

Ungated.

Job: earn relevance from the starting-solids/choking life-stage trigger.

### M1 — Open Problem Resource

Ungated.

Job: answer one meaningful parent question well enough to demonstrate usefulness and earn the next interaction.

### F1 — Choking / Starting-Solids Readiness Mini-Training

Gated free asset — TARGET / HYPOTHESIS until clinically reviewed.

Recommended architecture:

- concise expert-led video or equivalent;
- printable/reference aid;
- explicit router to Guide, Full or continued learning.

Job:

**identified relationship + problem education + self-selection.**

### M2 — Solution / Buyer's Guide

Free long-form educational asset.

Job:

**teach how to choose the right level and type of preparation.**

### M3 — Formalife Difference / Proof Asset

Format may be long-form page, VSL, article or video.

Job:

- explain operational difference;
- show proof against the specific doubts;
- answer "why Formalife?".

The final difference is still test-gated and must not be invented by copy.

### P1 — Guida Anti-Panico al Soffocamento Pediatrico

Current paid information front-end.

Current canonical price: **EUR 19.90**.

Job:

- durable reference;
- first paid relationship;
- authority/publishing object;
- legitimate bridge to deeper practical preparedness.

### F2 — Online Parent Safety Briefing / Q&A

Status: OPTIONAL TARGET HYPOTHESIS — not required for V1 implementation.

Possible job:

- give slow-moving problem/solution-aware prospects a higher-engagement one-to-many experience;
- handle common objections/questions;
- move qualified participants toward Guide or Full.

Do not build it merely because a webinar makes the funnel look longer. It earns its place only if it increases economically useful conversion.

### O1 — Full Course Offer System

Includes:

- sales page;
- proof;
- scope;
- dates;
- FAQ;
- Single/Couple choice;
- checkout;
- human-help path;
- waitlist.

---

## 9. Offer ladder — present and future

### Free

- F0/M0/M1 open useful content;
- F1 gated mini-training;
- M2/M3 long-form education/proof;
- optional F2 briefing when validated.

### First paid relationship

- P1 Guide — EUR 19.90 current.

### Current core

- C2 Live Full Course — EUR 80 Single / EUR 120 Couple; maximum 12 participants; target 10-12 paying participants per edition.

### Planned future core

- C1 Digital Emergency-Preparedness Flagship;
- C3 Hybrid Digital + Live Bundle.

### Future depth / premium

- B1 BLSD Formalife non-certified;
- B2 Private Family / Caregiver Intensive;
- B3 additional caregiver/family-extension mechanics;
- R1 Practical Refresh / Reassessment.

### Horizontal specialist/lifecycle

Only when a real new need exists:

- Starting Solids / Feeding Safety specialist partnership;
- later Home Safety, Water Safety, Allergy, Car Seat, Child + Pet and other mapped verticals.

---

## 10. Route architecture by awareness

### Route R0 — latent / starting-solids cohort

```text
Life-stage trigger
-> M0 open content
-> M1 problem resource
-> F1 mini-training / identified lead
-> E1 problem sequence
-> E2 solution-category sequence
-> M2 buyer's guide
-> Guide OR Full OR continued nurture
-> E3/E4 proof + Formalife offer when ready
-> Full
```

This is deliberately the longest path.

### Route R1 — problem-aware

```text
Problem search/content
-> M1 or F1
-> E1 problem education
-> E2 solution education
-> Guide / M2 / Full
-> proof + offer
-> Full
```

### Route R2 — solution-aware

```text
Course-category search / comparison behavior
-> M2 solution/buyer education
-> M3 Formalife proof/difference
-> Full offer
-> checkout
```

Guide remains available as a lower-commitment paid reference, not a mandatory step.

### Route R3 — product-aware

```text
Formalife product page / retargeting / returning prospect
-> proof + FAQ + scope + offer
-> Full
```

### Route R4 — brand-aware / referral / high trust

```text
Referral / brand search / returning relationship
-> trust-transfer or Full offer page
-> date + Single/Couple
-> checkout
```

If the person signals lower awareness despite the source, route backward into education.

### Route R5 — Guide buyer

```text
Guide purchase
-> fulfilment
-> usage/value
-> information-vs-practice bridge
-> proof
-> Full offer
-> Full OR long nurture/wait
```

### Route R6 — previous no-sale

```text
Reason for no-sale
-> appropriate backtrack or barrier route
-> new education/proof/offer
-> Full OR timing wait / non-fit
```

### Route R7 — previous Formalife customer

Do not restart acquisition.

```text
customer history
-> actual new trigger / new child / caregiver change / depth intent
-> relevant next product
```

---

## 11. Acceleration rules

Move a person forward faster when there is strong evidence such as:

- explicit request for course dates/pricing;
- repeated Full Course page/date interaction;
- checkout start;
- direct question about Single/Couple or location;
- referral accompanied by explicit purchase intent;
- response indicating "I want supervised practice";
- Guide buyer actively requesting course information.

Acceleration actions can include:

- expose Full CTA immediately;
- skip lower-awareness sequence;
- suppress redundant education;
- create human follow-up task when useful and lawful.

Do not accelerate solely because an email was opened once.

---

## 12. Backtracking rules

Backtracking is diagnostic, not punitive.

### Product/offer exposure but no action

Test whether the missing step is:

- insufficient proof;
- weak offer/value;
- wrong date/location;
- insufficient category understanding;
- low problem priority;
- no current need.

Possible routing:

`A4/A3 -> A2` through solution/category education.

### Solution content but weak engagement

Possible routing:

`A2 -> A1` through problem/trigger education.

### Problem content but weak relevance

Move to:

`TIMING_LATER / long nurture / WAITING_FOR_TRIGGER`.

Do not keep increasing pressure.

### Returning after a new trigger

Reassess awareness and resume from the highest justified level rather than restarting from zero.

---

## 13. Guide architecture

### Guide checkout

Primary CTA: buy Guide.

Secondary visible route: if the prospect wants supervised practice now, go directly to Full.

### Candidate Guide order bump — future test only

**Additional caregiver / gift copy.**

Reasonable because it increases use around the same household problem.

Do not add unrelated merchandise merely to lift AOV.

### Training Credit

Existing asset, final rules still require standardization.

Strategic job:

**reduce the feeling of paying twice when moving from focused reference to a broader practical/core learning level.**

Candidate future applications may include Guide -> Live, Guide -> Digital or Guide -> Hybrid, but exact amount, expiry, stacking and eligibility remain decisions to finalize.

---

## 14. Current Full Course offer architecture

The live flagship must answer in sequence:

1. who it is for;
2. what problem/job it solves beyond choking;
3. why its format matters;
4. why Formalife is credible;
5. what is actually practiced/covered;
6. what is included;
7. what it is not;
8. price;
9. real date/capacity reason to act;
10. FAQ/human-help route.

Current commercial facts:

- EUR 80 Single;
- EUR 120 Couple;
- maximum 12 participants;
- target 10-12 paying participants;
- attendance certificate, not professional certification;
- Guide/materials included according to current architecture;
- rebuilt curriculum and customer-facing clinical claims remain subject to scientific validation.

### Full checkout upgrade

Single/Couple is the first natural value ladder.

If a Single buyer indicates another regular caregiver should also learn, offer the **Couple / second-caregiver option** clearly before or immediately after checkout where operationally feasible.

This is not a fake add-on. It solves the same household preparedness problem for another relevant adult.

---

## 15. Upsell architecture

Upsells must add real value along depth, completeness, convenience or caregiver coverage.

### U1 — Guide -> Full Live

Current primary ascension.

Trigger:

customer wants supervised practice and broader emergency preparedness.

### U2 — Single -> Couple / second caregiver

Current natural upgrade.

Trigger:

another primary caregiver needs to participate.

### U3 — future Digital -> Live / Hybrid

After the Digital product exists.

Trigger:

customer wants supervised practical correction in addition to digital learning.

### U4 — Full -> BLSD Formalife non-certified

Future.

Trigger:

explicit desire for deeper CPR/AED/resuscitation mastery and repeated practical work.

Not mandatory for every Full customer.

### U5 — Full/public format -> Private Family / Caregiver Intensive

Future premium hypothesis.

Trigger:

household wants customized convenience and multiple caregivers trained together.

### U6 — customer -> Refresh/Reassessment

Future.

Trigger:

skill decay, reduced confidence, new caregiver, new child/context or scientifically justified refresh need.

---

## 16. Cross-sell architecture

Cross-sell is activated by a distinct adjacent need, not simply because another SKU exists.

### X1 — Starting Solids / Feeding Safety

Status: specialist-partnership candidate.

Trigger:

parent needs feeding/safe-weaning guidance beyond choking response.

Potential role:

co-created or partner-led Svezzy-type product rather than internal duplication.

### X2 — caregiver extension

Can behave as cross-sell when the original buyer wants another household caregiver equipped but does not need a higher-level core product.

### X3 — future lifecycle verticals

Only with real triggers:

- Home Safety when mobility/home exposure changes;
- Water Safety when water exposure becomes relevant;
- Allergy Preparedness when a real allergy-related need exists;
- Car Seat when purchase/transition creates the need;
- other mapped verticals according to `LIFECYCLE_STATE_TIMELINE.md`.

No generic catalog blast.

---

## 17. Downsell / alternative architecture

Downsell is based on the real barrier; it is not automatic discounting.

### D1 — Full too much commitment / not ready

-> Guide.

### D2 — Full location barrier

Current:

-> Guide + waitlist / next-location notification.

Future after Digital exists:

-> Digital Core.

### D3 — Full date barrier

-> next-date waitlist / notification.

Do not offer a cheaper product if date is the only barrier.

### D4 — Full price/value uncertainty

-> value/proof/FAQ education first.

Only then consider Guide if the real issue is commitment level rather than misunderstood value.

### D5 — Guide not purchased

-> free education / long nurture.

### D6 — future Hybrid too much commitment

-> Digital or Live standalone based on the actual preference.

### D7 — future Private Family too expensive

-> public Full / Couple.

### D8 — future BLSD depth unnecessary

-> no forced sale; possible refresh only if a real refresh need exists.

---

## 18. Core automation map

The automations below define the mature Funnel 01 system. Not all are immediate build requirements.

### A00 — Anonymous / open-content continuation

Entry:

meaningful F0/M0/M1 consumption where lawful technical tracking exists.

Action:

- contextual next-content CTA;
- retargeting only where lawful/appropriate;
- no invented identity or intent.

Exit:

identified lead, direct Full action or inactivity.

### A01 — F1 delivery + progressive profile

Entry: `f1_optin`.

Immediate:

- deliver promised asset;
- capture source automatically;
- optionally ask one routing question only if answer changes next step;
- present visible Guide and Full paths for those already ready.

Exit:

Guide purchase, Full purchase, strong high-intent behavior, opt-out or education lane assignment.

### A02 — Latent education sequence

Entry: A0/weak A1 + valid permission.

Job:

- trigger relevance;
- problem understanding;
- priority without fear inflation;
- move toward F1/E1/E2.

Cadence: slower and longer; approximately 21-45-day initial test horizon.

Exit:

progressed awareness, explicit timing later, opt-out, or long nurture.

### A03 — Problem-aware education sequence

Entry: A1.

Job:

- problem clarity;
- consequences;
- safe preparation logic;
- move into solution/category education.

Cadence: approximately 14-30-day initial test horizon.

Exit:

Guide/Full interest, A2 state, timing wait or long nurture.

### A04 — Solution/category education sequence

Entry: A2.

Job:

- teach selection criteria;
- explain reference vs supervised practice;
- compare types of solution;
- introduce Formalife difference/proof.

Exit:

Guide, Full, A3/A4, backtrack or timing wait.

### A05 — Product/brand acceleration

Entry: A3/A4 + meaningful intent.

Job:

- scope;
- proof;
- objections;
- price/date;
- CTA.

Exit:

checkout, question/human task, Guide alternative, waitlist or backtrack.

### A06 — Guide checkout abandonment

Entry: `guide_checkout_start` and no purchase.

Actions:

- technical reminder;
- value/use case;
- answer shipping/payment questions;
- direct Full option remains available if practical training was the actual desired job.

No automatic discount.

### A07 — Guide buyer bridge

Entry: `guide_purchase`.

Sequence job:

1. fulfilment and usage;
2. establish value of the Guide itself;
3. explain distinct value of supervised practice/broader preparedness;
4. relevant proof;
5. Full offer / real course-date invitation;
6. long nurture if not ready.

Stop selling Guide to Guide buyers.

### A08 — Full offer engaged but no checkout

Entry:

meaningful Full page/date/pricing engagement but no checkout.

Job:

- product proof;
- FAQ;
- exact scope;
- decision support.

If repeated direct-offer exposure fails, test backtracking to A2 solution education.

### A09 — Full checkout recovery

Entry: `full_checkout_start = true`, no purchase.

Suggested test sequence:

- rapid technical reminder;
- proof/FAQ within the next decision window;
- ask the actual barrier;
- route by `DATE`, `LOCATION`, `NOT_READY`, `PRICE_VALUE`, `TRUST_PROOF`, `NEED_INFO`, `OTHER`.

Human task for explicit purchase questions/payment problems where lawful and operationally justified.

### A10 — Waitlist / timing automation

Entry:

DATE, LOCATION or TIMING_LATER barrier.

Job:

- stop repeating irrelevant dates;
- preserve relationship;
- notify when the stated condition changes;
- continue useful low-pressure education where permission exists.

### A11 — Full onboarding / pre-course

Entry: Full purchase.

Actions:

- receipt/confirmation;
- date/location/logistics;
- attendee details only as needed;
- preparation/reference material;
- reminder cadence;
- easy help route.

Suppress acquisition ads/sequences for Full.

### A12 — No-show / transfer recovery

Entry: booked but not attended.

Job:

- determine cause;
- apply approved transfer/cancellation rules;
- recover genuine attendance opportunity;
- do not request satisfaction/referral before delivery.

### A13 — Post-course experience

Entry: attended.

Actions:

- reference/next-actions appropriate to product;
- satisfaction/feedback;
- ask why they chose Formalife and what felt different;
- identify service issue versus positive outcome;
- identify caregiver/depth/lifecycle need without forcing an offer.

### A14 — Service recovery

Entry:

negative feedback, unresolved problem or support escalation.

Suppress:

- testimonials;
- referral requests;
- upsell/cross-sell promotion.

Exit only when issue is resolved/closed according to service process.

### A15 — Proof/testimonial automation

Entry:

positive experience and eligibility.

Job:

- ask for specific, authentic feedback/proof;
- connect testimonial to the actual delivered product/claim;
- preserve permission/usage records.

Historical choking-course proof must not be stretched into proof of undelivered rebuilt-flagship outcomes.

### A16 — Referral automation

Entry:

high satisfaction / advocate eligibility.

V1 mechanism:

- make it easy to share genuinely useful choking/start-solids material;
- track referrer -> referred lead -> purchase -> contribution.

Future tests may include Guide gifts, caregiver invitations or economically sustainable incentives.

### A17 — Caregiver expansion

Entry:

customer states that another regular caregiver needs preparation.

Routes:

- Couple/second seat;
- future digital family access;
- future private family intensive;
- gift/Guide where appropriate.

### A18 — Depth intent / BLSD future

Entry:

explicit desire for substantially deeper resuscitation practice.

Until B1 exists:

record `DEPTH_INTEREST` only; do not promise a product/date that is not available.

After launch:

route to BLSD Formalife.

### A19 — Lifecycle cross-sell future

Entry:

real new trigger.

Route to the relevant mini-funnel, not a generic catalog.

### A20 — Lead No Sale / reactivation

Entry:

prior genuine offer exposure or checkout without purchase.

Job:

- use last known barrier/state;
- choose backtrack, new proof, new date or timing reactivation;
- do not treat as a brand-new cold lead.

---

## 19. Human escalation rules

Create a human task when:

- explicit purchase question;
- payment/checkout failure;
- need help choosing date or Single/Couple;
- barrier does not map cleanly;
- valuable high-intent opportunity has stalled and human intervention is economically justified;
- complaint/service problem;
- customer-facing clinical question that cannot be answered from approved educational material.

Marketing automation must not act as emergency medical advice. Urgent health situations are outside the marketing funnel and must be directed according to scientifically/legal approved customer-support policy.

---

## 20. Suppression logic

### Global suppression

Do not run marketing sequence when:

- no valid marketing permission where required;
- opt-out/do-not-contact;
- unresolved service issue;
- legal/policy restriction.

### Product suppression

- Guide buyer: suppress Guide acquisition;
- Full buyer: suppress Full acquisition/abandonment;
- attended customer: suppress pre-course sales;
- future Digital buyer: suppress Digital acquisition;
- waitlist: suppress irrelevant date pressure.

### State suppression

When strong intent appears, suppress slower redundant education and accelerate.

When timing-later is explicit, suppress repeated immediate close attempts.

---

## 21. Funnel surfaces / pages

Minimum mature surface architecture:

1. `P0 Trigger Content / Content Hub`;
2. `P1 Problem Resource`;
3. `P2 F1 Opt-in Landing`;
4. `P3 F1 Thank-you / Router`;
5. `P4 Solution / Buyer's Guide`;
6. `P5 Formalife Difference + Proof / VSL`;
7. `P6 Guide Sales Page`;
8. `P7 Guide Checkout`;
9. `P8 Full Course Sales Page`;
10. `P9 Full Date / Single-Couple Selection`;
11. `P10 Full Checkout`;
12. `P11 Waitlist / Date-Location Preference`;
13. `P12 Purchase Confirmation / Onboarding`;
14. `P13 Feedback / Proof`;
15. `P14 Referral Share Page`;
16. future Digital/Hybrid/depth pages when products exist.

Do not send every traffic source to the same generic homepage.

---

## 22. Content machine by awareness state

Formalife should maintain content families, not one undifferentiated newsletter.

### A0 content — trigger/life stage

Job: make the issue relevant at the right moment.

### A1 content — problem

Job: clarify the real parent problem and consequences.

### A2 content — solution/category

Job: explain solution types, trade-offs and selection criteria.

### A3 content — product/category comparison

Job: help evaluate training/reference formats and provider criteria.

### A4 content — Formalife/proof

Job: show real difference, authority, delivery and offer.

### Customer content

Job:

- improve use/outcome;
- collect proof;
- referral;
- detect real new trigger;
- support next purchase without generic cross-selling.

Every content asset must define its next CTA before production.

---

## 23. Lead-status movement — graduated evidence

Do not use one simplistic points score as truth.

### Weak signals

- one email open;
- one page view;
- short video start;
- social like.

Use for context only.

### Medium signals

- repeated relevant content consumption;
- meaningful completion of F1;
- repeated category/offer page visits;
- Guide page/checkout engagement.

Can increase directness.

### Strong signals

- dates/pricing view;
- waitlist request;
- explicit question;
- checkout start;
- choice of Single/Couple;
- purchase.

Can trigger acceleration/human action.

---

## 24. Event taxonomy

Track at minimum where technically feasible:

### Discovery / education

- `content_view_meaningful`;
- `f1_view`;
- `f1_optin`;
- `f1_complete`;
- `buyers_guide_view`;
- `difference_proof_view`.

### Guide

- `guide_page_view`;
- `guide_checkout_start`;
- `guide_purchase`;
- `guide_fulfilled` where trackable.

### Full

- `full_page_view`;
- `full_date_view`;
- `full_checkout_start`;
- `full_purchase`;
- `full_attended`;
- `full_no_show`.

### Intent/barrier

- `question_submitted`;
- `waitlist_joined`;
- `barrier_selected`;
- `human_task_created`;
- `human_task_resolved`.

### Post-sale

- `feedback_submitted`;
- `service_issue_opened`;
- `service_issue_resolved`;
- `testimonial_received`;
- `referral_created`;
- `referred_purchase`;
- `caregiver_interest`;
- `depth_interest`;
- `lifecycle_trigger_recorded`.

---

## 25. CRM minimum fields

Maintain a single customer identity where possible.

Useful fields:

- person/customer ID;
- contact data and consent state;
- source / medium / campaign;
- referrer / partner ID when applicable;
- relationship state;
- awareness state when known/inferred with evidence;
- intent state;
- trigger/context state;
- timing/barrier state;
- products purchased and dates;
- course date/status;
- last meaningful activity;
- next action and due date for human tasks;
- no-sale reason where explicitly known;
- service issue state;
- referral relationships;
- revenue and contribution data where available.

Do not collect data merely because the CRM has a field for it.

---

## 26. Proof architecture by funnel stage

### Trigger/problem stage

Need to prove:

- information is credible;
- concern is treated proportionately;
- Formalife is useful before asking for money.

### Solution/category stage

Need to prove:

- the selection criteria are legitimate;
- different formats solve different jobs;
- supervised practical learning adds real value where claimed.

### Guide stage

Need to prove:

- substantive editorial/reference value;
- credible authorship/scientific provenance;
- physical/product fulfilment reliability.

### Full stage

Need to prove:

- human/scientific authority;
- authentic practical delivery;
- relevance to parents like the prospect;
- exact broader flagship scope;
- value of the format;
- current product-specific outcomes only after the rebuilt flagship has actually generated that proof.

### Post-sale

Delivery creates the next proof asset.

The loop is:

**claim -> delivery -> observed experience -> proof -> acquisition.**

---

## 27. Sales and offer recovery logic

### Almost buyer first

Before paying to generate another cold prospect, recover:

- Guide cart abandoners;
- Full cart abandoners;
- date/location waitlist;
- explicit no-sale opportunities;
- prior Guide buyers with no core purchase;
- returning high-intent leads.

### Multichannel where economics justify it

Potential channels:

- email;
- SMS/WhatsApp where authorized and appropriate;
- retargeting;
- human contact;
- physical Guide/material in selected economics.

Do not use every channel by default.

### Human selling

Human interaction should begin from context already transferred by marketing, not repeat generic education from zero.

---

## 28. Full funnel economics

The funnel is not judged by leads alone.

Read by entry point and cohort:

**source -> total acquisition cost -> identified lead -> first paid transaction -> Full conversion -> normalized contribution -> later purchase/referral contribution -> payback.**

### Current Full economics baseline

Current target:

- 10-12 paying participants per edition;
- EUR 80 Single / EUR 120 Couple.

Current normalized direct contribution before acquisition/overhead:

- 10 participants: approximately EUR 250-450;
- 12 participants: approximately EUR 352-592;

These are not CAC allowances by themselves.

### Funnel scorecard

Measure at least:

- cost per meaningful visitor where relevant;
- F1 opt-in rate by entry point;
- F1 -> Guide;
- F1 -> Full direct;
- Guide conversion rate;
- Guide -> Full conversion and time-to-Full;
- Full sales-page -> checkout;
- checkout -> purchase;
- abandonment recovery;
- waitlist -> purchase;
- direct high-intent -> Full;
- referral -> Full;
- Full fill rate / participant count;
- Single/Couple mix;
- no-show rate;
- normalized contribution per edition;
- complete CAC per Full customer by source/path;
- payback;
- satisfaction/service-recovery rate;
- testimonial/proof rate;
- referral creation and referred-customer contribution;
- future upsell/cross-sell attach rate;
- unsubscribe/complaint rate by nurture path;
- time spent in each awareness/timing state where observable.

---

## 29. Funnel decision rules

### If active-intent traffic converts

Protect the short lane. Do not force education merely to increase touchpoints.

### If active-intent traffic reaches Full but does not buy

Inspect:

- positioning/difference;
- proof;
- offer/value;
- date/location;
- checkout friction;
- category comparability.

Do not respond first by making the funnel longer.

### If latent/problem-aware traffic does not progress

Inspect:

- trigger relevance;
- educational quality;
- awareness mismatch;
- CTA size;
- trust/proof;
- timing.

Give the educational system enough time before declaring the audience bad.

### If Guide sells but Full conversion is weak

Determine whether Guide:

- attracts the wrong buyer;
- satisfies the need sufficiently by itself;
- fails to explain the distinct value of practice;
- creates no economic recovery;
- or simply needs a longer legitimate decision horizon.

Guide does not retain strategic importance merely because it already exists.

### If F2 briefing/webinar does not create incremental economic value

Do not maintain it as ceremony.

### If cold acquisition works only through discounts

Re-examine offer, positioning, proof and target before scaling.

---

## 30. Staged implementation — architecture is complete, build is progressive

The full funnel above is the **target operating design**. Implementation still follows causal order.

### Stage 0 — prerequisites

- scientific review of flagship curriculum/claims;
- final authority wording;
- Full offer/scope clarity;
- Training Credit rules;
- Guide economics;
- checkout/attribution audit;
- CRM identity and consent states;
- basic proof inventory.

### Stage 1 — short high-intent engine

Build/verify:

- EP01/EP02/EP04/EP05 high-intent entries;
- Full page/date/checkout;
- Guide optional route;
- Guide buyer bridge;
- Full abandonment recovery;
- onboarding;
- post-course feedback/proof/referral.

### Stage 2 — problem-aware engine

Build:

- M1/F1;
- A03 problem sequence;
- M2 solution education;
- Guide/Full routing;
- long nurture and backtracking.

### Stage 3 — latent demand engine

Build:

- M0 trigger content;
- A02 longer education;
- social/editorial/SEO entry segmentation;
- education across 21-45-day test horizons before broad conclusions.

### Stage 4 — deeper one-to-many education

Only if needed by evidence:

- F2 briefing/Q&A;
- richer VSL/long-form materials;
- selected physical/material follow-up.

### Stage 5 — paid cold scaling

Only after downstream economics are readable.

### Stage 6 — future Digital / Hybrid

After Block 3 of `FORMALIFE_BUILD_SEQUENCE.md` becomes active.

### Stage 7 — depth, lifecycle and broader back-end

BLSD Formalife, private family, refresh and adjacent product mini-funnels only after the core mechanism justifies them.

---

## 31. What not to do

- do not force every lead through the same number of steps;
- do not send latent prospects directly into repeated Full sales asks before educating them;
- do not force ready prospects through F1 or Guide;
- do not treat a click as proof of high intent;
- do not create fake deadlines or fear;
- do not turn clinical marketing content into unreviewed medical claims;
- do not use the B2B/B2B2C Light inside direct B2C;
- do not discount automatically after abandonment;
- do not send the same nurture forever;
- do not restart old leads from zero if their history is known;
- do not send acquisition ads for products already purchased;
- do not request referrals/testimonials while a service issue is unresolved;
- do not cross-sell every Formalife topic to every customer;
- do not build F2, Digital, membership or other layers merely to make the funnel look sophisticated;
- do not scale traffic before measuring conversion, contribution, CAC and delivery capacity.

---

## 32. Master funnel — simplified map

```text
                         CHOKING / STARTING-SOLIDS MARKET
                                      |
         +----------------------------+-----------------------------+
         |                            |                             |
      LATENT                      PROBLEM-AWARE               ACTIVE / HIGH INTENT
       A0/I0                         A1/I1                    A2-A4 / I2-I4
         |                            |                             |
   M0 trigger content          M1 / F1 education            Search / referral / brand
         |                            |                             |
   problem relevance            problem clarity                     |
         |                            |                             |
        F1 ----------------------> E2 solution education             |
         |                            |                             |
         |                        M2 buyer guide                     |
         |                            |                             |
         +-------------------+--------+----------+                  |
                             |                   |                  |
                           GUIDE            M3 PROOF / DIFFERENCE <-+
                             |                   |
                       Guide bridge              |
                             +-------------------+
                                      |
                              FULL COURSE OFFER
                                      |
                         +------------+------------+
                         |                         |
                      PURCHASE                   NO SALE
                         |                         |
                    ONBOARDING             +------+-------+
                         |                  |      |       |
                     ATTEND             barrier  wait   backtrack
                         |                  |      |       |
             +-----------+-----------+      |      |       |
             |           |           |      |      |       |
          positive     issue       no-show  |      |       |
             |           |           |      |      |       |
          proof       service      recovery +------+-------+
             |
          referral
             |
      +------+----------------------------+
      |                                   |
 caregiver/depth need              no current need
      |                                   |
 future upsell/cross-sell             WAITING FOR TRIGGER
```

---

## 33. Canonical Layer 1 references

Primary doctrine used for this architecture:

- `REASONING_KERNEL.md`;
- `merenda/01_mercato/clienti-identificabili-e-target.md`;
- `merenda/03_offerta/offerta-a-risposta-diretta.md`;
- `merenda/03_offerta/front-end-e-back-end.md`;
- `merenda/04_marketing/gerarchia-domanda-e-canali.md`;
- `merenda/04_marketing/complessita-e-riduzione-variabili.md`;
- `merenda/05_acquisizione/funnel-e-conversione.md`;
- `merenda/05_acquisizione/information-marketing.md`;
- `merenda/05_acquisizione/database-email-e-sequenze.md`;
- `merenda/05_acquisizione/referral-e-soddisfazione.md`;
- `merenda/06_vendita/follow-up-lead-non-convertiti.md`;
- `merenda/07_copy_comunicazione/priorita-azione-e-inerzia.md`;
- `merenda/09_business/numeri-cassa-e-crescita.md`.

## 34. Related Layer 2 files

- `CURRENT_STATE.md`;
- `B2C_COMMERCIAL_FUNNEL_V2.md`;
- `B2C_FUNNEL_01_CHOKING_WEANING_TO_FLAGSHIP.md` — narrower prior implementation slice;
- `FORMALIFE_BUILD_SEQUENCE.md`;
- `FLAGSHIP_SCOPE_V1.md`;
- `ASSET_INVENTORY.md`;
- `MARKET_EVIDENCE.md`;
- `LIFECYCLE_STATE_TIMELINE.md`;
- `LIFECYCLE_FUNNEL_PATHS.md`.

## 35. Revision conditions

Revise this architecture when real Funnel 01 data shows one or more of the following:

- an awareness path is materially misclassified;
- a material repeatedly fails to move the intended state;
- a shorter path produces better economics without harming fit/outcome;
- a longer education path materially improves Full conversion/payback;
- Guide does not create incremental economic/customer value;
- a new product changes the rational routing;
- customer feedback shows that the choking -> broader preparedness bridge is misunderstood;
- operational capacity makes a path uneconomic;
- scientific/legal review changes permitted content or claims.

The objective is not to preserve this diagram. It is to create a measurable customer-acquisition and relationship system that becomes more accurate as evidence accumulates.
