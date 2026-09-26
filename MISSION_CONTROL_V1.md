# Formalife Mission Control v1

Status: CURRENT OPERATING ARCHITECTURE — founder-authorized upgrade
Date: 2026-09-26

## Purpose

Formalife Mission Control converts suitable work from chat-by-chat prompting into **bounded autonomous missions with explicit evidence and verification**.

It is not a general permission layer for agents to act freely. It is a control system for giving an AI enough autonomy to complete a well-defined job while preserving business logic, provenance, safety, reversibility and human authority.

The core loop is:

**GOAL -> MISSION CONTRACT -> EXECUTOR -> EVIDENCE/ARTIFACTS -> VALIDATORS -> HUMAN GATE WHEN REQUIRED -> PROMOTION**

Mission Control is engine-agnostic. A mission may be executed by ChatGPT, Codex, Claude, another capable agent, deterministic code, or a combination of these.

## 1. Why this exists

The useful shift is from:

**prompt -> answer -> founder checks everything manually**

into:

**business objective -> bounded autonomous execution -> machine/human verification -> usable result**.

The gain does not come from making the model more verbose or giving it unlimited freedom. It comes from making the task state, evidence, actions, invariants and acceptance criteria explicit enough that long-horizon work can be trusted and repeated.

## 2. Mission contract

Every autonomous mission must define at minimum:

- `mission_id` and version;
- business objective and why it matters;
- expected decision/economic outcome;
- scope and explicit non-goals;
- source-of-truth inputs;
- authorized tools/actions;
- forbidden actions;
- required outputs/artifacts;
- evidence requirements;
- invariants that may not be violated;
- validators and acceptance criteria;
- stop conditions;
- human approval gates;
- resource/cost ceiling when material;
- retry policy;
- final state and next decision.

A mission without measurable acceptance criteria is a task request, not an autonomous mission.

## 3. Runtime roles

### 3.1 Mission Controller

Loads the contract, resolves dependencies, checks permissions and determines whether the mission is ready to run.

### 3.2 Executor

Performs the substantive work. It may research, reason, write code, edit a branch, analyze data or generate an internal asset only within the authorized action envelope.

### 3.3 Evidence Ledger

Records the factual basis used by the Executor. Material claims must be traceable to a source, repository state, dataset, tool result or explicit founder decision.

The ledger separates:

- VERIFIED FACT;
- CURRENT CANONICAL DECISION;
- FOUNDER INPUT;
- DERIVED RESULT;
- INFERENCE;
- HYPOTHESIS;
- UNKNOWN.

### 3.4 Validator

Tests the output against the contract. Validation should be as deterministic as possible.

Validator classes:

1. **structural** — schema, required files, formatting, completeness;
2. **deterministic** — tests, calculations, duplicate checks, hashes, CI, constraints;
3. **evidence** — every important factual claim has adequate provenance and no source contradiction is ignored;
4. **semantic/business** — output respects current strategy, offer stage, qualification state and domain rules;
5. **independent review** — for high-impact work, a second reasoning pass should try to disprove the Executor's result rather than merely polish it.

The Executor must not silently self-certify a result when an independent or deterministic validator is available.

### 3.5 Human Gate

Founder approval remains mandatory where the result creates external commitment, meaningful spend, irreversible change or material strategic choice.

## 4. State machine

Canonical mission states:

`DRAFT -> READY -> RUNNING -> VERIFYING -> PASS | PARTIAL | FAIL | NEEDS_HUMAN -> PROMOTED | CLOSED`

Rules:

- `PASS` means acceptance criteria are satisfied, not that the output has been externally deployed;
- `PROMOTED` means the accepted result has been written into the appropriate canonical system or explicitly deployed;
- `NEEDS_HUMAN` is a valid success path when the contract intentionally contains a decision gate;
- a mission may stop `PARTIAL` when usable evidence was produced but one or more criteria remain unmet.

## 5. Action-risk tiers

### T0 — read/reason

Examples: research, repository reads, analysis, internal recommendations.

May execute autonomously inside the contract.

### T1 — reversible internal artifact

Examples: draft document, generated report, local file, Git branch, draft code, draft personalized sales asset.

May execute autonomously when reversible and isolated from production/current truth.

### T2 — canonical internal write

Examples: merge into a canonical repo, modify CRM/system-of-record data, alter an approved operating rule.

Requires the write-back rules of the affected system and human approval when the change is strategic or not already explicitly authorized.

### T3 — external commitment

Examples: send an email/message, publish a customer-facing page, spend money, place an order, change live pricing, sign/accept terms, submit a regulated/public filing.

Requires explicit human approval unless a separate standing authorization defines the exact permitted action and limits.

## 6. Non-negotiable invariants

1. **No source-of-truth substitution.** Current Formalife repository rules continue to apply.
2. **No silent inference promotion.** Hypotheses do not become facts because an agent repeated them.
3. **No action beyond authorization.** Tool access does not imply permission.
4. **No hidden business-rule bypass.** A generated asset may not jump required qualification, scientific-review, procurement or pricing gates.
5. **Fail closed on material uncertainty.** Missing critical evidence results in `NEEDS_HUMAN`, `PARTIAL` or `FAIL`, not invented certainty.
6. **Prefer reversible work first.** Branch/draft/sandbox before production.
7. **Validator independence increases with consequence.** The more material the decision, the less acceptable pure self-review becomes.

## 7. Mission design rule

Autonomy is appropriate when the job is:

- bounded;
- evidence-accessible;
- decomposable;
- objectively or semi-objectively verifiable;
- reversible until the final gate;
- valuable enough that repeated manual prompting is wasteful.

Autonomy is inappropriate when the core uncertainty is an unresolved founder choice, an unknown market fact that requires real-world contact, a legal/medical judgment requiring a professional, or a strategic question whose causal prerequisites have not been established.

## 8. Output package

Each completed mission should leave a compact reusable package:

- `mission.yaml` — contract and state;
- `evidence.md` or machine-readable evidence ledger;
- final artifact(s);
- validator result;
- unresolved assumptions;
- recommended next action.

Large raw data remains in the system appropriate to that data and is referenced rather than copied unnecessarily into Company OS.

## 9. First implementation pattern

The first live benchmark combines Mission Control with personalized B2B sales assets:

**Mission SALES-001 — Family Polo pre-qualification Fit Brief**

The mission may autonomously:

- research the prospect from reliable/current public and canonical Formalife sources;
- construct a prospect dossier;
- distinguish verified facts from inferred fit;
- generate a concise personalized fit brief;
- check every material external claim against the evidence ledger;
- verify that the asset does not present an unqualified commercial proposal or fixed institutional price;
- produce a draft artifact for founder review.

It may not autonomously:

- invent project budget availability;
- state that Formalife is eligible/rendicontabile without evidence;
- quote a final price;
- publish the page;
- contact the prospect.

Successful completion ends at `NEEDS_HUMAN` with a validated draft. Sending/publishing remains a T3 decision.

## 10. Relationship to existing systems

### Formalife Intelligence

`formalife/intelligence` remains the canonical domain system for parent/market intelligence. Mission Control must consume its outputs through explicit contracts rather than absorb its ontology or pipelines into Company OS.

### Company OS

This repository stores Formalife-specific mission rules, accepted operating contracts, tests/results and strategic state.

### Layer 1

Mission design remains subject to the current Layer 1 rule that automation follows process definition: define state, trigger, next action, owner, data, exception and escalation before automating.

## 11. Initial success metrics

For the first 10 missions, measure:

- founder interventions required per mission;
- percentage ending PASS/NEEDS_HUMAN without rework;
- factual/evidence errors found by validation;
- elapsed human attention versus the previous workflow;
- material errors that escaped validators;
- reuse of the same mission template;
- whether the mission produced an actual decision, artifact or operational improvement rather than activity.

Do not scale mission count merely because agents can run. Scale only mission classes that repeatedly produce correct, decision-useful outputs.

## 12. Revision condition

Revise this architecture when real mission results show that:

- a required field does not affect reliability;
- an important failure mode is not captured;
- human gates are unnecessarily restrictive or insufficient;
- validator design fails to catch recurring errors;
- a stable mission class deserves deterministic orchestration rather than prompt-level execution.

## Provenance

- Founder authorization to implement the autonomous-mission and personalized-sales-asset upgrade: 2026-09-26 Project conversation.
- Current Formalife source-of-truth/runtime rules: `PROJECT_BOOTSTRAP.md`, `CHATGPT_RUNTIME_CONTRACT.md`.
- Current automation doctrine: Layer 1 `REASONING_KERNEL.md`, section 11.
- First commercial benchmark constraints: `CENTRI_FAMIGLIA_OUTREACH_TEST_V1.md`.
