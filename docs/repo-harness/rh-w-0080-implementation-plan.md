# RH-W-0080 Ephemeral Batch Interaction Implementation Plan

> **Status: proposed.** The owner approved this bilingual plan on 2026-10-06 ("si") for documentation recording only. Implementation authorization remains separate, and the lifecycle stays `proposed`.

Derived from the owner-approved [design](rh-w-0080-design.md). Spanish peer: [rh-w-0080-implementation-plan.md](../es/repo-harness/rh-w-0080-implementation-plan.md).

## Proven Surface And Authority Boundaries

Ownership is resolved, not optional. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` owns activation, inspect-first intake, grouped non-authorizing clarification, and continuation for one objective. `::session_stop_skill()` owns expiry on pause, close, completion, abandonment, or replacement of that objective. `::rule_outputs()` owns shared authority exclusions in working agreements. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` owns generated-contract assertions. `render_with_skill_mapping()` renders all three owners. RH-W-0067 owns response syntax, RH-W-0075 external necessity, RH-W-0074 post-answer compliance, RH-W-0069 continuity, RH-W-0068 changelog disposition, RH-W-0081 chronology, and RH-W-0053 executable checkpoints. RH-W-0074 and RH-W-0075 are references to completed behavior, not unfinished dependencies. Builder preview and apply retain exact digest and token authority without modification.

Group only non-authorizing requirement questions; exclude human/material decisions and every approval. Preserve evidence, safety, and authority pauses, asking one blocking decision at a time. Activation and answers are session-only, expire on pause or objective replacement (as well as the other stop conditions above), confer no authority, and are never reused automatically. Generated-contract tests verify structure, not live-agent compliance.

## Task 1: RED For Activation, Intake, And Expiry

- [ ] Add `GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation` and `test_batch_intake_groups_only_non_authorizing_clarifications`.
- [ ] Assert explicit activation names one bounded selected-repository objective, inspect-first analysis precedes intake, and only newly discoverable non-authorizing requirement clarifications may be grouped.
- [ ] Assert ordinary repository-resolvable evidence gaps are resolved without asking and authorized in-scope work proceeds without file-by-file confirmation.
- [ ] Assert activation and grouped answers are session-only; expire on pause or objective replacement and all other objective stop conditions; are never reused automatically or written to manifest, local state, backups, Engram, checkpoints, receipts, or another store.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation GeneratedCoreTests.test_batch_intake_groups_only_non_authorizing_clarifications -v`. Expected RED: missing contract clauses only.

## Task 2: RED For Separate Resumable Authority Gates

- [ ] Add `GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable` and `test_batch_answers_never_satisfy_builder_digest_bound_authority`.
- [ ] Assert separate one-question gates for external access, Git delivery, publication, destructive action, secrets, builder preview, builder apply, exact digest or token approval, and newly discovered material product decisions.
- [ ] Assert grouped answers never persist, reuse, broaden, normalize, supply, infer, or substitute `APPROVE`, `DENY`, `APROBAR`, `DENEGAR`, `planSha256`, generation digest, refreshed evidence, previewed-byte approval, apply authority, or active approval domain.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable GeneratedCoreTests.test_batch_answers_never_satisfy_builder_digest_bound_authority -v`. Expected RED: authority separation and exact builder exclusions are absent; existing `ApprovalUxTests` remain passing controls.

## Task 3: Minimal GREEN And Owner Collision Control

- [ ] Add activation and intake clauses only to `session_start_skill()`, expiry clauses only to `session_stop_skill()`, and shared authority exclusions only to the working-agreements output in `rule_outputs()`. Add no parser, mode object, flag, manifest field, callback, state machine, service, or persistence.
- [ ] Reference the existing response contract for one-question presentation without reproducing RH-W-0067 syntax.
- [ ] Keep RH-W-0075 necessity, RH-W-0074 compliance, and all other named owners as references, not copied behavior.
- [ ] Repeat the two exact commands above, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py ApprovalUxTests GeneratedCoreTests -v`. Expected GREEN: all pass.
- [ ] Rollback removes only RH-W-0080 clauses and tests. It leaves every authority owner and any concurrently edited working-agreements clause intact.

## Task 4: Projections, Documentation, And Lifecycle

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v` after focused GREEN. Expected: exit 0.
- [ ] Synchronize canonical script and test package projections, then use the normal maintenance renderer for generated session contracts. Review exact preview and digest before separately authorized apply. Expected second preview: no actions.
- [ ] After verified implementation and separate lifecycle authority, reconcile exactly `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `docs/repo-harness/README.md`, `docs/es/repo-harness/README.md`, `docs/repo-harness/rh-w-0080-design.md`, `docs/es/repo-harness/rh-w-0080-design.md`, `docs/repo-harness/rh-w-0080-implementation-plan.md`, `docs/es/repo-harness/rh-w-0080-implementation-plan.md`, `CHANGELOG.md`, `CHANGELOG.es.md`, and Engram `work/RH-W-0080`. Do not store batch authority in Engram.
- [ ] Keep independent issue, commit, review, rollback, and delivery from every coordinated owner. No Git action is authorized now.

## Verification

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_batch_interaction_requires_explicit_one_objective_activation GeneratedCoreTests.test_batch_intake_groups_only_non_authorizing_clarifications GeneratedCoreTests.test_batch_interaction_keeps_authority_gates_separate_and_resumable GeneratedCoreTests.test_batch_answers_never_satisfy_builder_digest_bound_authority ApprovalUxTests -v`; repeat against `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` and `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`. Then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v` against all three paths. Expected: equal pass results.
- [ ] Run `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Expected: valid digest and only intended rule or session replacements.
- [ ] After separate apply authorization and without changing inputs, run `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Expected: `VALIDATED` and empty second mutating preview.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, zero writes.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four pass.
- [ ] Run `python -c "from pathlib import Path; roots=[Path('.repo-harness/manifest.json'),Path('.repo-harness/rules'),Path('.repo-harness/skills')]; terms=('batchMode','batch_mode','groupedAnswers','grouped_answers','batchApproval','batch_authority'); files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*')) if p.is_file()]; bad=[f'{p}:{t}' for p in files for t in terms if t in p.read_text(encoding='utf-8',errors='ignore')]; assert not bad,bad"`. Expected: exit 0 and no persisted batch state or authority.
- [ ] Run `git diff --check` and scan changed Markdown for U+2013 and U+2014. Expected: no findings.

## Review Workload Forecast

Authored change: 150 to 230 lines. Mechanical projection change: 90 to 180 lines. Risk: high for authority semantics, low for runtime complexity. Recommended work units: activation/intake, authority exclusions, then projections/lifecycle. The 400-line authored review budget is not at risk.

## Acceptance And Decisions

- [ ] Grouped intake is clarification only, after inspection, for one objective.
- [ ] Every authority gate stays separate, resumable, exact, and one-question.
- [ ] No batch authority persists or affects builder digest-bound approval.
- [ ] No owner is merged and no new state, dependency, daemon, store, service, or framework appears.

Unresolved decisions: none for planning. Plan approval is recorded; separate implementation authorization remains the next gate.
