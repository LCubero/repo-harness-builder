# RH-W-0071 ADHD-Friendly Response Mode Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Review Contract

Review this design against the canonical [RH-W-0071 roadmap record](roadmap.md#rh-w-0071), [RH-W-0031 language precedence](rh-w-0031-research.md), [RH-W-0051 local-state taxonomy](rh-w-0051-design.md), [RH-W-0067 response contract](rh-w-0067-design.md), and the [Spanish peer](../es/repo-harness/rh-w-0071-design.md). Repository evidence owns current behavior. This work registers design and lifecycle only.

## Decision

The native visible choice is `ADHD-friendly response mode`; its Spanish label is `modo de respuesta ADHD-friendly`. The builder interview explicitly offers `standard` and `ADHD-friendly`. It records an explicit standard choice by default and never infers either mode.

When ADHD-friendly is selected during builder execution, the repository preference persists across future sessions. A later explicit change may be permanent or current-session-only. If that instruction omits scope, the agent asks exactly one question: permanent or current session. A session-only override never rewrites the repository preference.

Asking for more detail expands only the current response. It does not change the active mode or persist a preference.

## Exact Precedence And No-Inference Boundary

Mode resolution is deterministic:

1. A verified explicit current-session response-mode instruction.
2. The explicit repository-persisted response-mode preference.
3. Explicit standard default.

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

## Configuration And Persistence

The builder interview gains one explicit response-mode decision with two machine-stable conceptual values: `standard` and `adhd-friendly`. These are design values, not a commitment to an implementation API, CLI flag, schema name, or function signature.

The persisted repository preference belongs under `.repo-harness/state/preferences/`, the location reserved by RH-W-0051. RH-W-0071 owns the response-mode value and update semantics. RH-W-0051 owns path, containment, Git-ignore, and local-state mechanics. RH-W-0046 and deferred RH-W-0018 retain consent and preference architecture without absorbing this mode or sharing lifecycle.

The persisted value is local operational state, never canonical documentation, memory evidence, or a manifest-owned current value. Engram may mirror continuity after repository evidence exists, but it never activates the mode or outranks the repository preference.

## Builder, Help, And Generated Contract

- Builder interview: always present the two choices and make standard explicit; never preselect ADHD-friendly from context.
- Help: explain the active resolution order, both change scopes, the one-question rule for omitted scope, how to request more detail, and how to return to standard.
- Generated contract: extend the existing RH-W-0067 response contract with concise mode resolution and ADHD-friendly structure. Do not create an external-skill route or a second complete response contract.
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

### What this review did not change

Precedence, the no-inference boundary, persistence, the builder interview, migration, rollback, and the resource boundary are untouched. The external contract is weaker than this design on exactly the point that matters most: it resolves ambiguity with `if you are unsure whether they still apply, they do`, while this design resolves it with three deterministic levels and a prohibition on inferring the mode from any signal. That asymmetry is why this remains a native contract rather than an adaptation.

## Manifest And Checker Design

No manifest field stores the active response-mode preference. That value is local, revocable operational state and must not become tracked configuration or generated truth.

A future implementation may change the existing generated response-contract bytes and their normal inventory, but it requires no new checker subsystem. Existing generated-output and structural checks should verify contract presence and ownership. The checker must not read, classify, rewrite, or report the local preference value, and plain checking remains read-only.

## Compatibility And Migration

- Fresh repositories receive an explicit standard default unless the builder interview records ADHD-friendly.
- Existing installations with no response-mode preference resolve to explicit standard. No inference or automatic migration occurs.
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

Design rollback removes this document and its lifecycle references only. Future implementation rollback removes the mode-specific generated contract and builder/help integration through normal generated-output replacement, stops reading the response-mode preference, and returns resolution to explicit standard. It never deletes unrelated preference state automatically and never rewrites repository documentation as a side effect.

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

- No implementation plan, lifecycle advancement, code, test, generated skill, manifest or checker change, staging, commit, remote, or network action.
- No implementation API, CLI spelling, schema field, function name, or file format is invented by this design.
- No diagnosis, user classification, telemetry, monitoring, or behavioral inference.
- No duplicate response-contract owner, preference store, memory store, or lifecycle store.
- No additional no-overengineering rule. Existing smallest-change, unrelated-scope, proportionality, and bounded-failure owners already cover that concern; this verified non-change avoids duplicate policy.

## Remaining Human Decisions

None for this design. The exact persisted file format, implementation API, implementation plan, lifecycle advancement, and implementation require later explicit authorization and repository evidence.
