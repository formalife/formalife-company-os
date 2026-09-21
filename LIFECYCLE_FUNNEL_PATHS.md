# Formalife Lifecycle Funnel Paths

Status: PROVISIONAL LIFECYCLE FUNNEL ARCHITECTURE — founder review pending
Date: 2026-09-21

Founder feedback recorded 2026-09-21: the 12-node substantive course map in `COURSE_FUNNEL_GRAPH.md` is considered **substantially sound**. This does not yet promote every node or edge to a final build decision. The current task is to connect those nodes through real parent/caregiver motivations over months and years.

Purpose: define why, when and under which trigger a customer should move from one Formalife node to another. The objective is not to maximize cross-sell density. It is to build a long customer relationship in which the **next offer becomes relevant because the child's life stage, exposure, diagnosis/need or desired competence has changed**.

`FORMALIFE_BUILD_SEQUENCE.md` continues to govern what is built first. This document describes the mature lifecycle graph, not the immediate product roadmap.

## 1. Governing rule — no “next course” without a new reason to care

The correct unit is not:

**course completed -> sell another course.**

It is:

**current problem solved -> relationship retained -> new trigger appears -> new safety problem becomes salient -> relevant next product.**

A node-to-node edge deserves to exist only when at least one of these conditions is true:

1. **depth:** the customer now wants substantially greater competence in the same high-value capability;
2. **life-stage transition:** the child enters a stage that creates a new risk environment;
3. **new exposure/event:** pool, travel, pet interaction, daycare, a car-seat transition or another concrete context creates a new problem;
4. **specific specialist need:** a problem such as food allergy makes a specialist node newly relevant;
5. **care-network expansion:** another person now cares for the same child and must acquire the substantive skill themselves;
6. **skill decay / elapsed time:** a previously learned practical skill needs refresh or reassessment.

If none of these is present, Formalife should generally **wait rather than manufacture urgency**.

## 2. Four classes of graph edges

### A. Depth edges

Same responsibility, higher competence.

Typical pattern:

**C03 Choking -> C04 Pediatric Emergency Preparedness -> C05 BLSD Formalife -> refresh/reassessment**

The motivation changes from one feared event, to broader emergency capability, to deep resuscitation mastery.

### B. Life-stage edges

The customer does not need to be persuaded that the old problem was incomplete. The child's development creates a new one.

Typical pattern:

**C01 Newborn Safety -> C02 Starting Solids -> C08 Home Safety as mobility begins -> later exposure-specific nodes.**

The time gap can be months. That is a feature, not a funnel failure.

### C. Exposure / event edges

A new environment or event increases salience quickly.

Examples:

- pool/sea/villa/boat access -> C09 Water Safety;
- seat purchase or transition -> C10 Car Seat Safety;
- dog + new baby / dog + crawling child -> C12 Child + Pet Safety;
- illness / nursery exposure -> C07 Sickness & Red Flags.

### D. Specialist-need edges

These must remain conditional rather than broad cross-sells.

Example:

**C02 Starting Solids -> C11 Food Allergy & Anaphylaxis Preparedness only when the family has a real allergy-related need.**

A specialist node should not be promoted merely because the customer happens to share the same demographic profile.

## 3. Strong and conditional node-to-node edges

The following are the current proposed edges. `Strong` means the transition has a clear causal motivation when the trigger occurs. `Conditional` means the edge exists only for a subset with a specific new need.

| From | To | Strength | Trigger that makes the next node matter |
|---|---|---|---|
| C01 Newborn Safety & Safe Sleep | C02 Starting Solids & Feeding Safety | **Strong lifecycle** | family approaches complementary feeding |
| C01 | C07 Sickness & Red Flags | **Strong event/lifecycle** | first recurrent illness uncertainty, nursery/daycare exposure, parent asks “when should I worry?” |
| C01 | C08 Home Safety & Babyproofing | **Strong lifecycle** | rolling/crawling/pulling-to-stand/walking changes the home risk environment |
| C01 | C10 Car Seat Safety | **Strong when relevant** | first seat, fit/install uncertainty or later seat transition; can also precede C01 |
| C01 | C04 Pediatric Emergency Preparedness | **Conditional depth** | new parent wants broader preparedness beyond newborn-environment safety |
| C02 Starting Solids | C03 Choking & Disobstruction | **Very strong** | feeding starts and choking fear becomes operational rather than abstract |
| C02 | C11 Food Allergy & Anaphylaxis Preparedness | **Conditional specialist** | allergy-related need becomes real; not a routine upsell to every family |
| C02 | C08 Home Safety | **Strong lifecycle** | feeding transition coincides with increasing mobility/exploration |
| C03 Choking | C04 Pediatric Emergency Preparedness | **Very strong depth** | customer has solved one feared emergency and recognizes the wider question: “what about the other serious situations?” |
| C03 | C05 BLSD Formalife | **Conditional depth** | customer specifically wants much deeper CPR/resuscitation practice; C04 is normally the cleaner bridge |
| C04 Pediatric Emergency Preparedness | C05 BLSD Formalife | **Very strong depth** | practical introduction exposes the difference between awareness and resuscitation mastery |
| C04 | C07 Sickness & Red Flags | **Conditional horizontal** | family wants more depth on illness recognition/escalation than a broad emergency course should contain |
| C07 Sickness & Red Flags | C04 Pediatric Emergency Preparedness | **Strong when consequence becomes salient** | parent learns when a situation is serious and then wants to know what to do before professional help takes over |
| C08 Home Safety | C09 Water Safety & Drowning Prevention | **Strong exposure** | pool/beach/open-water season or new recurring water exposure |
| C08 | C12 Child + Pet Safety | **Strong conditional lifecycle** | household has dog/pet and the child's mobility changes interaction risk |
| C08 | C04 Pediatric Emergency Preparedness | **Conditional horizontal** | prevention-focused parent wants competence for incidents that prevention cannot eliminate |
| C09 Water Safety | C04 Pediatric Emergency Preparedness | **Strong consequence edge** | water-risk education makes general emergency action relevant |
| C09 | C05 BLSD Formalife | **Strong depth for high-intent segment** | family wants deeper resuscitation capability because drowning consequence is salient |
| C10 Car Seat Safety | other nodes | **No forced edge** | wait for the next genuine lifecycle/exposure trigger; do not invent road-safety cross-sells |
| C11 Food Allergy & Anaphylaxis | C04 Pediatric Emergency Preparedness | **Conditional horizontal** | family wants broader emergency competence beyond allergy management |
| C12 Child + Pet Safety | C08 Home Safety | **Strong adjacent when mobility is active** | pet interaction is one component of a broader newly mobile-child home risk environment |
| C06 BLSD Certified | C05 BLSD Formalife | **Conditional depth** | customer obtained the credential but separately wants more deliberate practice than the certification product provides |
| C06 | C04 Pediatric Emergency Preparedness | **Conditional context shift** | credential customer is also a parent/caregiver and wants broader pediatric preparedness |

Important: **not every node needs a compulsory outgoing edge.** A customer can complete one problem, remain in the Formalife relationship and wait months or years until another legitimate trigger appears.

## 4. The main long-horizon customer journeys

### Journey A — pregnancy/newborn -> early childhood safety lifecycle

This is the cleanest multi-year Formalife journey.

**Pregnancy / newborn arrival**
-> C01 Newborn Safety & Safe Sleep

Then the graph can branch rather than force one next purchase:

- seat purchase/installation question -> C10 Car Seat Safety;
- desire for broad emergency capability -> C04 Pediatric Emergency Preparedness;
- first meaningful illness uncertainty -> C07 Sickness & Red Flags.

**Starting solids**
-> C02 Starting Solids & Feeding Safety
-> C03 Choking & Disobstruction

**Increasing mobility**
-> C08 Home Safety & Babyproofing
-> C12 only if pet interaction is relevant.

**Water exposure / first high-risk summer / recurring pool context**
-> C09 Water Safety & Drowning Prevention.

At any point, if the parent moves from general preparedness to deep motor-skill competence:

**C04 -> C05 BLSD Formalife -> later refresh/reassessment.**

This journey can legitimately span years. The relationship survives because Formalife is useful at each transition, not because it continuously pushes a subscription.

### Journey B — starting-solids acquisition funnel

This is especially attractive because it begins from a predictable life-stage trigger and connects naturally to Formalife's strongest current wedge.

**starting-solids question / partner content / free utility**
-> C02 Starting Solids & Feeding Safety
-> C03 Choking & Disobstruction
-> C04 Pediatric Emergency Preparedness
-> C05 BLSD Formalife for the high-intent mastery segment
-> later refresh.

The causal logic is strong:

**feed safely -> know what to do if choking occurs -> become prepared for emergencies beyond choking -> become highly competent in resuscitation.**

C11 Allergy is a side branch only if a real allergy-related need appears.

### Journey C — choking fear -> emergency mastery

This is the shortest near-term Formalife funnel and should remain the first one to prove economically.

**choking fear / Guide / Light / partner referral**
-> C03 Choking & Disobstruction
-> C04 Pediatric Emergency Preparedness
-> C05 BLSD Formalife
-> refresh/reassessment
-> additional caregiver takes the relevant substantive course.

The key psychological transition is not “buy the bigger course”. It is:

**I entered because I was afraid of one event -> I now understand that preparedness is a broader capability -> I want enough practice that I can actually perform under pressure.**

### Journey D — illness-first customer

Some parents will enter Formalife through uncertainty rather than through a practical emergency skill.

**fever / breathing / dehydration / red-flag question**
-> useful reference or C07 Sickness & Red Flags
-> C04 Pediatric Emergency Preparedness when the customer wants action capability for situations that have crossed the serious threshold
-> C05 only if deeper resuscitation mastery is desired.

This path is cognitively different from choking acquisition and should not be forced through C03 first.

### Journey E — prevention-first customer

A parent may enter because the child becomes mobile rather than because they fear an acute emergency.

**mobility / new home / babyproofing concern**
-> C08 Home Safety & Babyproofing

Then only real exposures create branches:

- pool/sea -> C09;
- pet + mobile child -> C12;
- car-seat transition -> C10;
- broader “what if something still happens?” concern -> C04.

This path is valuable because it can acquire a parent who would not initially search for “first aid course”.

### Journey F — water-risk funnel

**summer / holiday villa / pool / boat / beach / recurring water exposure**
-> free water-safety tool/checklist or direct C09
-> C09 Water Safety & Drowning Prevention
-> C04 for broader emergency preparedness and/or C05 for deeper resuscitation competence
-> seasonal reminder / skill refresh before future high-exposure periods when scientifically and commercially justified.

This is a good example of **seasonal reactivation without creating a generic Travel Safety course**.

### Journey G — allergy-specific funnel

**specific allergy-related need**
-> C11 Food Allergy & Anaphylaxis Preparedness
-> caregiver expansion so other responsible adults understand the same plan
-> C04 only if the family wants broader pediatric-emergency competence.

This funnel should remain narrow. Its strength comes from relevance, not volume.

### Journey H — child + pet transition funnel

**pregnancy with dog/pet OR crawling/walking child changes interaction dynamics**
-> C12 Child + Pet Safety
-> C08 Home Safety when the family also needs broader environmental prevention
-> otherwise remain dormant until another genuine trigger appears.

The correct commercial model may ultimately be specialist partnership or consultation-led rather than a high-volume Formalife-owned course.

### Journey I — credential-entry funnel

**formal certification need**
-> C06 BLSD Certified

From there the customer enters the consumer graph only if a separate job exists:

- wants deeper learning/practice -> C05;
- is a parent/caregiver and wants broader child-safety preparedness -> C04.

Do not force certification customers through consumer products merely because their email address is now known.

## 5. One family can travel through several independent spines

The mature graph is better understood as several overlapping spines rather than one giant funnel.

### Emergency mastery spine

**C03 -> C04 -> C05 -> refresh**

### Early-life lifecycle spine

**C01 -> C02 -> C08 -> later exposure nodes**

C03 branches strongly from C02; C07 can activate at any point when illness becomes salient.

### Environmental-prevention spine

**C08 -> C09 / C10 / C12 according to exposure**

This is not a fixed order. The environment selects the branch.

### Health-navigation spine

**C07 -> C04 when recognition creates demand for action competence**

C11 remains a separate specialist branch rather than generic “advanced health”.

### Credential spine

**C06 -> renewal**, with optional entry into C05/C04 only when the customer's job changes.

## 6. Timing architecture — Formalife should wait intelligently

The system should distinguish **immediate ascension** from **lifecycle reactivation**.

### Immediate / short-horizon transitions

These can happen after a first strong experience because the new motivation already exists:

- C02 -> C03;
- C03 -> C04;
- C04 -> C05;
- C07 -> C04 when broader emergency action is already salient;
- C09 -> C04/C05 for high-intent families.

### Months-later transitions

These depend primarily on development or environment:

- C01 -> C02 when starting solids approaches;
- C01/C02 -> C08 as mobility changes;
- C01 -> C07 when illness/daycare creates the question;
- any earlier node -> C09 when meaningful water exposure begins;
- C08 -> C12 when mobility changes pet interaction.

### Years-later or repeated transitions

These are often better treated as reactivation or repeat depth rather than as new nodes:

- new sibling -> C01/newborn resources become relevant again;
- new car seat / size-stage transition -> C10 reactivation;
- elapsed time since hands-on C03/C04/C05 -> refresh/reassessment;
- new grandparent/babysitter/nanny -> caregiver skill replication;
- recurring water season/exposure -> C09 reminder or refresh;
- new family/home/travel context -> relevant C08/C09/C10 resources.

Exact age or refresh intervals should be governed by scientific/product evidence. The commercial architecture should store the **trigger**, not invent a universal medical/safety schedule.

## 7. Caregiver expansion is an overlay, not a node

Every substantive node can produce a second customer without inventing another course.

Pattern:

**parent completes relevant node -> identifies another real caregiver -> caregiver receives the same appropriate substantive training.**

Examples:

- C03 parent -> grandparent/babysitter C03;
- C04 parent -> partner/grandparent C04;
- C11 allergy-prepared household -> all relevant caregivers receive the required specialist preparation;
- C01 newborn-safety system -> caregiver consistency around safe sleep and home rules.

The sale is justified by a real safety responsibility, not by a generic referral incentive.

## 8. Refresh is also an overlay

Practical competence should not be modeled as “course once -> permanent capability”.

For C03, C04 and C05, Formalife should eventually create a refresh/rehearsal layer triggered by elapsed time, self-reported confidence, updated guidance, a new caregiver or renewed exposure.

The exact cadence is not decided here.

Refresh can include:

- short practical reassessment;
- scenario session;
- digital reference/recap;
- family practice event;
- update module;
- full retraining where appropriate.

Commercially, this creates legitimate repeat value without inventing a thirteenth substantive course.

## 9. CRM / lifecycle state machine

The future lifecycle system should operate approximately as:

**entry trigger -> relevant free/paid node -> completion -> identify next plausible trigger -> waiting state -> trigger becomes active -> useful content/reference -> relevant offer -> purchase/completion -> new waiting state.**

The important state is often **WAITING FOR TRIGGER**, not “needs nurturing harder”.

Minimum future customer-state dimensions, where lawful and voluntarily supplied, can include:

- purchased/completed nodes;
- broad child life stage;
- likely upcoming life-stage transition;
- relevant environmental exposures voluntarily indicated;
- whether additional caregivers need preparation;
- elapsed time since practical skills training;
- source/partner;
- consent/communication state.

Health information such as allergy diagnoses can be sensitive. Formalife should not collect unnecessary clinical data merely to improve cross-sell. Where a specialist need must be routed, use the minimum lawful information necessary and appropriate governance.

## 10. The next-best-offer rule

The “next best offer” should be selected by this hierarchy:

1. **active new problem / explicit search**;
2. **known imminent life-stage trigger**;
3. **clear depth request after a completed product**;
4. **new caregiver responsibility**;
5. **scientifically justified refresh need**;
6. only then broader educational cross-sell.

This means the same customer may receive no paid offer for months. That is acceptable if Formalife remains useful through references, reminders and appropriate content until the next real demand window opens.

## 11. What not to build

Do not create:

- an all-to-all recommendation graph;
- “customers who bought X also buy Y” logic without causal relevance;
- automated course promotion based only on elapsed calendar time;
- fake urgency when the next life-stage trigger has not arrived;
- a mandatory C03 -> C04 -> C05 staircase for everyone;
- a mandatory flagship step before specialist problems;
- generic Travel/Outdoor/Caregiver courses merely to add intermediate transactions;
- a subscription used to hide the absence of recurring value.

The graph should be **sparse but powerful**.

## 12. Measurement

Once enough products exist to test the graph, track at least:

- second-purchase rate by entry node;
- time to second purchase by transition type;
- percentage of customers reactivated by an actual life-stage/exposure trigger;
- conversion of each major edge, e.g. C03 -> C04 and C04 -> C05;
- contribution/LTV by original entry node;
- caregiver-expansion rate;
- refresh/retraining uptake;
- share of offers sent because a relevant trigger was known versus generic batch promotion;
- customer opt-out/unsubscribe and complaint signals as a check against over-promotion.

The graph is economically useful only if the edges create additional contribution without damaging trust or adding disproportionate operating complexity.

## 13. Relationship to the current build sequence

Nothing in this lifecycle architecture authorizes Formalife to build twelve courses now.

The current founder-authorized sequence remains:

**focused flagship -> repeatable trusted distribution -> scalable digital monetization -> owned demand/lifecycle system -> adjacent expansion.**

The immediate proof path remains the choking/Guide/Light/Full system. The lifecycle graph matters now mainly because it tells Formalife **what customer data and future bridge should be designed into the first engine**, so that the relationship does not end after the first course.

## Layer 1 references

- `REASONING_KERNEL.md` — design second sale/retention only where naturally supported; route by customer state rather than source alone.
- `merenda/03_offerta/front-end-e-back-end.md` — design the second transaction explicitly; cross-sell must add real value rather than make the first product deliberately incomplete.
- `merenda/04_marketing/gerarchia-domanda-e-canali.md` — demand has a temporal dimension; life events and timing determine when a customer becomes more responsive.
- `merenda/04_marketing/riattivazione-clienti.md` — reactivation requires an expected return/trigger rather than treating every inactive customer as lost.

## Revision condition

Revise an edge when customer data shows that the assumed trigger does not produce meaningful demand, when the next product does not create sufficient incremental value, or when conversion/economics do not justify the complexity of maintaining that path.
