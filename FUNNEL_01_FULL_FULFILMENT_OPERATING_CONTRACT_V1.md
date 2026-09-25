# Funnel 01 — Full Fulfilment Operating Contract V1

Status: CURRENT OPERATING CONTRACT — first operationalization block; unresolved parameters remain explicit OPEN items
Date: 2026-09-25

Purpose: define what Formalife must do from Full purchase/pre-enrolment through attendance, service recovery, proof/referral and refresh/lifecycle handoff.

Canonical parents:

- `FUNNEL_01_FULL_OPERATIONALIZATION_PLAN_V1.md`
- `FUNNEL_01_G3_FULL_OFFER_ARCHITECTURE.md`
- `FUNNEL_01_MASTER_OPERATING_SYSTEM_V1.md`
- `B2C_FUNNEL_01_CHOKING_FULL_ADAPTIVE.md`
- `FUNNEL_01_G4_PROOF_ARCHITECTURE.md`

This file does not create a competing CRM taxonomy. It uses the current Funnel 01 relationship/intent/barrier states and defines fulfilment stages/actions around them.

---

# 1. Fulfilment invariant

A Full sale is not complete when payment succeeds.

Operational chain:

**commercial commitment -> correct edition/order state -> confirmation -> preparation -> attendance -> promised physical/service delivery -> service resolution -> proof/referral -> refresh/lifecycle relationship.**

Every customer promise must map to:

- trigger;
- owner;
- action;
- record/event;
- exception path;
- completion condition.

---

# 2. Entry modes

## F1 — Confirmed Full purchase

Trigger:

customer completes payment for a confirmed edition.

Relationship transition:

`KNOWN/ENGAGED PROSPECT -> FULL_CUSTOMER`.

Required record:

- order/customer identity;
- edition/date/location;
- 1 or 2 Caregivers;
- amount/payment status;
- source/referrer/campaign where available;
- Guide ownership/Training Credit if applicable;
- consent/permission state;
- customer-protection terms version accepted/displayed.

## F2 — Protected activation pre-enrolment

Trigger:

customer enters an approved `EDIZIONE IN ATTIVAZIONE — PRE-ISCRIZIONE PROTETTA` route and pays the approved Guide-backed amount.

Customer is **not yet a confirmed Full attendee**.

Required record:

- activation edition;
- published minimum threshold;
- published decision deadline;
- Guide fulfilment;
- amount paid;
- Training Credit eligibility if edition activates;
- refund obligation if edition does not activate.

Do not communicate a pre-enrolled customer as if the edition is already confirmed.

---

# 3. Stage contract

## STAGE A — Payment / commitment received

### Job

Create one reliable order/customer record and route to the correct fulfilment lane.

### Actions

- verify payment status;
- identify confirmed-purchase vs activation-pre-enrolment;
- identify 1 vs 2 Caregivers;
- check Guide already owned / Training Credit path where relevant;
- attach edition and source/referrer information when available;
- suppress acquisition/abandonment messaging for the product actually purchased.

### Owner

Commerce/CRM system with human exception handling.

### Exit

Order is operationally classified and confirmation can be sent.

### Failure path

Payment failure / ambiguous order / duplicate customer -> human support task.

---

## STAGE B — Immediate confirmation

### Job

Remove uncertainty about what was purchased and what happens next.

### Confirmed-edition customer must receive

- course name;
- date/time/location;
- 1 or 2 Caregivers;
- participant-name/count confirmation where needed;
- concise fit/boundary statement;
- logistics/preparation expectation;
- customer-protection route/link;
- support contact/path;
- next communication expectation.

### Activation-pre-enrolment customer must receive instead

- conspicuous `EDIZIONE IN ATTIVAZIONE` status;
- threshold for that edition;
- confirmation deadline;
- what happens if activated;
- what happens if not activated;
- Guide fulfilment expectation;
- support route.

### Event / record

`full_purchase` for confirmed Full or appropriate activation/pre-enrolment event in implementation.

### Exit

Customer can accurately describe their current booking status.

---

## STAGE C — Guide / included physical entitlement resolution

### Job

Avoid duplicate fulfilment and ensure the offer inclusion is real.

### Rules already approved

- direct 1-Caregiver Full -> one Guide unless already owned through Guide-first path;
- direct 2-Caregivers -> two Guides total unless one/both already owned;
- Guide buyer upgrading to Full does not automatically receive a duplicate copy for themselves;
- activation-pre-enrolment customer receives the Guide immediately under the approved protected mechanism.

### OPEN implementation

Exact operational check for prior Guide ownership/redemption must be encoded in checkout/CRM later.

### Exit

Correct Guide entitlement is recorded for the order.

---

## STAGE D — Second caregiver resolution

### Job

Make the household option operational without overselling scarce capacity.

### Current approved commercial rule

1 Caregiver can add a second caregiver for `+EUR 40`, subject to real seat availability and an operating cutoff.

### Required behavior

- expose whether second caregiver is already included;
- allow extension only if seat exists and cutoff rule permits it;
- update participant count and Guide entitlement;
- update revenue/contribution record;
- do not create 3/4-person group discounts in V1.

### OPEN

**Exact cutoff for adding the second caregiver.**

Do not publish an invented cutoff until founder/operations decides it.

---

## STAGE E — Pre-course orientation

### Job

Prepare the participant to use live time for decision/practice work without creating a second course.

### Required jobs

- frame the broader parent-first preparedness system;
- explain what the live course will and will not do;
- set expectations for active practice/simulation;
- give only logistics/preparation information that improves attendance/delivery;
- clarify that participation certificate is not professional certification where relevant.

### Format

OPEN implementation choice: short page, email, video or combined minimal asset.

### Constraint

Do not move important supervised-practice learning into passive pre-course material merely to shorten the live event.

### Exit

Participant has received the orientation asset before attendance.

---

## STAGE F — Attendance reminder / final readiness

### Job

Reduce preventable no-show/confusion and surface operational problems before the course starts.

### Required reminder content

- date/time/location;
- arrival/logistics;
- participant count / second caregiver as booked;
- simple support path for transfer/emergency;
- no sales content required.

### Timing

Exact reminder cadence is an implementation test; no universal cadence is canonical yet.

### Exceptions

- transfer/cancellation request -> Customer Protection procedure;
- second-caregiver request -> Stage D;
- material service question -> human support;
- clinical question outside approved customer material -> escalate to appropriate scientific/support owner, not marketing improvisation.

---

## STAGE G — Day-of delivery record

### Job

Deliver what was sold and create evidence that the process occurred.

### Required operating record

- edition/date;
- instructors;
- participant count;
- 1/2 Caregiver mix;
- attendance/no-show status;
- curriculum/deck version;
- practical stations delivered;
- material handoff completed;
- significant deviation/service issue;
- media permission where separately obtained.

### Relationship transition

Attended participant -> `ATTENDED_CUSTOMER` after completed delivery.

### Proof boundary

This record proves delivery/process, not real-emergency competence.

---

## STAGE H — Guide / Family Emergency Pack / certificate handoff

### Job

Make the AFTER promise physically real.

### Current package rule

- one Family Emergency Pack per household/order;
- duplicate small personal pieces for second caregiver only where useful;
- Guide entitlement as resolved earlier;
- attendance certificate within final approved wording;
- refresh entitlement explained;
- Pass Sicurezza/referral tool explained during handoff.

### Dependency

Family Emergency Pack V1 production specification remains the next dedicated operationalization block.

### Exit

Handoff is complete or an explicit fulfilment exception is recorded.

---

## STAGE I — Immediate service / BTRF window

### Job

Resolve a bad experience before asking for proof/referral.

### Current approved BTRF direction

Complete attendee may contact Formalife within 24 hours if they believe the Full experience did not justify the amount paid.

If legitimate request is processed under approved policy:

- refund Full amount actually paid;
- customer keeps Guide + Family Emergency Pack;
- unused future Full service entitlements, including refresh, end;
- refund/payment cost recorded.

### Suppression

Open service/BTRF issue suppresses ordinary testimonial/referral/upsell activity.

### Event

Service issue / guarantee request / refund resolution must be recordable.

---

## STAGE J — T+24h feedback / review

### Job

Capture experience and begin current rebuilt-Full proof production without contaminating service recovery.

### Current approved operating direction

- send simple Google-review request at approximately T+24h;
- no referral CTA in the same message;
- service recovery takes precedence.

### Internal learning capture should preserve, where feasible

- original trigger;
- expectation before course;
- most useful practical element;
- perception of broader preparedness vs choking-only expectation;
- value of supervised correction;
- intended use of Guide/Pack/framework/refresh;
- weak/confusing/disappointing elements.

Do not force every question into one customer-facing form if friction destroys response quality.

---

## STAGE K — T+7d referral eligibility / ask

### Job

Ask only after positive evidence and no unresolved service issue.

### Current direction

Explicit digital referral ask is sent only to customers who actually left a Google review or usable testimonial and have no unresolved service issue.

Tools may include:

- WhatsApp share;
- personal referral link;
- physical Pass reminder.

### Reward

When a referred person completes a paid Full purchase, current approved reward is one Guide Gift for the referrer to give to another family/caregiver.

Referral fulfilment cost must be tracked as referral CAC.

---

## STAGE L — Customer relationship stream

### Job

Remain useful and available without turning the customer into a perpetual sales target.

### Current stream purpose

- verified pediatric-safety updates;
- useful articles/news;
- seasonal/transversal relevant content;
- refresh reminders;
- surface genuine new needs/triggers;
- future offers only when relevant to state/trigger.

### Permission boundary

Transactional/service messages and marketing/editorial messages remain distinct.

### OPEN

Exact cadence/content operating rhythm.

---

## STAGE M — Refresh entitlement

### Job

Deliver the promised one free practical retraining opportunity within 12 months without silently destroying sellable capacity.

### Current entitlement

- one redemption per participant;
- approximately final 90 minutes of an appropriate Full edition;
- booking required;
- practical/decision refresh, not repetition of the full course.

### Required records

- eligibility start/end;
- booked edition;
- redemption;
- cancellation/no-show;
- whether seat displaced otherwise sellable capacity.

### OPEN

- capacity reservation rule;
- booking cutoff;
- refresh cancellation/no-show rule;
- exact expiry handling.

A separate Refresh Operating System will close this block.

---

# 4. Exception contract

## Customer cancellation / transfer

Use current Total Protection architecture:

- >7d: refund or free transfer;
- 7d–48h: one free transfer within 12 months or Formalife credit if suitable date unavailable;
- <48h/before start: one free transfer for genuine family/child illness or material emergency communicated before start;
- no-show without notice: no cash refund; current test recovery route = rebook once at 50% of then-current public price, subject to capacity.

Exact legal/customer wording remains pending.

## Formalife cancellation

Current direction:

- 100% refund or priority transfer;
- EUR 20 future goodwill credit.

OPEN:

exact expiry/use rules for the goodwill credit.

## Activation edition fails

Under the approved Guide-backed protected mechanism:

- refund the EUR 19.90 pre-enrolment payment automatically;
- customer keeps the Guide;
- record failed-activation fulfilment cost.

## Service issue

Move to `SERVICE_RECOVERY` and suppress ordinary promotion/proof/referral until resolved.

---

# 5. Minimum event/data contract

Before the first active-demand route is interpreted economically, implementation must support enough of the following to reconstruct the customer journey:

- customer/person identity;
- source/referrer/campaign where available;
- edition;
- 1/2 Caregivers;
- purchase/pre-enrolment/payment status;
- Guide entitlement/ownership;
- attendance/no-show/transfer;
- service issue/BTRF/refund;
- material handoff completion where operationally useful;
- review/testimonial;
- referral relationship/referred purchase;
- refresh entitlement/booking/redemption;
- revenue and material variable fulfilment cost.

Do not create unused profile fields.

---

# 6. Current unresolved decisions surfaced by this contract

The first operationalization pass exposes five material decisions that still require closure before launch:

1. second-caregiver addition cutoff;
2. exact pre-course orientation implementation;
3. Family Emergency Pack manufacturable V1 + supplier cost;
4. refresh capacity/no-show rules;
5. EUR 20 goodwill-credit expiry/use rule.

Legal wording, final clinical QA and rehearsal remain separate pre-delivery gates rather than hidden inside these operating rules.

---

# 7. Next block

The next operationalization child is:

**Family Emergency Pack Production Specification V1.**

Reason:

The Pack is currently promised inside both 1- and 2-Caregiver Full offers and appears in proof/referral/continuity architecture, but its actual manufactured V1 is not yet specified or cost-verified. Until it exists, part of the customer-facing Full offer remains architectural rather than fulfilable.

Do not solve this by deleting the Pack from the offer merely to remove work; first build the smallest useful, low-cost V1 and obtain real supplier economics.