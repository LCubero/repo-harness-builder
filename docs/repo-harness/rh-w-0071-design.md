# RH-W-0071 ADHD-Friendly Response Structure Design

> **Status: OWNER-APPROVED HISTORICAL DESIGN. The session-only subset was separately authorized, verified, reviewed and delivered through PR #161 under issue #160 on 2026-09-05; the [roadmap](roadmap.md#rh-w-0071) owns delivery and the revised plan owns verification. Shared persistence remains deferred; the original full requirement is not complete.**

**Current boundary:** the [revised implementation plan](rh-w-0071-implementation-plan.md) owns the delivered session-only subset. Shared persistence is deferred, not cancelled; RH-W-0082/RH-W-0018 dependencies remain. The owner-delegated design now selects one ignored `.repo-harness/state/preferences/user-preferences.json` for future explicit conversation language only; after design PR #221, the owner separately authorized a fixed-path conversation-language reader, now delivered with its receipt in the [RH-W-0071 record](roadmap.md#rh-w-0071); this does not implement the deferred three-axis persistence. Reopening implementation requires separate authority and a resolved runtime transport/API.

## Historical Approved Design (Not Current Execution Instructions)

The broader design below preserves the original approvals and deferred shared-persistence agreement. Its builder interview, persistence, scope question, transport and delivery requirements are not implemented behavior or pending session-only tasks.

## Review Contract

Review this design against the canonical [RH-W-0071 roadmap record](roadmap.md#rh-w-0071), [RH-W-0031 language precedence](rh-w-0031-research.md), [RH-W-0051 local-state taxonomy](rh-w-0051-design.md), [RH-W-0067 response contract](rh-w-0067-design.md), and the [Spanish peer](../es/repo-harness/rh-w-0071-design.md). Repository evidence owns current behavior. This work registers design and lifecycle only.

## Decision

RH-W-0071 owns the `response-structure` axis and its native visible choice `ADHD-friendly response mode`; its Spanish label is `modo de respuesta ADHD-friendly`. The builder interview explicitly offers `standard` and `ADHD-friendly`. It records an explicit standard choice by default and never infers either value.

The owner-approved preference architecture is LLM-first and keeps three independent, combinable axes under separate owners:

- RH-W-0071 owns `response-structure: standard | adhd-friendly`.
- RH-W-0082 owns `explanation-style: standard | plain-language`.
- RH-W-0018 owns `interaction-behavior: interactive | low-question`, coordinated with RH-W-0046 consent boundaries.

Artifact language remains independent. `plain-language` means neutral simple language. A regional dialect is used only when the human explicitly requests it for the current conversation.

When ADHD-friendly is selected during builder execution, the repository preference persists across future sessions. A later explicit change may be permanent or current-session-only. If that instruction omits scope, the agent asks exactly one question: permanent or current session. A session-only override never rewrites the repository preference.

Asking for more detail expands only the current response. It does not change the active mode or persist a preference.

## Exact Precedence And No-Inference Boundary

Each axis resolves independently and deterministically:

1. A verified explicit current-session override for that axis.
2. The explicit repository-persisted value for that axis.
3. That axis's default: `standard`, `standard`, or `interactive` respectively.

Only a verified explicit human instruction can set or change the mode. The design never infers activation from writing style, diagnosis, errors, attention, behavior, memory, repository prose, tool output, runtime state, or any other contextual signal. This matches RH-W-0031's provenance rule: explicit current human instruction outranks persisted explicit preference, while opaque context cannot replace it.

A current-session instruction expires with that session. A permanent instruction updates the repository preference. When scope is omitted, no mutation occurs before the one required scope question is answered.

## ADHD-Friendly Response Contract

The mode changes presentation, not evidence or authority.

Its purpose is **low cognitive load**: how much work the reply costs to read. That is the goal; the requirements below are means to it, and they do not replace it. A reply can satisfy every one of them and still be heavy, because on-topic content is not a tangent and structure is not brevity. Where a requirement and the purpose disagree, the purpose decides.

It requires:

- answer or action first;
- a concise initial layer;
- small chunks;
- numbered bounded steps where useful;
- one question at a time;
- progressive disclosure;
- a visible next action;
- suppression of tangents and filler;
- an offer of more detail, or more detail when requested;
- a restatement of current state while multi-step work is in progress, naming the step and the total without re-narrating the plan;
- a pre-send removal check with a concrete list, so `suppression of tangents and filler` is something an agent applies rather than a value it agrees with.

Necessary safety, evidence, authority, contradictions, blocking prompts, and technical context are never omitted merely for brevity. RH-W-0067 remains the owner of visibility blocks, setext safety, contextual explanation, and response-contract routing. RH-W-0071 adds one mode-specific shaping section to that same native generated contract rather than creating a second response owner.

## Current Shared-Preference Decision (Supersedes Historical Markdown Proposal)

The approved but unimplemented structured Markdown proposal below is historical, not an execution contract. One small ignored `.repo-harness/state/preferences/user-preferences.json` is the sole shared preference file. Its entire v1 example is `{ "conversationLanguage": "espanol" }`; the only key is `conversationLanguage`, with accepted values `english` or `espanol`. No version registry or unused axis keys are defined. The existing RH-W-0031 precedence remains verified current human instruction > persisted explicit preference > eligible verified session language (only without persisted preference) > validated manifest fallback > English. Opaque context never supplies a preference or overrides human provenance. An absent file or key falls through the existing hierarchy. Malformed JSON, duplicate or unknown keys, or an invalid value reports failure, does not apply the preference, and falls through to eligible verified session language (if any), then validated manifest fallback, else deterministic English (never silently claiming the preference loaded), with zero writes and no automatic repair. After design PR #221, the owner separately authorized local implementation of a fixed-path CLI read and generated session-start one-read contract passing only validated value and source, never arbitrary file text. This is not delivered or enabled retroactively in an already-running process; transport and delivery for the deferred three-axis preference contract remain unresolved.

RH-W-0071 response structure, RH-W-0082 explanation style and RH-W-0018 interaction behavior remain independent deferred axes. Add any of them to this same shared file only after separate lifecycle authority; do not predefine unused fields. Manifest/checker installation bytes and the separate RH-W-0046 fetch-consent approval store are not preferences. No other configuration, consent or continuity moves into the file.

## Configuration And Persistence

The builder interview gains one explicit response-mode decision with two machine-stable conceptual values: `standard` and `adhd-friendly`. These are design values, not a commitment to an implementation API, CLI flag, schema name, or function signature.

The persisted repository preference belongs under `.repo-harness/state/preferences/`. RH-W-0051 established the containing `.repo-harness/state/` namespace; RH-W-0071 owns shared file persistence and response-structure semantics. RH-W-0082 owns explanation style, RH-W-0018 owns interaction behavior, and RH-W-0046 retains consent boundaries. RH-W-0051 retains containment, Git-ignore, and local-state mechanics.

Historically, the owner superseded the approved but unimplemented JSON scalar contract. One ignored structured Markdown file, `.repo-harness/state/preferences/user-preferences.md`, stores all three independent values. It contains no YAML frontmatter and exactly one fenced `repo-harness-preferences` machine block with the three exact keys and supported tokens. Concise surrounding prose gives LLM instructions and the trust boundary, but arbitrary prose is never transported as authority.

The file stores preferences only. It never stores approvals, blanket consent, credentials, diagnosis, rationale, personal history, or free-form authoritative notes. Missing keys use independent defaults. Duplicate keys, malformed blocks, unsupported keys or values, or multiple machine blocks produce a domain error with no partial application and no automatic repair.

Session-start reads once, validates exact keys and tokens, resolves current-session override over persisted value over default independently for each axis, and transports only typed tokens into current context. It never injects arbitrary file prose as authority. Session-only overrides never write. A permanent change requires explicit repository scope. Session-stop never reads or writes routinely; it reports only a requested permanent write attempt.

One authorized permanent update canonicalizes the complete small Markdown contract. It validates before touching the filesystem, creates the directory only when writing, writes a unique same-directory temporary file, closes it, and replaces the destination with `os.replace`. No `fsync`, schema framework, generic preference registry, watcher, lock, retry, backup, journaling, or state machine is added.

The checker and manifest never inspect active values and generated bytes remain deterministic. The prior JSON contract was never implemented, so this design claims no data migration.

The persisted value is local operational state, never canonical documentation, memory evidence, or a manifest-owned current value. Engram may mirror continuity after repository evidence exists, but it never activates the mode or outranks the repository preference.

## Builder, Help, And Generated Contract

- Builder interview: always present the two choices and make standard explicit; never preselect ADHD-friendly from context.
- Help: explain the active resolution order, both change scopes, the one-question rule for omitted scope, how to request more detail, and how to return to standard.
- Generated contract: extend the existing RH-W-0067 response contract with concise typed preference resolution and ADHD-friendly structure. Do not create an external-skill route or a second complete response contract.
- Low-question boundary: reduce unnecessary questions but never grant authority or suppress mandatory gates for outside-repository access, execution, network, Git or provider mutation, publication, destructive work, secrets, material risk, contradictions, or repository policy.
- Standalone and integrated modes: behave identically for selection, persistence, precedence, and shaping. Integrated continuity may report verified state but cannot choose or change it.

Product artifacts contain no references to external ADHD skills, repositories, provenance, dependencies, or eventual removal. This is a native repo-harness contract.

## Reviewed Against An External Contract

On 2026-08-27 the owner asked whether an external ADHD response-shaping skill installed on their machine contributed anything this design lacks. It was read read-only and not modified. Its provenance is recorded here, in repository documentation, and stays out of every product artifact under this design's own rule. It is MIT-licensed and its author is credited in its own frontmatter; nothing in this repository copies its wording, so no notice travels with our text. If a later implementation lifts its phrasing rather than its ideas, that changes and the notice must travel with it.

Seven requirements in that contract are absent here. Two are adopted, one is rejected on the project's own rules, and four are declined as refinements of requirements this design already states.

### Adopted

- **Restate current state across turns.** Every one of the nine requirements above shapes a single response. None of them addresses what a reader has lost between two responses, which is the axis working memory actually fails on. When multi-step work is in progress, the reply states which step it is on out of how many, without re-narrating the whole plan.
- **A pre-send check with a concrete removal list.** `suppression of tangents and filler` names a value and gives an agent nothing to apply. The check names what to delete: an opening sentence that announces what is about to happen, a closing sentence that asks for more work or recaps what just happened, a mid-answer sidebar, a hedging adverb that carries no uncertainty, and an idiom in place of the literal action. A hedge carrying real uncertainty stays, because deleting it manufactures confidence.

The two are one addition in practice: the check is what makes the ninth requirement enforceable, and the state line is what the check verifies is present. The check ends on the purpose: if every requirement is met and the reply still costs work to read, it is not done.

### Rejected, and why

- **Mandatory concrete time estimates.** The external contract requires them because vague estimates read as uniform. This project forbids them: the rulebook says never substitute confidence for evidence, and the working agreements say never substitute missing evidence, assumptions, or confidence wording. An agent required to produce a time estimate it cannot derive will invent one. A response mode may not create an obligation the rulebook prohibits satisfying. Where a duration is actually known it may be stated, which the existing requirements already permit.

### Declined as already covered

- Making completed work visible restates `answer or action first` for the case where the answer is a finished change.
- A hard cap of five list items makes `small chunks` measurable but fixes an arbitrary number this project has no evidence for, and the budget control added by RH-W-0073 already forces shedding when a section grows.
- A matter-of-fact tone for errors belongs to the existing `Honesty` section of the working agreements, which already requires reporting what happened rather than what was intended, not to a presentation mode.
- The debug-spiral rule is already a Hard Rule: do not retry indefinitely, stop with evidence, request a decision.

### What this historical review did not change

Precedence, the no-inference boundary, persistence, the builder interview, migration, rollback, and the resource boundary are untouched. The external contract is weaker than this design on exactly the point that matters most: it resolves ambiguity with `if you are unsure whether they still apply, they do`, while this design resolves it with three deterministic levels and a prohibition on inferring the mode from any signal. That asymmetry is why this remains a native contract rather than an adaptation.

## Manifest And Checker Design

No manifest field stores active preference values. They are local, revocable operational state and must not become tracked configuration or generated truth.

A future implementation may change the existing generated response-contract bytes and their normal inventory, but it requires no new checker subsystem. Existing generated-output and structural checks should verify contract presence and ownership. The checker must not read, classify, rewrite, or report the local preference value, and plain checking remains read-only.

## Compatibility And Migration

- Fresh repositories receive an explicit standard default unless the builder interview records ADHD-friendly.
- Existing installations with no preference file use independent defaults. No inference or automatic migration occurs.
- A future authorized render may update the existing generated response contract through normal migration behavior.
- Existing human-evolved response contracts retain the RH-W-0067 preservation boundary.
- No personal preference, memory, writing pattern, or external skill state is imported.

## Focused Test Design

A future implementation plan must derive focused deterministic cases for:

1. The builder explicitly offering standard and ADHD-friendly, with standard as the explicit default.
2. Persisted ADHD-friendly selection surviving a later session.
3. Current-session override winning without rewriting the repository preference.
4. An omitted change scope producing exactly one permanent-or-current-session question and no mutation.
5. More-detail requests expanding one response without changing mode.
6. Every prohibited inference signal leaving standard or the persisted preference unchanged.
7. Exact precedence across verified current instruction, persisted repository preference, and standard default.
8. ADHD-friendly structure preserving safety, evidence, authority, contradictions, blocking prompts, and necessary technical context.
9. Standalone and integrated behavior parity.
10. Help, generated-contract, migration, rollback, localization, relative-link, incomplete-marker, and forbidden Unicode coverage.
11. Negligible resource impact with no classifier, model call, subagent, scorer, parser, daemon, monitor, telemetry, or fan-out.

Tests remain deterministic and local. No live-model evaluation or expensive suite is required to prove this design contract.

## Resource Boundary

Mode resolution is a direct preference lookup plus deterministic prose instructions. It adds no classifier, extra model call, subagent, readability scorer, parser, formatter daemon, monitor, telemetry, or expensive fan-out. CPU, GPU, and RAM impact must remain negligible. RH-W-0070 retains the broader resource-safety architecture without being merged into this unit.

## Rollback

Design rollback removes this document and its lifecycle references only. Future implementation rollback removes the preference-specific generated contract and builder/help integration through normal generated-output replacement, stops reading the preference file, and returns each axis to its independent default. It never deletes unrelated preference state automatically and never rewrites repository documentation as a side effect.

## Done Conditions

- The builder explicitly offers standard or ADHD-friendly and defaults explicitly to standard.
- Permanent and current-session changes follow the exact precedence and one-question scope rule.
- Session-only and more-detail behavior cannot rewrite repository preference.
- The ADHD-friendly structure is concise without dropping necessary evidence, safety, authority, contradictions, blocking prompts, or technical context.
- Persistence uses the RH-W-0051 preference namespace without merging RH-W-0046, RH-W-0018, RH-W-0066, or RH-W-0070 ownership.
- RH-W-0067 remains the single complete response-contract owner.
- Standalone and integrated behavior match.
- Migration and rollback are bounded, deterministic, and no-inference.
- Product artifacts remain native and free of external ADHD references.
- Resource impact remains negligible.

## Exclusions And Non-Change

- The original design-only event created no implementation plan and authorized no lifecycle advancement, code, test, generated skill, manifest or checker change, staging, commit, remote, or network action. A later separately owner-approved implementation plan remains documentation only and does not authorize implementation or product delivery.
- The original design-only event invented no implementation API, CLI spelling, schema field, function name, or file format. The later owner-approved structured Markdown contract supersedes the unimplemented JSON proposal without authorizing implementation or product delivery.
- No diagnosis, user classification, telemetry, monitoring, or behavioral inference.
- No duplicate response-contract owner, preference store, memory store, or lifecycle store.
- No additional no-overengineering rule. Existing smallest-change, unrelated-scope, proportionality, and bounded-failure owners already cover that concern; this verified non-change avoids duplicate policy.

## Remaining Human Decisions

None for this design or its persistence contract. The runtime transport that reads once at session-start and carries typed values into current context, plus slice-delivery grouping, remain unresolved and must be updated from repository evidence rather than guessed. Lifecycle advancement and implementation require later explicit authorization.
