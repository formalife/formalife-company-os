# Funnel 01 — Referral System V2

Status: CURRENT FOUNDER-APPROVED REFERRAL ARCHITECTURE — supersedes Referral V1; exact creative/landing implementation remains to build and measure
Date: 2026-09-22

Purpose: convert satisfied Full customers into a measurable trust-transfer acquisition channel using a physical/digital referral tool, adaptive routing and a Guide Gift incentive rather than a generic discount.

Governing Layer 1 logic:

**value delivered -> explicit referral opportunity -> facilitated introduction -> referred prospect routed by readiness -> purchase attribution -> useful reward -> second-generation referral potential.**

This file supersedes `FUNNEL_01_REFERRAL_SYSTEM_V1.md`.

---

# 1. Founder decisions now in force

1. **T=0 / end of course:** referral is already explicitly requested through the `Pass Sicurezza`; it is not merely a passive seed.
2. Each Family Emergency Pack includes **2 Pass Sicurezza** referral cards/tools per household/order.
3. The referral QR/link uses a **two-way router**:
   - ready/high-intent -> Full dates/program/price/checkout;
   - not ready / wants to understand first -> educational entry point inside adaptive Funnel 01.
4. **T+24h:** send a simple request for a Google review; do not make another referral request at this point.
5. **T+7d:** make an explicit digital referral request **only to customers who have actually left a review/testimonial** and have no unresolved service issue.
6. When a referred person completes a paid Full purchase, the referrer earns **one free Guida Anti-Panico to give to another family/caregiver**.
7. Do not use an untrained Guest Refresh Pass as the referral reward; refresh is for prior Full participants.
8. Referral must be attributable end to end rather than recorded generically as `passaparola` when the source is known.

---

# 2. Referral mechanism at T=0 — Pass Sicurezza

## 2.1 Physical/digital tool

Family Emergency Pack includes:

- **2 Pass Sicurezza** per household/order;
- unique or referrer-attributed QR/short link/code;
- simple indication of who it may help: parent, grandparent, babysitter or other regular caregiver;
- no requirement that the customer provide the friend's personal details to Formalife.

## 2.2 End-of-course referral ask

ACT 7 should explain the tool clearly and make a real but non-aggressive ask.

Customer-facing direction:

**“Nel vostro Pack trovate due Pass Sicurezza. Se vi viene in mente una persona che si occupa regolarmente di un bambino e a cui potrebbe essere utile prepararsi meglio, passategliene uno. Non dovete spiegarle o venderle il corso: il Pass la porterà nel punto giusto per capire da sola se e come vuole approfondire.”**

The job is not to make the client a salesperson. The job is to trigger a relevant introduction while Formalife handles education and conversion.

---

# 3. Referral landing/router

Referral arrival must be recognised, for example:

`source=customer_referral`

and preserve the referrer's attribution code.

Do **not** assume every referred person is ready for the same commercial step.

## Route A — `SONO PRONTO`

Customer-facing intent:

**“Voglio vedere date, programma e prezzo.”**

Route:

**referral trust surface -> Full sales surface -> 1/2 Caregivers -> confirmed/activation edition -> checkout.**

Show near decision:

- scientific responsibility;
- package before/live/after;
- Total Protection;
- Better-Than-Risk-Free;
- real dates/capacity;
- clear fit/non-fit.

## Route B — `VOGLIO CAPIRE PRIMA`

Customer-facing intent:

**“Prima voglio capire meglio.”**

Route:

**useful choking / pediatric-preparedness educational asset -> owned lead if permission given -> awareness-appropriate Funnel 01 education -> Guide or Full when ready.**

This is the key adaptive advantage of referral:

**trust is transferred, but readiness is still diagnosed.**

---

# 4. T+24h — Google review request

Current founder decision:

**one day after attendance, request a Google review.**

Do not add a referral CTA to this message.

Job:

- capture a current public testimonial/review while the experience is fresh;
- strengthen G4 proof architecture;
- identify advocates for the T+7 referral follow-up.

Operating boundary:

- ask for an honest review rather than prescribing positive wording;
- do not condition a benefit on leaving a positive review;
- if the customer is already in an active service-recovery/BTRF dispute, resolve that issue before ordinary promotional follow-up.

Record where feasible:

- review request sent;
- review observed/confirmed;
- review URL/reference;
- product/version/date;
- unresolved service issue status.

---

# 5. T+7d — explicit referral ask to reviewers/testimonial givers

Eligibility:

- customer attended Full;
- customer left a Google review or other usable testimonial;
- no unresolved service-recovery/BTRF issue.

The review/testimonial acts as a practical advocacy signal; do not infer advocacy merely from an email open or passive engagement.

Recommended question:

**“C'è una persona che si occupa regolarmente di un bambino — partner, nonno/a, amico, babysitter — a cui pensi possa essere utile prepararsi come hai fatto tu?”**

Then offer one-click tools:

- WhatsApp share;
- copy personal referral link;
- reminder that the customer already has two physical Pass Sicurezza.

Editable suggested message direction:

**“Ho fatto questo corso e mi è stato utile. Ti giro il link perché penso possa interessarti: puoi prima capire di cosa si tratta e decidere tu se approfondire.”**

Do not ask the customer to upload/import their contacts.

---

# 6. Referral reward — Guide Gift

## Founder decision

When one referred person completes a **paid Full purchase**, the referrer earns:

**1 Guida Anti-Panico gratuita da regalare a un'altra famiglia/caregiver.**

This is deliberately not a cashback or Full discount.

Strategic job:

- reward successful trust transfer;
- give the advocate another useful referral tool;
- create the possibility of a second-generation Guide -> Funnel 01 relationship;
- preserve the public Full price;
- keep the reward coherent with Formalife's publishing/education system.

## Trigger

Recommended attribution trigger:

`REFERRAL_FULL_PURCHASE_COMPLETED`

Do not award from a click, lead or abandoned checkout.

If the referred Full later receives a full BTRF refund, Formalife should initially treat the reward status as a measurable operating exception rather than silently multiplying gifts; final clawback/no-clawback rule remains an implementation decision.

## Known cost

Current planning cash fulfilment for a standalone shipped Guide is approximately **EUR 14.15** before CAC/overhead under current founder-provided costs.

Therefore measure:

**Guide Gift cost / completed referred customer contribution**

and compare with paid-acquisition CAC.

## Fulfilment detail still open

Operational implementation may choose:

- Formalife ships the gift directly to the new recipient after recipient consent/address collection;
- referrer receives a gift voucher/code to forward;
- event/partner pickup where useful.

The customer-facing promise is the free Guide; logistical implementation should minimize unnecessary cost/friction without weakening that promise.

---

# 7. Referral state machine

Suggested operating states:

- `REFERRAL_ELIGIBLE_FULL_ATTENDEE`
- `PASS_ISSUED`
- `PASS_VISIT`
- `REFERRAL_READY_ROUTE`
- `REFERRAL_EDUCATION_ROUTE`
- `REFERRAL_LEAD_IDENTIFIED`
- `REFERRAL_FULL_CHECKOUT`
- `REFERRAL_FULL_PURCHASED`
- `GUIDE_GIFT_EARNED`
- `GUIDE_GIFT_ISSUED`
- `GUIDE_GIFT_REDEEMED`
- `REFERRAL_SERVICE_RECOVERY`
- `REFERRAL_REFUNDED`

Do not create manual complexity if the CRM cannot yet support every state; the minimum requirement is that the economic chain remains reconstructable.

---

# 8. Attribution model

Minimum reconstructable chain:

**referrer -> referred visit/lead -> readiness route -> first meaningful action -> Full purchase -> contribution -> Guide Gift -> recipient/second-generation outcome.**

Minimum useful fields/events:

- `referrer_customer_id`;
- `referral_code`;
- Pass source: physical / WhatsApp / copied link;
- referred visitor/contact ID after voluntary identification;
- ready vs education route;
- first landing date;
- Full checkout start;
- Full purchase;
- 1/2-Caregiver order;
- booked edition;
- gross price;
- estimated contribution;
- BTRF/refund status;
- Guide Gift earned/issued/redeemed;
- second-generation Guide -> Full progression if it occurs.

Do not leave a known specific referral as generic `word of mouth`.

---

# 9. Referral automation sequence

## T=0 — in-room / Pack

- instructor explains 2 Pass Sicurezza;
- explicit referral invitation;
- no long pitch;
- record Pack/Pass issuance where practical.

## T+24h

- Google review request only;
- route service issues to service recovery.

## T+7d

For confirmed reviewer/testimonial giver only:

- explicit referral request;
- personal referral link;
- WhatsApp share;
- remind them of physical Pass cards.

## On successful referred Full purchase

- mark attribution;
- issue Guide Gift entitlement;
- communicate how to gift/redeem;
- track fulfilment cost.

## Later

Do not nag at fixed high frequency.

Natural future referral moments may include:

- successful refresh;
- unsolicited positive feedback;
- successful second purchase;
- customer reports having already recommended Formalife.

---

# 10. Referral scorecard

Track:

- Full attendees receiving Pass;
- Pass visits / attendees;
- referral visits by channel;
- ready-route vs education-route split;
- referral lead identification rate;
- referred lead -> Full purchase conversion;
- days referral -> purchase;
- contribution per referred Full;
- Guide Gift cost per referred Full;
- total referral CAC;
- BTRF/refund rate for referred customers;
- Guide Gift redemption;
- second-generation Guide -> lead -> Full conversion;
- referral share of total new Full buyers.

Primary economic question:

**does one euro invested in Pass + Guide Gift + operations acquire higher-quality contribution more efficiently than the next-best acquisition route?**

---

# 11. Reputational protection

The referrer lends Formalife part of their reputation.

Therefore:

- make the Pass useful and low-pressure;
- let the referred person choose ready vs education;
- show Total Protection/BTRF clearly;
- never make unsupported clinical/outcome claims;
- resolve referred-customer service failures quickly;
- never require the referrer to act as the closer;
- do not collect third-party contact data without direct consent.

---

# 12. Canonical V2 referral flow

**Full delivered -> ACT 7 explicit Pass Sicurezza request -> referred person chooses READY or UNDERSTAND FIRST -> adaptive Funnel 01 -> Full purchase -> referrer earns Guide Gift -> Guide can create second-generation Funnel 01 entry.**

Parallel follow-up:

**T+24 Google review request -> T+7 referral ask only to actual reviewer/testimonial giver.**

This is the current founder-approved referral architecture.

---

# Layer 1 references

- `merenda/05_acquisizione/referral-e-soddisfazione.md`
- `REASONING_KERNEL.md`

## Related Layer 2

- `FUNNEL_01_FULL_DELIVERY_SYSTEM_V5_CANONICAL.md`
- `FUNNEL_01_G3_FULL_OFFER_ARCHITECTURE.md`
- `FUNNEL_01_G4_PROOF_ARCHITECTURE.md`
- `B2C_FUNNEL_01_CHOKING_FULL_ADAPTIVE.md`
