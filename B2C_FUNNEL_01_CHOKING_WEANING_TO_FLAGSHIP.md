# B2C Funnel 01 — Choking / Weaning -> Formalife Live Flagship

Status: PROVISIONAL OPERATING SPEC — founder-requested; test-gated
Date: 2026-09-21

Purpose: instantiate the first complete direct-to-consumer Formalife funnel as an executable state machine, from first demand signal through purchase, delivery, recovery, referral and exit into the next customer state.

This funnel implements only Block 1 of `FORMALIFE_BUILD_SEQUENCE.md`. It deliberately does **not** depend on the future Digital Flagship, broad lifecycle automation, membership, BLSD Formalife or the partner-hosted Light product.

`Light / Serata Anti-Panico` belongs to the B2B/B2B2C partner-hosted lane and is outside this funnel.

## 1. Funnel decision

The first B2C funnel is built around the strongest currently evidenced trigger:

**parent entering or currently in complementary feeding / starting solids + material concern about pediatric choking.**

The economic destination of Funnel 01 is the broader Formalife **Live Full Course — pediatric safety / emergency preparedness**, while the choking-specific Guide can act as an optional paid information front-end.

The funnel is not a mandatory staircase.

Two principal purchase routes coexist:

### Route A — high intent

**active demand / referral / returning prospect -> Full Course sales page -> checkout -> Live Full Course**

### Route B — problem-aware / lower readiness

**trigger-led content -> focused lead asset -> owned contact -> Guide OR Full Course -> follow-up -> Full Course when appropriate**

The customer can jump forward whenever explicit intent justifies it. A Guide buyer is not forced to buy the Full Course. A high-intent prospect is not forced to buy the Guide first.

## 2. Evidence, decisions and open assumptions

### FORMALIFE EVIDENCE

- choking fear/concern was common among historical B2C participants;
- founder estimates approximately 50-60% were in the weaning/complementary-feeding phase;
- the active-demand vs latent-demand mix is unknown and must be measured prospectively;
- Formalife owns a physical choking Guide priced at EUR 19.90;
- current live flagship commercial target is EUR 80 Single / EUR 120 Couple, maximum 12 participants and target 10-12 paying participants per edition;
- current normalized direct contribution before acquisition/overhead is approximately EUR 250-450 at 10 participants and EUR 352-592 at 12 participants, depending on Single/Couple mix;
- WordPress, Stripe, Brevo, GTM/UTM design, Gmail, Calendar and operating sheets exist, although end-to-end course checkout/attribution still requires verification;
- Formalife has authentic delivery photography/proof and prior customer reviews/testimonials, but proof from the old choking course must not be stretched to unsupported claims about the materially rebuilt flagship.

### FOUNDER DECISIONS

- choking/weaning is the first B2C acquisition wedge;
- Guide is an optional B2C paid information front-end;
- Full Course is the broader parent-first pediatric safety/emergency-preparedness live flagship;
- high-intent B2C prospects can buy Full directly;
- Light is excluded from direct B2C and belongs to B2B/B2B2C;
- the funnel must be designed around customer state and economics, not around protecting current assets.

### OPEN / TEST-GATED

- exact final market-facing differentiation of the rebuilt Full Course;
- scientific validation of the rebuilt Full Course curriculum and claims;
- final Training Credit rules;
- Guide unit economics including printing, fulfillment, payment and support;
- actual B2C direct-channel CAC;
- usable marketing-consent/database baseline;
- exact best-performing lead asset, message and channel mix.

These open items do not authorize invented customer-facing claims.

## 3. Economic job

Funnel 01 must prove that Formalife can turn a choking/weaning demand signal into **measurable contribution**, not merely leads.

Primary economic chain:

**source -> identifiable prospect or direct buyer -> first transaction -> Full Course buyer -> delivered customer -> proof/referral -> contribution -> payback.**

The first funnel must answer:

1. which sources produce buyers, not only clicks;
2. whether direct Full purchase works for high-intent demand;
3. whether the Guide creates incremental Full buyers or contribution rather than just another transaction;
4. where non-buyers drop out and why;
5. whether delivery produces enough satisfaction/proof/referral to strengthen the next acquisition cycle;
6. whether the complete customer economics justify adding cold traffic later.

## 4. Funnel map

```text
DEMAND / TRIGGER
    |
    +--> HIGH INTENT ------------------------------------+
    |                                                    |
    |     Full Sales Page -> Checkout -> Full Purchase --+--> PRE-COURSE
    |                        |                            |
    |                        +-> Abandon Recovery --------+
    |
    +--> PROBLEM-AWARE
          |
          +-> Trigger Content / Landing
                |
                +-> F1 Focused Lead Asset
                      |
                      +-> explicit route: "I want practice / course" -> Full Sales Page
                      |
                      +-> explicit route: "I want a home reference" -> Guide Sales Page
                      |                                           |
                      |                                           +-> Guide Purchase
                      |                                                 |
                      |                                                 +-> Guide Fulfillment
                      |                                                 +-> Full Bridge Sequence
                      |                                                 +-> Full Sales Page
                      |
                      +-> no paid step -> Nurture / Wait / Re-route

FULL PURCHASE
    |
    +-> confirmation + logistics + reminders
    +-> attendance
          |
          +-> positive experience -> feedback -> proof/testimonial -> referral
          |
          +-> issue / dissatisfaction -> service recovery
          |
          +-> no current next need -> WAITING_FOR_TRIGGER
          |
          +-> explicit future deeper-skill interest -> future-product interest state only
```

## 5. Source architecture

Source and customer state are stored separately.

### S1 — Direct active-intent demand

Examples:

- relevant Google/search intent;
- branded search;
- direct site return;
- consumer referral / word of mouth;
- previous prospect returning for date/price.

Routing default:

**Full Course sales page.**

Do not force F1 or Guide if the prospect is already ready to evaluate the flagship.

### S2 — Problem-aware discovery

Examples:

- organic/social content around starting solids and choking concern;
- problem-aware paid social only after downstream economics are legible;
- editorial/search content with educational rather than immediate purchase intent.

Routing default:

**F1 focused lead asset**, with visible direct access to the Full Course for those already ready.

### S3 — Existing owned relationship

Examples:

- prior Guide buyer;
- prior lead with lawful permission;
- prior customer/referrer introducing a parent in the trigger window.

Routing depends on current state, not list membership.

### Source activation priority

For Funnel 01:

1. existing direct/referral/owned demand that can be measured;
2. active-intent search capture;
3. only after downstream conversion is readable, problem-aware paid traffic;
4. broad interruptive paid demand only after the offer/funnel proves it can recover acquisition cost.

This is a sequencing rule, not a claim that a specific media will win.

## 6. Stage F1 — focused lead asset

### Working internal offer

**F1 — Choking / Starting-Solids Readiness Mini-Training**

Status: HYPOTHESIS / BUILD CANDIDATE. Final customer-facing name and all clinical content require scientific review.

Recommended V1 format:

- short expert-led video or equivalent focused educational asset;
- concise printable/reference checklist;
- one clear next-step router.

Its job is **not** to teach the whole paid course for free.

Its job is to:

- make the starting-solids/choking concern concrete and useful without fear inflation;
- demonstrate scientific/human credibility;
- help the parent understand the difference between information/reference and supervised practical preparedness;
- identify whether the next appropriate step is Guide, Full Course or no paid step yet.

### F1 opt-in data

Required only when needed for delivery:

- email;
- first name if operationally useful.

Progressive/optional fields only if they change routing:

- caregiver role;
- broad trigger stage such as `starting solids soon / already started / other`;
- source/campaign, captured automatically where possible.

Marketing permission / consent state must remain explicit and separate from mere technical ability to send a transactional delivery message.

### F1 landing-page job

One observable primary CTA:

**get/access the focused mini-training.**

The page should contain, once claims are approved:

- trigger-specific relevance;
- what the asset helps the parent understand;
- visible human/scientific authority;
- proof that the asset is substantive, not generic bait;
- simple data capture;
- no fake timer or invented emergency.

### F1 thank-you/router page

Immediately after opt-in, present two legitimate self-selection paths:

1. **"I want a complete reference to keep at home" -> Guide**;
2. **"I want supervised practice and broader emergency preparedness" -> Full Course**.

The Full Course route must not be hidden merely to protect the Guide front-end.

## 7. Stage P1 — Guide paid front-end

### Current offer

**Guida Anti-Panico al Soffocamento Pediatrico — EUR 19.90.**

### Job

- create a first paid relationship;
- deliver real standalone reference value;
- demonstrate publishing/authority quality;
- identify buyers willing to invest in preparedness;
- create a legitimate bridge into supervised practice and broader preparedness.

### Guide sales-page requirements

Primary CTA:

**buy the Guide.**

Must make clear:

- what the Guide is for;
- what the buyer receives;
- physical format / fulfillment expectations;
- price;
- scientific/editorial provenance that Formalife is authorized to claim;
- what a written reference can and cannot replace relative to supervised hands-on practice;
- Training Credit only after the rules are standardized;
- direct Full Course route for a buyer who is already ready.

### V1 order bump decision

**Do not add an order bump in the first controlled test.**

Reason: Guide -> Full economics are not yet known. Adding a second variable before the basic front-end is legible makes diagnosis harder.

A second-caregiver copy/family pack can be tested later if the base Guide funnel works.

### Guide checkout events

Track at least:

- `guide_page_view`;
- `guide_checkout_start`;
- `guide_purchase`;
- source/campaign/referrer;
- gross revenue and complete variable fulfillment cost when available.

## 8. Guide buyer automation

Automation ID: **A-GUIDE-01**

### Entry

`guide_purchase = true`

### Immediate actions

- move relationship state to `GUIDE_BUYER`;
- stop Guide acquisition/abandon sequences;
- send receipt/fulfillment information;
- record source and purchase;
- trigger shipping/fulfillment workflow;
- expose the Full Course as a different next job, not as the part deliberately omitted from the book.

### Suggested operating sequence — TEST CADENCE

Timing is an operating default to test, not Layer 1 doctrine.

**T0 — purchase**

- confirmation;
- fulfillment expectation;
- how to use the Guide as reference;
- clear explanation of Training Credit only if final rules are live.

**T+2 days**

Job: distinguish information from supervised practice.

Message function:

- the Guide is useful reference;
- motor practice, correction and broader emergency preparedness are a different job;
- show what happens in the Full Course rather than claiming the book is insufficient.

CTA: view Full Course dates/details.

**T+5 days**

Job: proof and relevance.

- relevant parent story/testimonial;
- authentic practical-delivery evidence;
- Full Course customer job;
- CTA to next available date.

**T+8 days or tied to a real course-date decision window**

Job: decision.

- FAQ/objection handling;
- real capacity/date scarcity only;
- Training Credit reminder if valid and standardized;
- CTA to book.

### Exit conditions

Exit immediately if:

- Full Course purchased;
- customer opts out;
- customer explicitly says timing is wrong and gives a later relevant horizon;
- support/service issue is open.

### No conversion

Move to `GUIDE_BUYER_NO_CORE` and maintain low-pressure useful communication. Do not repeatedly resend the same course pitch.

## 9. Stage C2 — Live Full Course sales system

### Current offer facts

- broader pediatric safety/emergency-preparedness live flagship;
- approximately 4 hours historically/currently, subject to final delivery design;
- EUR 80 Single;
- EUR 120 Couple;
- maximum 12 participants;
- target 10-12 paying participants per edition;
- attendance certificate, not professional certification;
- Guide(s) currently included in the live offer architecture;
- scientific validation of rebuilt curriculum/claims remains a pre-launch gate.

### Full Course sales-page job

Primary CTA:

**select/book the appropriate next course date.**

The page must answer, in a deliberate order:

1. **Is this for a parent like me?** — trigger and target specificity.
2. **What problem does it solve beyond choking?** — broader preparedness job.
3. **Why Formalife rather than a credible alternative?** — real operational difference once finalized.
4. **Can I believe you?** — authority and relevant proof early enough.
5. **What will I actually do?** — practical format, supervised work, broad scope boundary.
6. **What do I receive?** — components, Guide/materials, duration/date/location.
7. **What is it not?** — not professional certification; not a diagnostic pediatrics course.
8. **What does it cost?** — EUR 80 Single / EUR 120 Couple.
9. **Why decide now?** — next real date and actual 12-person capacity only.
10. **What if I still have a doubt?** — FAQ and simple human-help route.

### Proof placement

Use proof against the exact risk:

- scientific credibility -> verified bios/governance;
- practical format -> authentic class/practice imagery;
- relevance -> parent testimonials with similar trigger/context;
- rebuilt broader flagship -> only proof generated by that actual rebuilt product for claims about its broader value.

Historical choking-course reviews can support continuity of delivery credibility but must not be represented as proof of outcomes never delivered in the rebuilt product.

## 10. Full Course checkout

Minimum checkout path:

1. select date;
2. choose `Single` or `Couple`;
3. show exact amount and inclusions;
4. collect only operationally necessary attendee/contact fields;
5. payment;
6. confirmation.

Avoid optional fields that do not change delivery, routing or analysis.

### Required commercial attribution

Capture where possible:

- source / medium / campaign;
- referrer/customer referral ID where relevant;
- entry path: `DIRECT_FULL`, `F1_TO_FULL`, `GUIDE_TO_FULL`, `RETURNING`;
- broad trigger/life-stage if voluntarily provided;
- stated primary reason for buying / choosing Formalife, captured at checkout or post-purchase without increasing payment friction excessively.

## 11. Checkout-abandonment automation

Automation ID: **A-FULL-ABANDON-01**

### Entry

`full_checkout_start = true` AND `full_purchase = false`

### Operating default — TEST CADENCE

**T+30-60 minutes**

Email/message function:

- technical reminder;
- preserve selected date where technically possible;
- restate price/inclusions;
- route to help if payment/process problem.

**T+24 hours**

Function:

- answer top decision objections;
- relevant proof;
- course date/capacity;
- CTA to complete purchase.

**T+48 hours**

Function:

- ask the barrier rather than automatically discounting.

Suggested one-click classifications:

- date does not work;
- location/geography;
- not ready yet;
- price/value uncertainty;
- want to understand the course better;
- other question.

### Barrier routing

`DATE` -> waitlist / next-date notification.

`LOCATION` -> wait for appropriate local offer; Guide can be offered only if useful for the person's current job. Future Digital is not sold until it exists.

`NOT_READY` -> problem-aware nurture / waiting state.

`PRICE_VALUE` -> proof/value/FAQ path; do not default to discount.

`NEED_INFO` -> human/help FAQ route.

`OTHER` -> human review where economically reasonable.

### Human escalation

Create a human task when the person:

- asks an explicit purchase question;
- reports payment failure;
- requests help choosing Single/Couple or date;
- provides a barrier that cannot be resolved automatically.

Operating SLA target: **same business day**, subject to capacity review. Tighten only after the process is reliably staffed.

## 12. Full purchase / onboarding automation

Automation ID: **A-FULL-ONBOARD-01**

### Entry

`full_purchase = true`

### Immediate actions

- state -> `FULL_CUSTOMER_CONFIRMED`;
- stop acquisition, Guide-upgrade and cart-recovery messages for the purchased course;
- write product/date/Single-Couple/source to customer record;
- send payment/order confirmation;
- send date/location/duration/logistics;
- state what attendance certificate is and is not;
- give contact route for practical questions;
- create attendance roster entry.

### Reminder sequence — OPERATING DEFAULT

Adapt automatically if the purchase occurs inside the normal reminder windows.

**T-7 days**

- logistics;
- what to expect;
- what to bring if anything is required;
- rescheduling/cancellation terms once canonical.

**T-2 days**

- concise reminder;
- location/time;
- attendance expectations.

**T-3 hours**

- short operational reminder only where useful and consent/channel allows.

No reminder should introduce new clinical claims not already validated.

## 13. Attendance branches

### Attended

Event:

`full_attended = true`

State -> `FULL_ATTENDED`

Enter post-delivery system.

### No-show

Event:

`full_no_show = true`

State -> `FULL_NO_SHOW`

Actions:

- stop post-course testimonial/referral automation;
- send a practical check-in;
- apply the canonical rebooking/cancellation policy once defined;
- create human task when the case needs judgment;
- measure no-show reason.

Do not improvise refunds or credits outside defined terms.

## 14. Post-delivery automation

Automation ID: **A-POSTCOURSE-01**

### T0 — same day

- thanks;
- reference/material access where applicable;
- clarify how to use provided materials;
- support route.

### T+1 day — structured feedback

Collect:

- satisfaction rating;
- what was most valuable;
- what could have been better;
- why the person originally chose Formalife;
- what they perceived as different versus alternatives;
- whether another caregiver should also be prepared;
- whether there is already another distinct safety/preparedness need.

The "why did you choose Formalife?" field is strategically important because current positioning difference remains an open evidence problem.

### Satisfaction branch — OPERATING DEFAULT

Use the following as a workflow threshold to test, not a universal doctrine benchmark.

**9-10 / explicitly very positive** -> `ADVOCATE_ELIGIBLE`.

**0-8 or qualitative issue** -> `SERVICE_REVIEW`.

### Service-review branch

- do not ask for referral/review while a material issue is unresolved;
- human owner reviews the feedback;
- resolve legitimate delivery/service defects;
- record cause and corrective action;
- only after recovery, decide whether a review/referral request is appropriate.

This protects proof quality: reputation follows delivery rather than replacing it.

## 15. Testimonial / review workflow

Automation ID: **A-PROOF-01**

Entry:

`ADVOCATE_ELIGIBLE`

Sequence:

1. request specific authentic feedback while the experience is fresh;
2. make the response mechanism simple;
3. when useful, ask structured questions that elicit specificity without scripting false claims;
4. record consent/permission for any testimonial media use;
5. classify the proof by the objection/claim it supports.

Useful proof tags:

- `TRIGGER_WEANING_CHOKING`;
- `PRACTICAL_FORMAT`;
- `BROADER_PREPAREDNESS`;
- `COUPLE_CAREGIVER`;
- `AUTHORITY_TRUST`;
- `PURCHASE_DECISION_REASON`.

Do not reduce the proof library to a pile of generic five-star comments.

## 16. Referral workflow

Automation ID: **A-REFERRAL-01**

Entry:

`ADVOCATE_ELIGIBLE` AND no open service issue.

### V1 referral mechanism

Start with **no financial incentive** to reduce variables.

Primary referral action:

**share the useful F1 Choking / Starting-Solids Mini-Training with another parent/caregiver in the relevant life stage.**

Why this is preferred for V1:

- it is easy for the customer to share;
- it does not force the referrer to sell a EUR 80/120 course;
- it protects the referrer's reputation by offering useful value first;
- it creates traceable entry into the same funnel.

Track where possible:

`referrer_id -> referred lead -> Guide/Full purchase -> contribution.`

Later test only if economics justify:

- gift Guide;
- referral credit;
- caregiver benefit;
- other incentive.

## 17. Non-buyer nurture

Automation ID: **A-LEAD-NURTURE-01**

Entry:

F1 lead who has not purchased Guide or Full.

The sequence should not be an endless newsletter and should not repeat the same pitch.

### Suggested minimum sequence — TEST CADENCE

**Message 1 — immediate**

- deliver promised asset;
- one next-step router: Guide vs Full vs continue learning.

**Message 2 — +2 days**

- deepen the real trigger/problem;
- correct one relevant misconception only after scientific approval;
- explain when information is enough and when supervised practice serves a different job.

**Message 3 — +4 days**

- proof/authority;
- show the live experience;
- explain broader preparedness beyond choking;
- CTA to Full Course.

**Message 4 — +7 days**

- address common objections / alternatives;
- Guide option for those preferring reference-first;
- direct Full CTA for ready prospects.

**Message 5 — tied to a real next-course window**

- next real date;
- actual remaining capacity if reliably known;
- CTA.

### No response after sequence

State -> `LEAD_WAITING_TIMING`.

Move into low-frequency useful communication until:

- explicit new intent;
- new course/date interaction;
- trigger update;
- unsubscribe.

Do not infer strong buying intent from an email open alone.

## 18. CRM / state machine

Minimum relationship states for Funnel 01:

- `ANONYMOUS_VISITOR`;
- `LEAD_CHOKING_WEANING`;
- `LEAD_FULL_HIGH_INTENT`;
- `GUIDE_CHECKOUT_STARTED`;
- `GUIDE_BUYER`;
- `GUIDE_BUYER_NO_CORE`;
- `FULL_CHECKOUT_STARTED`;
- `FULL_CUSTOMER_CONFIRMED`;
- `FULL_NO_SHOW`;
- `FULL_ATTENDED`;
- `SERVICE_REVIEW`;
- `ADVOCATE_ELIGIBLE`;
- `LEAD_WAITING_TIMING`;
- `WAITLIST_DATE_LOCATION`;
- `WAITING_FOR_TRIGGER`;
- `UNSUBSCRIBED / CONTACT_RESTRICTED` where relevant.

### State transition principle

**event / explicit signal -> state update -> next action -> result -> new state.**

Technical behavior is evidence, not certainty. An open, click or page view can adjust priority but should not by itself be treated as a purchase decision.

## 19. Minimum customer record

Store only data that changes service, routing or economics.

Minimum useful fields:

- customer/prospect ID;
- email;
- phone only where voluntarily provided and operationally needed;
- marketing/communication consent state;
- source / medium / campaign;
- referral/referrer ID where relevant;
- entry path;
- current relationship state;
- current trigger / broad life-stage when voluntarily provided;
- Guide purchase/date;
- Full purchase/date/Single-Couple;
- course edition / attendance;
- checkout-abandon reason if known;
- satisfaction / service issue;
- reason for choosing Formalife;
- perceived difference;
- referral activity;
- next action / next-action date;
- last meaningful activity.

Do not collect detailed family data that does not change an actual decision.

## 20. Automation suppression rules

Every automation must have explicit suppression.

### Global rules

- never promote a product already purchased as if the purchase did not exist;
- open service issue suppresses upsell/referral until resolved;
- Full purchase suppresses Guide->Full and Full-abandon campaigns;
- unsubscribe/contact restriction suppresses marketing communication according to the allowed state;
- no-show suppresses normal attendee post-course messages;
- customer with no current next need enters `WAITING_FOR_TRIGGER` rather than forced cross-sell;
- an explicit human conversation can override automation when documented in the customer record.

## 21. Human owner rules

Automation handles repeatable information. Humans handle ambiguity and high-value friction.

Human task required for:

- explicit sales question not answered by standard FAQ;
- payment/checkout failure needing intervention;
- cancellation/reschedule exception;
- material complaint/service recovery;
- ambiguous barrier from a hot prospect;
- scientific/clinical question beyond approved customer-service knowledge;
- testimonial/media permission requiring clarification.

Clinical questions outside approved educational scope must be escalated appropriately; marketing automation must not improvise medical advice.

## 22. Page / asset inventory required for Funnel 01

### Must exist before controlled launch

1. F1 focused lead-asset landing page.
2. F1 thank-you/router page.
3. F1 asset itself, scientifically reviewed.
4. Guide sales page.
5. Guide checkout + fulfillment flow.
6. Full Course sales page.
7. Full Course date/Single-Couple checkout.
8. Full Course confirmation/onboarding page.
9. waitlist/date-location page or equivalent workflow.
10. post-course feedback page/form.
11. testimonial/review request flow.
12. referral-share page/link.
13. core email templates for the automations above.
14. internal funnel dashboard / operating sheet.

### Technical systems already available as assets

Potential implementation stack already in Formalife:

- WordPress for pages;
- Stripe for payment;
- Brevo for email/automation where suitable;
- GTM/UTM architecture for attribution;
- Gmail for human follow-up;
- Google Calendar for course operations;
- operating sheets for edition-level economics and exceptions.

The exact software implementation is subordinate to the state logic. Do not automate undefined processes merely because the tools exist.

## 23. Event and measurement specification

Minimum funnel events:

- `session_source_captured`;
- `f1_optin`;
- `f1_route_guide_click`;
- `f1_route_full_click`;
- `guide_page_view`;
- `guide_checkout_start`;
- `guide_purchase`;
- `full_page_view`;
- `full_checkout_start`;
- `full_purchase`;
- `full_attended`;
- `full_no_show`;
- `feedback_submitted`;
- `advocate_eligible`;
- `testimonial_received`;
- `referral_created`;
- `referred_purchase`.

### Economic fields by source/cohort

- media/acquisition spend;
- creative/content production cost where material;
- fulfillment cost;
- human follow-up cost/time where material;
- payment fees;
- Guide contribution;
- Full Course normalized contribution;
- total customer contribution to date;
- payback date/days when applicable.

## 24. Scorecard

Do not judge Funnel 01 by one conversion rate.

### Demand / lead

- qualified sessions by source;
- F1 opt-in rate by source/message;
- cost per identifiable lead where paid acquisition exists;
- percentage self-routing Guide vs Full.

### Guide

- Guide checkout-start rate;
- Guide purchase rate;
- contribution per Guide order;
- Guide fulfillment/support burden;
- Guide -> Full attach rate at 30/60/90-day cohort windows;
- time from Guide to Full purchase.

### Full Course

- sales-page -> checkout-start rate;
- checkout-start -> purchase rate;
- direct-Full purchase rate by source;
- Guide-assisted Full purchase rate;
- Single/Couple mix;
- paid participants per edition;
- revenue per edition;
- normalized direct contribution per edition;
- complete acquisition cost per Full buyer/source;
- no-show rate;
- attendance rate.

### Post-delivery

- satisfaction distribution;
- service-recovery rate;
- testimonial/review acquisition;
- referral creation rate;
- referred buyer rate;
- stated reason for choosing Formalife;
- perceived difference themes.

### Whole-system economics

- contribution per lead/cohort;
- contribution per acquired buyer;
- percentage reaching Full Course;
- time to Full Course purchase;
- complete CAC by source;
- payback;
- capacity consumed per euro of contribution.

No benchmark conversion percentages are assumed before Formalife generates its own baseline.

## 25. Controlled test sequence

### Test 0 — launch readiness

Must pass before measuring marketing:

- rebuilt Full curriculum/claims scientifically reviewed;
- customer-facing authority/credential wording verified;
- Full offer page/price/date terms coherent;
- Guide fulfillment works;
- Training Credit rules either standardized or omitted from customer-facing automation until ready;
- Stripe/course checkout works end-to-end;
- attribution events verified;
- suppression logic works;
- post-purchase reminders and roster creation work.

Failure here is not a marketing failure.

### Test 1 — direct Full lane

Population:

- high-intent direct/referral/owned/search demand that can be measured.

Question:

**Can the rebuilt Full Course convert sufficiently at EUR 80 / EUR 120 and fill editions toward 10-12 paying participants without forcing a low-ticket step?**

Read:

- source -> Full page -> checkout -> purchase -> attendance -> contribution.

### Test 2 — Guide-first lane

Question:

**Does adding the EUR 19.90 Guide front-end create incremental economically useful Full buyers or contribution versus direct-only acquisition?**

Read:

- source -> Guide -> Full;
- Guide standalone contribution;
- Guide -> Full attach/time;
- complete CAC/payback;
- incremental complexity/support.

If Guide adds transactions but worsens economics or delays ready buyers, reduce its role.

### Test 3 — F1 lead/nurture lane

Question:

**Can Formalife convert problem-aware but non-ready parents into identifiable prospects and then into Guide or Full buyers economically?**

Read:

- traffic -> F1 -> route -> Guide/Full -> contribution.

Only after downstream conversion is readable should Formalife test significant cold paid volume into F1.

### Test 4 — paid demand scale

Allowed only when:

- downstream funnel events are trustworthy;
- offer/delivery work;
- capacity can absorb demand;
- complete CAC can be compared to actual contribution/payback.

Increase traffic after the first material bottleneck is known and corrected, not before.

## 26. Diagnostic matrix

### Traffic but weak F1 response

Possible upstream causes:

- wrong audience/intent;
- trigger not salient;
- weak lead offer;
- insufficient credibility;
- landing friction.

Do not blame email automation first.

### F1 leads but no paid movement

Check:

- wrong awareness assumption;
- free asset solves too much or too little;
- Guide/Full bridge unclear;
- offer/proof weak;
- bad timing;
- traffic quality.

### Guide sells but Full does not

Check:

- Guide buyers may be a different economic segment;
- supervised-practice value not clear;
- broader Full job not understood;
- Training Credit not meaningful;
- Full offer/positioning weak;
- Guide may be delaying people rather than qualifying them.

Do not protect the Guide because it exists.

### Full page gets traffic but little checkout intent

Check before changing checkout:

- positioning/difference;
- offer value;
- proof;
- price comparability;
- target/trigger match;
- date/location fit.

### Checkout starts but purchases are weak

Check:

- technical failure;
- unexpected terms/costs;
- payment friction;
- date/location uncertainty;
- last-step trust/risk.

### Purchases good but attendance weak

Check:

- reminder/logistics;
- purchase-to-event delay;
- cancellation policy clarity;
- event timing/location;
- buyer commitment/fit.

### Attendance good but satisfaction/proof weak

This is primarily a delivery/product problem, not an acquisition problem.

### Satisfaction high but referrals weak

Check whether referral was:

- explicitly requested;
- easy to perform;
- timed after success;
- low-risk for the referrer;
- attributable.

## 27. What Funnel 01 deliberately does not include

Do not contaminate this first proof engine with:

- partner-hosted Light;
- broad B2B/B2B2C mechanics;
- Digital Flagship sales;
- Hybrid bundle;
- BLSD Formalife sales;
- membership;
- large lifecycle product graph;
- dozens of lead magnets;
- physical-product catalog;
- complex loyalty program;
- cold paid scale before downstream economics are legible.

Future products can later attach to the same customer record without changing the causal logic of Funnel 01.

## 28. Definition of done for Funnel 01

Funnel 01 is not "done" when the pages and automations exist.

It is operationally proven when Formalife can reliably read, for repeated course editions:

1. where each buyer came from;
2. whether they entered direct, through F1 or through Guide;
3. where non-buyers fell out and the main known reason;
4. checkout and attendance behavior;
5. revenue and normalized contribution per edition;
6. complete acquisition cost by relevant source;
7. Guide -> Full behavior when Guide is used;
8. customer reason for choosing Formalife / perceived difference;
9. satisfaction/service-recovery outcomes;
10. proof/referral generated after delivery;
11. whether the system can operate without founder-side improvisation at each step.

Only after these are readable does scaling traffic or adding the Digital Flagship become a controlled next decision rather than another source of ambiguity.

## 29. Revision conditions

Revise or remove a component when evidence shows that it:

- adds friction without incremental contribution;
- attracts economically poor-fit buyers;
- produces acquisition cost that cannot be recovered within an acceptable cash window;
- overloads delivery/support capacity;
- confuses the choking wedge with the broader Formalife promise;
- relies on claims Formalife cannot support;
- creates data/automation complexity without changing decisions.

The system is allowed to become **simpler** after testing.

## Layer 1 references

- `REASONING_KERNEL.md`
- `merenda/03_offerta/offerta-a-risposta-diretta.md`
- `merenda/03_offerta/front-end-e-back-end.md`
- `merenda/05_acquisizione/funnel-e-conversione.md`
- `merenda/05_acquisizione/information-marketing.md`
- `merenda/05_acquisizione/database-email-e-sequenze.md`
- `merenda/05_acquisizione/referral-e-soddisfazione.md`
- `merenda/06_vendita/follow-up-lead-non-convertiti.md`
- `merenda/09_business/numeri-cassa-e-crescita.md`

## Layer 2 dependencies

- `CURRENT_STATE.md`
- `MARKET_EVIDENCE.md`
- `B2C_COMMERCIAL_FUNNEL_V2.md`
- `FORMALIFE_BUILD_SEQUENCE.md`
- `FLAGSHIP_SCOPE_V1.md`
- `ASSET_INVENTORY.md`

## Provenance note

This document is a Formalife operating synthesis built from current Layer 2 evidence/decisions and current Layer 1 doctrine. It is not a transcript or impersonation of Frank Merenda. Offer mechanics not already approved are marked as hypotheses or operating defaults and must earn permanence through Formalife results.