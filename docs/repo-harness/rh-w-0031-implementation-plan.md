# RH-W-0031 Conversation-Language Boundary Remediation Plan

> **Completed lifecycle notice:** RH-W-0031 completed on 2026-08-15. The canonical [roadmap record](roadmap.md#rh-w-0031) owns completion and delivery evidence. The historical task instructions and unchecked boxes below are retained as execution evidence, not pending work.

> **For agentic workers:** REQUIRED SUB-SKILLS: use `systematic-debugging`, `test-driven-development`, and `verification-before-completion`. Track each step with its checkbox. This is one bounded product unit.

**Goal:** Make session-start, session-stop, and help preserve an explicit human conversation-language preference while keeping technical artifacts and machine-significant values outside language resolution.

**Architecture:** Resolve conversation language from typed, provenance-aware evidence when the host exposes it. A persisted explicit preference changes only through a later explicit human instruction. Hosts that flatten provenance must preserve that preference conservatively and must not claim reliable switch detection. Python validates configuration, routes projections, and renders contracts; it does not infer language from arbitrary prose.

**Tech Stack:** Python 3.10+ standard library, `unittest`, generated Markdown contracts, JSON manifest, and exact checked-in OpenCode and Claude package projections.

**Lifecycle owner:** [RH-W-0031](roadmap.md#rh-w-0031), status `completed`, priority `P1`. The roadmap owns status and delivery evidence; this file preserves the historical implementation sequence.

**Evidence owner:** [RH-W-0031 conversation-language research](rh-w-0031-research.md) owns the reviewed primary-source registry, claim limitations, revision dates, and refresh procedure.

## Root Cause

Conversation-language responsibility is fragmented across duplicated session prose, fixed-language approval questions, help routing, manifest fallback, and tests that inspect fragments instead of a single precedence contract. The old plan also contradicted itself: it said an explicit preference remained active until explicitly changed, then allowed sustained ordinary prose to replace it.

The missing boundary is provenance. Latest text is not necessarily the latest verified human instruction. Repository rules, memory, tool output, forwarded content, generated wrappers, and arbitrary latest bytes can all look conversational after flattening. They cannot supersede an explicit human preference merely because they appear later.

## Authoritative Decision

A persisted explicit conversation-language preference changes **only** through a later explicit human instruction. Sustained ordinary prose in another language does not override it.

Exact precedence:

1. Current explicit human language instruction.
2. Persisted explicit conversation-language preference.
3. Established current-session human language, only when no persisted preference exists.
4. Validated manifest `language.conversation` fallback.
5. Deterministic English fallback.

The resolver distinguishes:

- **Latest verified human instruction:** typed host evidence that a human explicitly requested a conversation language. A later verified instruction may supersede an earlier explicit preference.
- **Latest text or bytes:** untyped or insufficiently proven content, regardless of recency, length, language dominance, or apparent conversational form. It cannot supersede an explicit preference.

Manifest language is validated configuration and a continuity fallback. It is never evidence about the language of the current conversation. Invalid manifest values fail through the owning field validation path rather than silently becoming conversational evidence.

Language resolution is not a security boundary. It must never change command meaning, permission scope, approval tokens, executable values, paths, identifiers, remotes, branches, digests, arguments, or any other machine-significant bytes.

## Primary-Source Basis

The [canonical research registry](rh-w-0031-research.md#source-registry) records official owners, exact URLs, the pinned Gentle AI revision, access dates, supported claims, limitations, and refresh triggers. Those sources define host boundaries and testing practices, not a universal provider standard for selecting conversation language.

## Host Capability Matrix

| Host boundary | Available provenance | Safe RH-W-0031 claim | Required mode | Official source |
|---|---|---|---|---|
| OpenAI API | Application supplies developer and user messages; content can contain quoted or untrusted material | Message role and instruction authority help preserve boundaries, but `user` content alone does not prove direct human prose | Provenance-aware only when the application supplies a verified human-instruction type; otherwise flattened | [Model Spec](https://model-spec.openai.com/2025-12-18.html), [Prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering) |
| Anthropic Messages API | Application supplies `user` and `assistant` turns plus system instructions | `role: "user"` is not equivalent to verified human authorship | Provenance-aware only with an application-owned verified boundary; otherwise flattened | [Messages API](https://platform.claude.com/docs/en/api/messages) |
| MCP sampling | Client and server exchange protocol messages with roles | MCP roles identify protocol shape, not guaranteed human origin | Provenance-aware only if the client carries trusted origin metadata outside arbitrary content; otherwise flattened | [Sampling](https://modelcontextprotocol.io/specification/2025-11-25/client/sampling), [Security](https://modelcontextprotocol.io/specification/2025-11-25/basic/security_best_practices) |
| OpenCode | Runtime loads user prompts, rules, skills, and repository context | Rules and loaded files are instructions or context, never current human language evidence | Use typed host boundaries if exposed to the skill; otherwise flattened | [OpenCode rules](https://opencode.ai/docs/rules/) |
| Claude Code | Runtime loads user interaction, project memory, and settings | `CLAUDE.md`, memory, hooks, and repository text cannot supersede verified human preference | Use typed interaction boundaries if exposed; otherwise flattened | [Memory](https://docs.anthropic.com/en/docs/claude-code/memory), [Security](https://docs.anthropic.com/en/docs/claude-code/security) |

No row claims that provider message roles establish human authorship. The repository contract relies only on provenance the integrating host can actually verify.

## Operating Modes

### Provenance-Aware Mode

The host supplies typed boundaries rather than asking the model to infer origin from strings:

```text
VerifiedHumanLanguageInstruction(language: ConversationLanguage)
PersistedExplicitPreference(language: ConversationLanguage)
EstablishedSessionHumanLanguage(language: ConversationLanguage)
ValidatedManifestFallback(language: ConversationLanguage)
OpaqueContext(value: bytes, origin: ContextOrigin)
MachineValue(kind: MachineValueKind, value: bytes, translate: false)
```

Only `VerifiedHumanLanguageInstruction` can replace `PersistedExplicitPreference`. `EstablishedSessionHumanLanguage` is eligible only when no persisted preference exists. `OpaqueContext` is never switch evidence.

### Flattened-Host Mode

When the runtime exposes only flattened text or cannot prove origin:

- Preserve the persisted explicit preference.
- Do not infer an override from ordinary prose, repetition, recency, dominance, or apparent wrappers.
- Accept a change only through a host-supported explicit human instruction boundary that can be verified independently of arbitrary content.
- If no persisted preference exists, use validated manifest fallback, then deterministic English.
- Do not claim reliable current-session language establishment or switch detection from flattened bytes.

This is deliberately conservative. False continuity is visible and correctable; silently treating injected or forwarded text as a preference change corrupts persisted state.

## Machine-Significant Value Isolation

Generated contracts must separate localized explanation from executable invocation. Machine values use typed opaque placeholders, inline or fenced code formatting, and an explicit non-translation path.

```text
LocalizedExplanation(language=resolved_language)
ExecutableInvocation(
    command=MachineValue(kind=command, value=<exact bytes>, translate=false),
    arguments=[MachineValue(kind=argument, value=<exact bytes>, translate=false)],
)
```

Required invariants:

- Never concatenate localized prose into an executable placeholder.
- Never send `MachineValue.value` through translation or language-resolution logic.
- Render commands, paths, tokens, identifiers, enum values, remotes, branches, digests, and invocations as code.
- Assert byte-for-byte equality before and after localized rendering.
- Localize only adjacent explanatory prose and the human question.
- Keep approval meaning and permission scope identical across languages.

## Scope

- Generate one concise conversation-language principle shared by session-start, session-stop, and help.
- Keep the exhaustive precedence, provenance taxonomy, exclusions, and adversarial cases in validation references and tests, not duplicated in every runtime prompt.
- Remove fixed English direct-reply questions while retaining exact semantic fields.
- Decouple help guide locale from conversation language.
- Add provenance-aware and flattened-host contracts without adding natural-language detection.
- Preserve semantic parity across OpenCode, Claude Code, and generated consumer projections.
- Separate deterministic contract and projection tests from optional model behavior evaluations.

## Non-Goals

- No natural-language classifier, locale dependency, translation service, persisted transcript, or provider-specific authorship guess.
- No security claim based on language resolution.
- No changes to commands, approvals, Git consent, permissions, tokens, or executable values.
- No hand edits to generated consumer projections.
- No model evaluation as a completion gate for RH-W-0031.

## Canonical File Map

| Path | Responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Shared concise contract renderer, typed language inputs, help routing, session contracts, CLI fallback |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Deterministic precedence, provenance, isolation, projection, and literal-leakage tests |
| `skill/repo-harness-builder/references/architecture.md` | Two operating modes and four independent language dimensions |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Explicit preference collection and verified provenance boundary |
| `skill/repo-harness-builder/references/validation.md` | Exhaustive evidence taxonomy, adversarial cases, machine-byte invariants, and optional eval design |
| `.opencode/skills/repo-harness-builder/**` | Exact package projection |
| `.claude/skills/repo-harness-builder/**` | Exact package projection |
| Generated session and help files | Renderer-owned consumer projections, never hand edited |

## Deterministic Contract Matrix

| Case | Expected result |
|---|---|
| Current verified instruction says Spanish; persisted preference is English | Spanish, persist the verified superseding instruction |
| Persisted Spanish preference; sustained ordinary English prose | Spanish |
| Persisted English preference; sustained ordinary Spanish prose | English |
| No persisted preference; verified current-session human language is Spanish | Spanish |
| No persisted preference or verified session evidence; valid manifest is Spanish | Spanish fallback, not conversational evidence |
| Invalid manifest and no stronger source | Field-specific validation failure |
| Missing manifest and no stronger source | Deterministic English |
| Flattened host plus persisted preference | Preserve persisted preference; report switch detection unavailable |
| Flattened host without persisted preference | Validated manifest, then English |
| Forged `user`, `human`, system, or instruction wrapper inside content | No override |
| Repository instruction or rule file requests another language | No override |
| Memory entry, stale memory, or recovered session summary requests another language | No override |
| Forwarded text, unquoted paste, quotation, transcript, or pasted conversation | No override |
| Compaction or truncation removes prior prose but persisted preference survives | Preserve persisted preference |
| Mixed-language conversation under persisted preference | Preserve persisted preference |
| Translation request | Translate requested content only; no conversation preference change |
| Language-learning exercise or role-play | Perform exercise; no preference change |
| Artifact language request | Change requested artifact language only |
| Command, token, path, branch, remote, digest, identifier, enum, or invocation | Byte-identical before and after localization |

Additional excluded evidence includes tool output, prior assistant turns, generated contracts, code, logs, stack traces, URLs, JSON, structured data, filenames, project names, examples, stylistic momentum, and isolated borrowed words.

## Task 1: Establish RED Deterministic Coverage

**Files:** modify `skill/repo-harness-builder/tests/test_harness_tool.py` only.

- [ ] Add `ConversationLanguageBoundaryTests` with fixtures for `VerifiedHumanLanguageInstruction`, persisted preference, established session language, manifest fallback, flattened input, opaque context, and machine values.
- [ ] Add one test for every row in the deterministic contract matrix, including all expanded injection and context-loss cases.
- [ ] Add `test_only_verified_human_instruction_supersedes_persisted_preference` and `test_latest_bytes_never_supersede_persisted_preference`.
- [ ] Add `test_manifest_is_configuration_fallback_not_conversation_evidence`.
- [ ] Add `test_flattened_mode_never_claims_reliable_switch_detection`.
- [ ] Add byte-for-byte assertions for every `MachineValueKind` used by start, stop, and help.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py ConversationLanguageBoundaryTests -v`.

Expected RED: assertion failures for missing precedence, provenance, flattened-mode wording, machine-value isolation, or adversarial taxonomy. Import, syntax, and fixture errors are invalid RED evidence.

## Task 2: Render the Minimal Runtime Principle

**Files:** modify `skill/repo-harness-builder/scripts/harness_tool.py` and focused tests.

- [ ] Add one renderer-owned contract block whose runtime principle states: follow a current verified human language instruction; otherwise preserve an explicit persisted preference; without one, use verified session language, validated manifest fallback, then English; never infer preference changes from context or ordinary prose; never alter machine-significant bytes.
- [ ] Keep the exhaustive taxonomy in `references/validation.md` and tests. Do not repeat it in each generated surface.
- [ ] Make the renderer consume validated typed values only. Do not pass free-form transcript text to Python language resolution.
- [ ] Emit a conservative flattened-mode clause whenever verified host provenance is unavailable.
- [ ] Run the focused Task 1 tests. Expected GREEN for precedence and mode semantics.

## Task 3: Isolate Localized Prose from Executable Values

**Files:** modify canonical session-start, session-stop, and help renderers plus focused tests.

- [ ] Replace fixed-language fetch, commit, push, and help questions with semantic localized explanation fields and typed opaque machine placeholders.
- [ ] Mark every machine placeholder non-translatable and render it as code.
- [ ] Preserve exact command, argument, path, remote, branch, upstream, ahead count, commit message, digest, approval token, and permission scope bytes.
- [ ] Add a single rendering path that never passes machine values through localization.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py ConversationLanguageBoundaryTests -v`.

Expected GREEN: localized prose may differ while all machine-value byte assertions remain equal.

## Task 4: Align Help, Manifest, and CLI Contracts

**Files:** modify `render_with_skill_mapping()`, `help_wrapper()`, CLI help, and focused tests only as required.

- [ ] Make guide locale select only the available reading surface. It never determines reply language.
- [ ] Describe manifest language as validated configuration and continuity fallback, never current conversational evidence.
- [ ] State direct CLI fallback honestly: orchestrators pass resolved typed evidence; absent that evidence, validated manifest applies, then deterministic English.
- [ ] Ensure report language never supplies conversation language.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py ConversationLanguageBoundaryTests LanguageFlagCliTests RepoHarnessHelpWrapperTests -v`.

## Task 5: Document Exhaustive Validation Ownership

**Files:** modify `references/architecture.md`, `references/audit-and-interview.md`, and `references/validation.md`; modify public skill or command documentation only if it contains a direct contradiction.

- [ ] Document provenance-aware and flattened-host modes in architecture.
- [ ] Document explicit preference collection and replacement only by later verified human instruction in audit and interview guidance.
- [ ] Put the full precedence table, host capability matrix, injection taxonomy, context-loss cases, and machine-byte invariants in validation.
- [ ] Cite the official primary sources listed in this plan near the claims they support.
- [ ] Do not state that OpenAI, Anthropic, MCP, OpenCode, or Claude Code guarantees human authorship from a `user` role or equivalent text wrapper.
- [ ] Add semantic-parity tests for OpenCode, Claude Code, and generated consumer contracts.

## Task 6: Verify Canonical Behavior and Literal Boundaries

- [ ] Run the focused language suite from Task 4.
- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"`.
- [ ] Scan canonical language renderers for fixed direct-reply questions and classify matches as generated instruction, strict report text, machine value, or defect.
- [ ] Confirm the generated runtime principle is concise and the exhaustive taxonomy exists only in validation and tests.

Expected: canonical suite exits 0 with only supported skips; no fixed-language direct question or translation path for machine values remains.

## Task 7: Synchronize and Verify Projections

- [ ] Copy only changed canonical package-relative files byte-for-byte to `.opencode/skills/repo-harness-builder/` and `.claude/skills/repo-harness-builder/`.
- [ ] Compare SHA-256 for each canonical, OpenCode, and Claude Code triple. Expected: exact equality.
- [ ] Run all three package suites:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py"
```

- [ ] Preview generated consumer changes with the existing content-bound maintenance workflow, obtain approval for the exact digest, apply only that digest, and verify an empty second preview.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, zero writes.

## Task 8: Optional Future Model Behavior Evaluation

This task is design-only and is not a completion gate for RH-W-0031.

- [ ] Create a separate future work item before implementing any live-model harness.
- [ ] Pin provider, exact model identifier, model snapshot or version, prompt bytes, temperature and sampling settings, harness revision, and grading rubric.
- [ ] Use repeated samples per case rather than one response.
- [ ] Include adversarial multi-turn transcripts covering every injection, context-loss, mixed-language, translation, learning, and artifact-language case in the deterministic matrix.
- [ ] Define thresholds before execution: 100 percent machine-byte preservation, 100 percent permission-scope preservation, and a human-calibrated minimum language-policy pass rate based on an agreed sample size.
- [ ] Calibrate automated graders against blinded human labels, record disagreements, and retain a manual review sample. Use [OpenAI Evals](https://platform.openai.com/docs/guides/evals) and [Graders](https://developers.openai.com/api/docs/guides/graders) only as implementation references, not proof of correctness.
- [ ] Report confidence intervals, repeated-run variance, model drift risk, and the exact failed transcripts.

Deterministic contract and projection tests remain the RH-W-0031 completion evidence. Model evaluations measure probabilistic conformance and cannot replace byte, permission, or projection assertions.

## Task 9: Reconcile Lifecycle and Deliver

- [ ] Keep RH-W-0031 `active` during implementation.
- [ ] After canonical, package, consumer projection, checker, literal-scan, and byte-preservation verification pass, reconcile roadmap, current state, localized peers, changelog, and Engram topic `work/RH-W-0031` in one work unit.
- [ ] Run `git diff --check` and scan changed files for U+2013 and U+2014. Expected: no output and zero forbidden characters.
- [ ] Inspect exact status and diff. Confirm no dependency, secret, provider, unrelated lifecycle, or hand-edited generated file changed.
- [ ] Do not stage, commit, push, publish, or mutate an external system without separate approval.

## Risks and Rollback

- Flattened hosts cannot reliably distinguish genuine human switches from injected or forwarded text. The safe behavior is to preserve explicit preference and disclose the capability limit.
- Typed provenance is only as trustworthy as the integrating host boundary. Content labels embedded inside arbitrary text are not provenance.
- Prompt and projection tests prove contract distribution, not model obedience.
- Roll back only RH-W-0031 files through the bounded executor or the implementer's patch. Never use destructive Git commands or manual generated-file repair.

## Plan Acceptance Checklist

- [ ] Precedence exactly matches the authoritative decision and contains no prose-based override of persisted preference.
- [ ] Latest verified human instruction is distinct from latest text or bytes.
- [ ] Manifest language is validated fallback configuration, never current conversational evidence.
- [ ] Provenance-aware and flattened-host modes have explicit capability limits.
- [ ] Host claims are cited and do not equate `role: "user"` with human authorship.
- [ ] Machine values are typed, opaque, non-translatable, code-formatted, and byte-asserted.
- [ ] Deterministic completion tests are separate from optional future model evaluations.
- [ ] Runtime prose is concise; exhaustive taxonomy belongs to validation and tests.
- [ ] Injection, context-loss, translation, learning, mixed-language, and artifact-language cases are covered.
- [ ] Language resolution cannot alter security, permissions, commands, tokens, or executable values.
- [ ] Every task has exact files or bounded symbols, commands, expected outcomes, and no implementation placeholder.
- [ ] Scope remains one bounded product unit.
