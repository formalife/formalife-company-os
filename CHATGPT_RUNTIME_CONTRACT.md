# ChatGPT Runtime Contract — Formalife

Status: CURRENT OPERATING CONTRACT

Purpose: make live-repository usage observable and fail-closed for substantive Formalife work without forcing unnecessary full-context reloads.

This contract governs the ChatGPT Project runtime. It does not modify Layer 1 doctrine.

## 1. Core invariant

**NO LIVE REPO, NO SUBSTANTIVE FORMALIFE ANSWER.**

A substantive Formalife answer must include at least one successful live GitHub retrieval in the current turn before the answer is finalized.

Chat history, model memory, previous-turn retrievals, cached summaries and general knowledge may guide retrieval, but they do not satisfy this gate and must not replace GitHub live as the source of current Formalife truth.

## 2. What counts as substantive

Treat a task as substantive when the answer depends materially on current Formalife facts, decisions, architecture or doctrine, including:

- strategic diagnosis or recommendation;
- interpretation of current company state;
- claims about what is currently decided, canonical, superseded or open;
- pricing, offer, acquisition, sales, delivery, retention, economics, capacity or scale decisions;
- architecture, routing, governance or knowledge-base decisions;
- any write-back to Layer 2;
- any recommendation that could reasonably become a company decision.

Normally non-substantive:

- pure rewriting, translation or formatting of text already supplied by the founder;
- generic knowledge questions that do not depend on Formalife state;
- product/UI questions about ChatGPT or other software that do not require Formalife repositories;
- casual conversation.

When uncertain, treat the task as substantive.

## 3. Two-stage retrieval model

### A. Task-entry bootstrap

At the start of a new substantive Formalife task or decision block:

1. live-read `PROJECT_BOOTSTRAP.md`;
2. live-read this file;
3. live-read `LAYER1_REF.md`;
4. live-read only the relevant Layer 2 current files;
5. for strategic/diagnostic reasoning, live-read Layer 1 `REASONING_KERNEL.md`;
6. retrieve progressively only the specialist canonical doctrine required by the decision.

Do not preload the five full Layer 1 control-plane files unless governance/audit/fallback requires them.

### B. Turn-local freshness

For every later substantive reply in the same decision block:

- perform at least one fresh live GitHub read in that same turn from the current file(s) materially used;
- reread Layer 1 when the new reply depends on doctrine, changes diagnosis, or makes a new strategic recommendation;
- do not mechanically reload the full kernel when the new reply is only a narrow factual continuation and no Layer 1 reasoning changed;
- before any write-back, reread every affected live file immediately before editing.

This preserves freshness without turning every follow-up into a full bootstrap reload.

## 4. Fail-closed behavior

If required GitHub retrieval is unavailable, unauthorized or fails:

- do not present repository-dependent claims as current or canonical;
- state `Repo preflight: BLOCKED` and the reason briefly;
- limit the response to clearly labeled non-canonical reasoning, hypotheses or generic guidance when useful;
- never silently substitute chat memory for live repository state.

If the founder explicitly asks not to access GitHub, a repo-dependent answer may only be framed as hypothetical/non-canonical.

## 5. Visible preflight marker

Every substantive Formalife answer must make successful live access observable with a compact marker, for example:

`Repo preflight: LIVE — L2: CURRENT_STATE; L1: REASONING_KERNEL + SALES.PAIN_QUALIFICATION`

Rules:

- never emit `LIVE` unless at least one successful GitHub retrieval occurred in the current turn;
- list only the materially used files/nodes, not every incidental read;
- use `L2` and `L1` to make layer provenance visible;
- for high-impact architecture, governance or write-back work, include the relevant short commit/head SHA when practical;
- if Layer 1 was genuinely not needed for a substantive factual continuation, say only what was read, e.g. `Repo preflight: LIVE — L2: CURRENT_STATE`.

The marker is observability, not evidence by itself. The answer must still follow source-of-truth and provenance rules.

## 6. ChatGPT / Codex operating boundary

Use ChatGPT as the default **control plane** for Formalife:

- founder interview;
- classification of FACT / ASSET / CONSTRAINT / HYPOTHESIS / LEGACY DECISION / OBSERVATION / OPEN QUESTION / DECISION / TEST / RESULT;
- strategic diagnosis;
- Layer 1 application to Layer 2;
- decision formation and challenge;
- selective repository reading;
- small or moderate write-backs where the change is already well-defined.

Use Codex as the preferred **execution plane** when work is repository-native and operationally heavy, especially:

- multi-file refactors or migrations;
- scripts, validators and CI;
- large routing/index changes;
- repeated local test/debug loops;
- branch/worktree-intensive engineering;
- changes where command execution and repository-wide consistency dominate the task.

Codex does not become an independent source of strategy. The intended flow is:

**Founder ↔ ChatGPT decision/control plane → Codex execution when useful → GitHub canonical → ChatGPT rereads GitHub live before treating the result as current.**

## 7. Write-back and canonicality

A change is not current Formalife truth merely because it appeared in chat or a Codex workspace.

For important decisions:

1. founder approval or clear authorization;
2. live reread of affected files;
3. minimal write-back with status/provenance preserved;
4. GitHub update/merge;
5. subsequent reasoning rereads the canonical GitHub state.

## 8. Project Instructions minimum clause

The ChatGPT Project Instructions should contain, directly or equivalently, this compact clause:

> For every substantive Formalife task, follow Layer 2 `PROJECT_BOOTSTRAP.md` and `CHATGPT_RUNTIME_CONTRACT.md`. NO LIVE REPO, NO SUBSTANTIVE FORMALIFE ANSWER. A successful GitHub retrieval must occur in the current turn before any repository-dependent diagnosis, recommendation or claim of current truth. Never use chat memory as a substitute for GitHub live. Surface a compact `Repo preflight: LIVE` marker listing the materially used L2/L1 sources; if retrieval fails, use `Repo preflight: BLOCKED` and do not claim current canonical state.

This clause exists because repository files cannot enforce Project UI instructions by themselves. The Project Instructions are the trigger; this contract is the canonical detailed behavior they point to.
