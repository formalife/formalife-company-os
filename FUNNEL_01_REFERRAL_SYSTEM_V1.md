# Funnel 01 — Referral System V1

Status: CURRENT WORKING OPERATING DESIGN — founder asked for a concrete referral mechanism; incentive economics remain testable, not permanent
Date: 2026-09-22

Purpose: convert satisfied Full customers into a measurable trust-transfer acquisition channel without turning the end of the course into a sales pitch or putting the customer's reputation at unnecessary risk.

Layer 1 governing principle:

**satisfaction real -> appropriate moment -> explicit request -> facilitated introduction -> sustainable incentive if useful -> attribution -> referred-customer experience.**

Referral is not “hope they tell friends”. It is an operating system.

---

# 1. Referral jobs

The system must do five things:

1. ask only after enough value/satisfaction has been earned;
2. make sharing easier than explaining Formalife from memory;
3. protect the referrer's reputation through useful material + strong customer protection;
4. route the referred person according to awareness/intent rather than forcing immediate purchase;
5. attribute economic value to the original referrer.

---

# 2. Do not use the course close as the main referral ask

ACT 7 should contain only a **referral seed**, not a hard request to “bring us customers”.

Reason:

- the participant has just completed a high-emotion learning experience;
- we have not yet captured service issues or guarantee dissatisfaction;
- the final memory should be preparedness/value, not a commercial extraction moment.

The explicit ask comes after satisfaction is observed.

---

# 3. Referral Seed inside the Family Emergency Pack

Add a lightweight **Pass Sicurezza / Family Safety Invitation** element.

Recommended V1:

- 2 physical share cards per household Pack;
- each card has a unique/referrer-attributed QR or short code;
- cards are not coupons disguised as education;
- front side explains who it is for: parent/grandparent/babysitter/regular caregiver;
- back side sends the person to a useful Formalife entry surface.

Customer-facing concept:

**“Se conosci una persona che si occupa di un bambino e pensi che possa esserle utile prepararsi meglio, puoi passarle uno di questi inviti. Non devi spiegarle o venderle il corso: il materiale lo farà per te.”**

Do not collect the friend's contact data from the customer without the friend's direct action/consent.

---

# 4. Where the referral QR should land

Do not send every referral directly to checkout.

The landing/router should recognise `source=customer_referral` and allow two paths:

## High intent

**“Voglio vedere date / programma / prezzo”**

-> Full sales surface -> 1/2 Caregivers -> checkout / activation edition.

## Wants to understand first

**“Prima voglio capire meglio”**

-> useful choking/parent-preparedness educational asset -> owned lead -> adaptive Funnel 01 education.

This preserves trust transfer without assuming every referred person is product-aware.

Referral traffic should see customer-protection proof prominently:

- Total Protection;
- Better-Than-Risk-Free;
- scientific responsibility;
- clear fit/non-fit.

These protections also protect the reputation of the person who made the introduction.

---

# 5. Satisfaction gate

Within approximately 24 hours after Full:

1. ask concise experience/service feedback;
2. identify service problem / BTRF request / material dissatisfaction first;
3. only customers showing strong satisfaction become `ADVOCATE_ELIGIBLE`.

Initial conservative rule:

- explicit 9–10/10 satisfaction, **or**
- clearly positive qualitative response without unresolved service issue.

Customers with an unresolved problem:

-> `SERVICE_RECOVERY`

not testimonial/referral automation.

This is an operating threshold to test, not a universal truth.

---

# 6. Explicit referral ask

After `ADVOCATE_ELIGIBLE`, ask directly.

Job:

- trigger recall of an appropriate person;
- make sharing one action;
- remove the burden of composing an explanation.

Recommended question structure:

**“C'è una persona che si occupa regolarmente di un bambino — partner, nonno/a, amico, babysitter — a cui avresti voluto far vedere quello che hai imparato oggi?”**

Then offer:

- WhatsApp share button;
- copy-link;
- physical Pass Sicurezza already in the Pack.

Suggested share message must be editable by customer and should sound like a personal introduction, not a Formalife advertisement.

Example operating direction only:

**“Ho fatto questo corso e mi è stato utile. Ti giro il link perché secondo me potrebbe interessarti; qui puoi prima vedere il materiale e decidere da solo/a.”**

Do not force customer to write names into Formalife forms.

---

# 7. Follow-up timing

Recommended V1 triggers:

## T0 — end of course

Seed only: show the two Pass Sicurezza cards and what they are for.

## T+24h

Feedback / satisfaction gate.

If advocate eligible:

-> explicit referral request.

## T+7d

If advocate-eligible and no referral action:

-> one light reminder tied to useful customer content, not “you forgot to refer someone”.

## Later natural success moments

Referral ask can recur when:

- customer voluntarily sends positive feedback;
- customer completes refresh and reports value;
- customer purchases a relevant second product and reports satisfaction;
- customer tells Formalife they recommended it already.

Do not create high-frequency referral nagging.

---

# 8. Incentive architecture

The referral system should work without an incentive first because trust transfer is the core mechanism.

However Layer 1 supports sustainable incentives when economics justify them.

## V1 — recommended launch

**No automatic cash discount.**

Customer advantage is:

- useful referral tool;
- referred person's strong Total Protection/BTRF;
- Formalife handles education/sales rather than making the customer sell.

Reason:

- establishes an organic referral baseline;
- preserves current Full price;
- avoids adding another variable before referral conversion is readable.

## V1-B — first incentive test after referral baseline exists

Preferred non-price test:

### `GUEST REFRESH PASS`

When one referred person completes a paid Full purchase, referrer earns:

**one Guest Refresh Pass** allowing them to bring one additional regular caregiver to the referrer's 90-minute practical refresh, subject to real capacity.

Why this is strategically coherent:

- reinforces household preparedness;
- perceived value can exceed cash cost;
- current cash cost may be low when refresh capacity is spare;
- it does not discount the core Full;
- it gives the customer something relevant rather than generic points.

Track capacity opportunity cost.

## Alternative test — `GUIDE GIFT`

After a successful referred Full purchase, referrer may receive one Guide gift voucher to pass to another caregiver/prospect.

Current standalone Guide cash fulfilment is approximately EUR 14.15 if Formalife prints/packages/ships it.

This has a higher direct cost but can seed a second acquisition path and closely follows the Layer 1 principle of giving customers referral materials instead of expecting them to become salespeople.

Do not launch both incentive tests simultaneously.

## Future double-sided credit test

Only if needed and economics support it:

- small Training Credit for the new referred prospect;
- small Formalife future credit for referrer after purchase.

Do not start here: discounting obscures the trust-transfer baseline and can reduce contribution unnecessarily.

---

# 9. Attribution model

Every referral relationship should make the following reconstructable:

**referrer -> referred lead -> first meaningful action -> purchase -> contribution -> current state.**

Minimum fields/events:

- `referrer_customer_id`;
- `referral_code`;
- `referred_contact_id` after referred person consents/identifies;
- first landing date;
- initial awareness/intent route;
- `referral_full_checkout_start`;
- `referral_full_purchase`;
- 1/2-Caregiver order;
- booked edition;
- gross price;
- contribution estimate;
- incentive awarded;
- incentive redemption;
- guarantee/refund status.

Never leave acquisition source as generic `passaparola` when a specific referrer can be lawfully/appropriately attributed.

---

# 10. Referral scorecard

Track at least:

- advocate-eligible customers / Full attendees;
- referral ask rate;
- share/referral action rate;
- referred leads per advocate;
- referred lead -> Full purchase conversion;
- average days referral -> purchase;
- 1/2 Caregiver mix;
- contribution per referred customer;
- referral CAC including incentives/materials/admin;
- guarantee/refund rate among referred customers;
- second-generation referrals;
- referrer repeat activity / inactivity.

Primary economic comparison:

**referral acquisition cost + incentive cost vs comparable paid acquisition CAC and contribution.**

---

# 11. Reputational protection rules

The customer is lending Formalife their reputation.

Therefore:

- do not ask dissatisfied customers to refer;
- do not misrepresent availability, scientific certainty or outcomes to referred people;
- show strong risk reversal clearly;
- make opt-out / no-pressure choice obvious;
- fulfil the referred customer's experience especially carefully;
- if a referred customer has a service failure, resolve it quickly because it affects both relationships.

---

# 12. V1 recommendation

Launch referral as:

**valuable Full experience -> Pack contains 2 share tools -> 24h satisfaction gate -> explicit one-click ask to advocates -> referred person enters adaptive referral landing/router -> attribution -> customer-protection delivery -> later test Guest Refresh Pass or Guide Gift incentive.**

This is intentionally more sophisticated than a `PORTA UN AMICO - €X off` coupon while remaining simple enough to operate now.

---

# Layer 1 references

- `merenda/05_acquisizione/referral-e-soddisfazione.md`
- `REASONING_KERNEL.md`

## Related Layer 2

- `FUNNEL_01_FULL_DELIVERY_SYSTEM_V4_FOUNDER_DIRECTED.md`
- `FUNNEL_01_G3_FULL_OFFER_ARCHITECTURE.md`
- `FUNNEL_01_G4_PROOF_ARCHITECTURE.md`
- `B2C_FUNNEL_01_CHOKING_FULL_ADAPTIVE.md`
