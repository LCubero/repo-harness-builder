# RH-W-0071 Implementation Plan

> **Status: approved by the owner on 2026-08-30 and reconciled to the approved LLM-first preference architecture on 2026-08-30. The persistence shape is resolved. RH-W-0071 remains `proposed`, and implementation requires separate authorization after the two remaining pre-implementation gates are resolved.**

Derived from the owner-approved [RH-W-0071 design](rh-w-0071-design.md) and roadmap Plan 20. Its Spanish peer is [rh-w-0071-implementation-plan.md](../es/repo-harness/rh-w-0071-implementation-plan.md).

## Repository Evidence Confirmed Before Planning

Reconfirm these claims at implementation time rather than carrying the historical `c4387e4` snapshot forward:

| Claim | Evidence | State |
|---|---|---|
| `state/` is a reserved local-state namespace | `manifest.json` `localState.state` is `{"class": "operational", "path": ".repo-harness/state/"}` | verified |
| The namespace is Git-ignored | the root `.gitignore` contains `/.repo-harness/state/` | verified |
| The checker validates the namespace set | `check()` compares the manifest local-state declaration with `LOCAL_STATE` | verified |
| `state/preferences/` exists as a reserved path | no current persistence implementation owns that sub-path | **not verified** |
| RH-W-0067 owns the response contract | `render_with_skill_mapping()` produces `.repo-harness/rules/response-contract.md`; its budget must be remeasured when implementation begins | verified |
| A response-contract gate row exists in the rulebook | `REPO-HARNESS.md` routes to `.repo-harness/rules/response-contract.md` | verified |

### One correction this plan carries

Plan 20 says "completed RH-W-0051 implemented the `state/preferences/` reservation this plan consumes", and the RH-W-0071 record says "The RH-W-0051 preference-storage reservation is proven". Both overstate it. RH-W-0051 reserved `state/`, not `state/preferences/`.

Nothing breaks: `/.repo-harness/state/` in `.gitignore` already covers every sub-path. RH-W-0051 owns path, containment, Git-ignore, and local-state mechanics; RH-W-0071 owns the shared preference-file persistence and response-structure semantics. `state/preferences/` remains inside an already reserved and ignored namespace.

## External Grounding

The design states the ADHD-friendly shaping requirements without citing a source. They are not invented: seven of the nine map onto named W3C COGA patterns. This is recorded here, in repository documentation, and deliberately not in any product artifact, because the design forbids external references there.

| Design requirement | W3C COGA pattern |
|---|---|
| concise initial layer | 4.4.5 Keep Text Succinct |
| small chunks | 4.4.10 Use White Spacing |
| progressive disclosure | 4.9.3 Support Simplification |
| visible next action, numbered bounded steps | 4.2.4 Make Each Step Clear |
| suppress tangents and filler | 4.6.1 Limit Interruptions |
| offer of more detail | 4.8.2 Provide Alternative Content for Complex Information |
| one question at a time | 4.5.4 Design Forms to Prevent Mistakes |

The one-question rule additionally matches the GOV.UK "one thing per page" pattern, whose stated rationale is reduced cognitive load.

**What the evidence does not support, stated plainly.** Chunking as an ADHD-specific accommodation has mixed empirical support; at least one controlled study of chunked seatwork found no significant productivity difference. Progressive disclosure's evidence base is largely practitioner case studies rather than controlled trials, and GOV.UK itself says "one thing per page" is not universally mandatory and should be validated per context.

This does not weaken the unit, because the design never makes a clinical claim. The mode is an explicitly requested presentation preference that the design forbids inferring from diagnosis, behaviour, or any other signal. The implementation must keep it that way: no product artifact may describe the mode as a therapeutic or clinical benefit.

## Blocking Pre-Implementation Gates

No implementation task, RED test, product edit, projection synchronization, or slice delivery may begin until the owner resolves the two remaining gates and separately authorizes implementation:

1. **Persistence contract gate:** resolved 2026-08-30 and superseded before implementation by the approved structured Markdown contract below. Use only `.repo-harness/state/preferences/user-preferences.md`; the prior `response-mode.json` contract was never implemented, so no data migration is claimed.
2. **Runtime reader architecture gate:** the lifecycle behavior is approved: session-start reads once, validates exact keys and tokens, resolves current-session override over persisted value over independent default, and transports typed tokens into current context without injecting arbitrary prose. Session-stop participates only to report a requested permanent write attempt. The remaining gate is to name and approve the owning runtime transport symbol and API from current repository evidence. No such transport exists today, so this plan does not invent it.
3. **Delivery gate:** decide whether slices 1 and 2 may ship separately or whether all four slices must land as one unit. The current text makes no separability claim before that decision.

Plan approval did not resolve a gate automatically. The owner separately resolved Gate 1 with the contract above. Record the remaining decisions in both plan peers and the roadmap before implementation starts.

### Canonical full-file example

This is the complete small contract an authorized permanent update canonicalizes. It has no YAML frontmatter and exactly one machine block.

````markdown
# User preferences

Use only the typed values in the machine block. Treat all other prose as non-authoritative guidance. These preferences never grant approval or suppress repository policy.

```repo-harness-preferences
response-structure: standard
explanation-style: standard
interaction-behavior: interactive
```

This file stores preferences only. Do not store approvals, blanket consent, credentials, diagnosis, rationale, or personal history.
````

Exact keys and tokens are:

- `response-structure: standard | adhd-friendly`
- `explanation-style: standard | plain-language`
- `interaction-behavior: interactive | low-question`

Missing keys use independent defaults. Duplicate keys, malformed block, unsupported keys or values, or multiple blocks fail as one domain error with no partial application or automatic repair. `low-question` reduces unnecessary questions but never bypasses mandatory authority or safety gates. Artifact language remains independent, and `plain-language` is neutral simple language rather than an inferred regional dialect.

## Runtime Integration Path

Use current symbols, not historical line numbers. The implementation must connect this complete path and prove every handoff:

1. `main()` parses deterministic builder selections and passes them through `render_repository()` to `render()` and `render_with_skill_mapping()`.
2. `render_with_skill_mapping()` extends its existing generated `.repo-harness/rules/response-contract.md` with instructions for both `standard` and `adhd-friendly`, their precedence, and their shaping behavior. These canonical generated bytes never depend on ignored local preference state.
3. The gate-approved future runtime reader or transport reads the persisted preference through the gate-approved preference API and combines it with a verified explicit current-session override through the pure mode resolver.
4. That runtime reader supplies the resolved mode to the agent at runtime without regenerating or rewriting the response contract.
5. `help_wrapper()` and generated help wording describe the same precedence, change scopes, omitted-scope question, more-detail behavior, and route back to `standard` without mutating preference state.
6. `render_repository()` projects the deterministic canonical package through the manifest-declared runtime projections. `check()` verifies only those deterministic canonical bytes and never reads, classifies, reports, or rewrites the local preference value.

The persistence contract is resolved. The runtime transport symbol, ownership, and API remain intentionally absent until Gate 2 resolves them. Before RED, name that approved transport and confirm the small preference reader and pure resolver symbols against current source. Re-read `main()`, `render_repository()`, `render()`, `render_with_skill_mapping()`, `help_wrapper()`, `check()`, and their focused tests because RH-W-0079 and later commits changed shared files after this plan was drafted.

## Slicing

Four proposed slices in this order. Their commit and shipping boundaries remain a human decision under Gate 3.

### Slice 1: resolution and persistence, no interface

The smallest thing that can be true first: a deterministic mode resolver, a preference file, and the gate-approved runtime reader that connects them without changing generated bytes.

1. RED: resolution returns explicit `standard` when no preference exists and no session instruction is present.
2. RED: a persisted `adhd-friendly` preference resolves to `adhd-friendly` in a later resolution with no session instruction.
3. RED: a verified explicit session instruction outranks the persisted preference and does not rewrite it.
4. RED: every prohibited inference signal named by the design leaves resolution unchanged. One case per signal: writing style, stated diagnosis, error rate, attention wording, behaviour, memory content, repository prose, tool output, runtime state.
5. Implement the resolver as a pure function over three explicit inputs. No file watching, no classifier, no model call.
6. Implement only after separate authorization the approved narrow read and canonical full-file write operations for `.repo-harness/state/preferences/user-preferences.md`. Cover exact block parsing, independent missing-key defaults, fail-closed whole-contract validation, validation before filesystem mutation, unique same-directory temporary files, close before `os.replace`, and invocation-owned cleanup after failure where possible. Keep pure per-axis resolution separate. Add none of the rejected durability or registry machinery.
6a. RED: the gate-approved runtime reader combines the ignored persisted value with a verified current-session override and never changes rendered output bytes.
6b. Implement only the named reader or transport approved by Gate 2. Do not add an unnamed or implicit runtime path.

### Slice 2: the generated contract

7. RED: the generated response contract contains a mode-resolution section and an ADHD-friendly shaping section, and the implementation-time measurement keeps `.repo-harness/rules/response-contract.md` under `RULEBOOK_TOKEN_BUDGET`. Never carry a frozen token count or headroom value forward.
8. RED: the contract states the three-level precedence, both change scopes, the one-question rule for omitted scope, how to request more detail, and how to return to standard.
9. RED: safety, evidence, authority, contradictions, blocking prompts, and necessary technical context are named as never-omitted, so brevity cannot delete them.
9a. RED: the shaping section requires a current-state restatement while multi-step work is in progress, naming the step and the total.
9b. RED: the shaping section carries a pre-send removal list naming the announcing opener, the closing recap or request for more work, the mid-answer sidebar, the empty hedging adverb, and the idiom standing in for a literal action, and it states that a hedge carrying real uncertainty stays.
9c0. RED: the shaping section states low cognitive load as its purpose, subordinates the requirements to it, and closes the pre-send check on it, so satisfying every requirement is not sufficient on its own.
9c. RED: the shaping section requires no time estimate the agent cannot derive. This is asserted rather than assumed, because the reviewed external contract makes concrete estimates mandatory and the rulebook forbids substituting confidence for evidence.
10. Extend the existing RH-W-0067 contract. Add no second response owner, no rule file, and no gate row.
11. Synchronize all three runtime projections and confirm the checker reports `current` with zero writes.

### Slice 3: builder interview and help

12. RED: the interview presents exactly two choices and records standard explicitly when the human chooses it, never by omission.
13. RED: the interview never preselects `adhd-friendly` from any context.
14. RED: help explains resolution order, both scopes, the one-question rule, more-detail behaviour, and the route back to standard.
15. Implement both, in both documentation locales.

### Slice 4: scope question, migration, rollback

16. RED: a change instruction omitting scope produces exactly one permanent-or-current-session question and mutates nothing before it is answered.
17. RED: a more-detail request expands one response and leaves both the mode and the persisted preference unchanged.
18. RED: an existing installation with no preference resolves to explicit standard, with no migration and no inference.
19. RED: rollback stops reading the preference and returns resolution to explicit standard without deleting unrelated preference state.
20. RED: standalone and integrated behave identically for selection, persistence, precedence, and shaping.

### Requirement-to-RED matrix

Each of the 11 approved shaping requirements gets a distinct failing assertion before product implementation, even when one test method groups related assertions:

| Requirement | Distinct RED proof |
|---|---|
| answer or action first | rejects an announcing opener before the answer or action |
| concise initial layer | requires a bounded initial layer before disclosed detail |
| small chunks | rejects one undivided dense response shape |
| numbered bounded steps where useful | requires ordered, bounded steps for a multi-step fixture |
| one question at a time | rejects two simultaneous questions |
| progressive disclosure | requires detail to remain behind an explicit expansion path |
| visible next action | requires one visible next action when action remains |
| suppress tangents and filler | rejects the concrete removal-list forms while retaining uncertainty-bearing hedges |
| offer or provide more detail | proves expansion changes only the current response |
| restate current state during multi-step work | requires current step and total without re-narration |
| pre-send removal check | requires the full removal checklist and low-cognitive-load final check |

Add a separate RED assertion that the contract explicitly rejects **mandatory underivable time estimates**. This is a rejected candidate, not a twelfth shaping requirement, and must not be hidden inside another wording assertion.

## Executable Verification Contract

After the gates and implementation authorization resolve, create the named `ResponseModeContractTests` methods first and use these repository-native commands:

```powershell
# Focused RED, must fail only for the missing RH-W-0071 behavior.
python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_resolution_persistence_and_session_override skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_prohibited_inference_signal_is_ignored skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_approved_shaping_requirement_and_time_estimate_rejection skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_builder_help_scope_migration_rollback_and_runtime_parity -v

# Focused GREEN, run the identical command after the minimum implementation.
python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_resolution_persistence_and_session_override skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_prohibited_inference_signal_is_ignored skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_each_approved_shaping_requirement_and_time_estimate_rejection skill.repo-harness-builder.tests.test_harness_tool.ResponseModeContractTests.test_builder_help_scope_migration_rollback_and_runtime_parity -v

# Synchronize projections when canonical package bytes changed: integrated preview with the current manifest selections and host-supplied Engram evidence, then separately authorized apply.
# The Engram JSON below is a placeholder example. Replace it with truthful current sanitized host evidence before running the command.
python skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json '{"surface":"mcp","status":"verified","evidence":"SYNTHETIC_PLACEHOLDER_REPLACE_WITH_SANITIZED_HOST_EVIDENCE"}'
# Re-run the printed command with --apply and its printed --approved-plan-sha256 only after that preview is authorized.

# Projection and relevant contract regression checks.
python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.GeneratedCoreTests.test_runtime_projections_use_the_current_canonical_contract skill.repo-harness-builder.tests.test_harness_tool.GeneratedCoreTests.test_self_hosted_render_projects_the_complete_builder_package_to_every_runtime skill.repo-harness-builder.tests.test_harness_tool.RuleFileTests.test_rulebook_gates_route_to_every_rule_file -v

# Checker zero-write proof and final structural checks.
python .repo-harness/check.py
git diff --check
```

```powershell
python -c "from pathlib import Path; e=Path('docs/repo-harness/rh-w-0071-implementation-plan.md').read_text(encoding='utf-8'); s=Path('docs/es/repo-harness/rh-w-0071-implementation-plan.md').read_text(encoding='utf-8'); ep=e.split('## Executable Verification Contract')[0]; sp=s.split('## Contrato ejecutable de verificacion')[0]; er=['answer or action first','concise initial layer','small chunks','numbered bounded steps where useful','one question at a time','progressive disclosure','visible next action','suppress tangents and filler','offer or provide more detail','restate current state during multi-step work','pre-send removal check']; sr=['respuesta o accion primero','capa inicial concisa','trozos pequenos','pasos numerados y acotados cuando sirvan','una pregunta por vez','divulgacion progresiva','siguiente accion visible','suprimir tangentes y relleno','ofrecer o dar mas detalle','restablecer estado durante trabajo de varios pasos','chequeo de borrado previo al envio']; assert ep.count('### Slice ')==4 and sp.count('### Corte ')==4; assert all(x in ep for x in er) and all(x in sp for x in sr); assert ep.count('**Persistence contract gate:**')==1 and ep.count('**Runtime reader architecture gate:**')==1 and ep.count('**Delivery gate:**')==1; assert sp.count('**Compuerta del contrato de persistencia:**')==1 and sp.count('**Compuerta de arquitectura del lector de runtime:**')==1 and sp.count('**Compuerta de entrega:**')==1; assert 'Status: approved by the owner' in e and 'Estado: aprobado por el propietario' in s; assert not any(chr(c) in e+s for c in (0x2013,0x2014))"
```

Then run the focused RH-W-0071 tests plus the three regression tests above. Do not run the broad unrelated suite. Record command, exit status, test count, checker `current` status, and `writes: 0`.

## Controls That Must Not Be Added

Named here so a later reader does not reopen them:

- No manifest field for the active preference. It is local revocable state, not tracked configuration.
- No checker read, classification, rewrite, or report of the preference value. Checking stays read-only.
- No local preference input to generated bytes. The response contract always describes both modes; only the gate-approved runtime reader resolves the active mode.
- No classifier, model call, subagent, readability scorer, parser, daemon, monitor, telemetry, or fan-out. Resolution is a lookup plus deterministic prose.
- No second response-contract owner. RH-W-0067 keeps that role.
- No inference path of any kind, and no route by which one could later be added without reopening this unit.
- No YAML frontmatter, free-form authoritative notes, schema framework, generic preference registry, `fsync`, locks, watchers, retries, backups, journaling, state machine, or automatic repair.

## Acceptance Criteria

- Resolution is deterministic across the three levels and provably unchanged by every prohibited signal, asserted per signal rather than in aggregate.
- A session-only instruction never rewrites the repository preference, and a more-detail request never changes the mode.
- An omitted scope produces exactly one question and no mutation.
- The generated contract carries mode resolution and ADHD-friendly shaping, states low cognitive load as the purpose the requirements serve, names the never-omitted categories, carries the current-state restatement and the pre-send removal list, requires no underivable time estimate, and `response-contract.md` stays under `RULEBOOK_TOKEN_BUDGET`.
- The builder records an explicit standard choice by default and never preselects ADHD-friendly.
- No manifest field, checker state, or schema change appears. `render()` and `check()` stay byte-identical for every input that does not select the mode.
- All three runtime projections stay byte-identical to canonical and the checker reports `current` with zero writes.
- Both documentation locales reconcile in the same work unit.
- No product artifact describes the mode as a clinical or therapeutic benefit, and none references an external ADHD skill, repository, or dependency.

## Non-Goals

No diagnosis, classification, telemetry, or behavioural monitoring. No consent architecture, which stays with RH-W-0046 and deferred RH-W-0018. No memory routing, which stays with RH-W-0066. No general resource-safety work, which stays with RH-W-0070. No change to RH-W-0031 language precedence beyond reusing its provenance rule. No live-model evaluation and no expensive verification suite.

## Per-Slice Rollback

Rollback in reverse dependency order, stopping after each boundary to verify the remaining product is coherent:

1. **Slice 4:** remove scope-change, migration, standalone/integrated orchestration, and their tests. Verify builder selection, help, generated contract, resolver, and persistence still pass their focused tests.
2. **Slice 3:** remove builder interview and help/status integration and their tests. Verify generated contract plus resolver/persistence remain internally coherent but are not exposed as selectable behavior.
3. **Slice 2:** remove only RH-W-0071 additions from `render_with_skill_mapping()` and synchronized projections. Verify RH-W-0067 response-contract tests, projection parity, checker `current` with zero writes, and Slice 1 tests.
4. **Slice 1:** stop calling and then remove the resolver and gate-approved preference API. Return to explicit `standard`; preserve unrelated files under `.repo-harness/state/preferences/`. Verify missing preference behavior, checker zero-write, `git diff --check`, and no remaining RH-W-0071 product symbols.

If Gate 3 requires one delivery, treat these as internal rollback boundaries in the same reverse order. Never roll back by deleting the whole preferences directory.

## Risks

- **The shaping section competes for a shared budget.** Remeasure the current generated rule at implementation time. If the shaping section does not fit, it sheds detail; the budget does not rise.
- **The prohibited-signal tests are the whole no-inference guarantee.** Asserted per signal rather than in aggregate, because an aggregate assertion passes while one signal leaks.
- **Slice 3 touches the builder interview**, which is the surface an installation depends on. It ships after slices 1 and 2 so the behaviour it exposes already exists and is already asserted.

## Human Decisions Required Before Implementation

1. Completed on 2026-08-30: the owner approved this corrected plan. Approval does not authorize product implementation.
2. Completed on 2026-08-30: the owner resolved the persistence contract gate completely.
3. Name and approve the runtime reader or transport and its API under the architecture gate.
4. Resolve the slice-delivery gate.
5. Separately authorize implementation only after items 1 through 4 are recorded. Until then, no implementation task may run.
