# RH-W-0091 Startup Priority and Resume-First Implementation Plan

> **Status: completed historical execution record.** RH-W-0091 is `completed`. Plan approval, implementation authorization, and lifecycle closure were separate owner decisions; no commit, push, PR, merge, release, or publication is claimed.

## Completed Execution Evidence

- Strict TDD RED/GREEN covered 2 + 3 + 2 focused tests for backup shape/stop order, startup priority/triggers, and owner/capability preservation.
- Maintenance digest `5077cc89efdd1032a7dcd4e8374eb84bcd8925cee7110e4f8276a536aa5ca59e` applied exactly eight generated session contracts. The correction RED measured 879; the intermediate 697 lost two exact phrases; final 697 preserves `repository evidence remains authoritative` and `never claim successful continuity`.
- Final full suite: 554 tests, 10 skipped, exit 0. The checker was `current` with zero writes; four lifecycle/link tests pass; `git diff --check` passed.
- Exact package parity: script SHA `b08fa14b2e11e7b050371279c6837165112c07ce40374da38e35d7777dab0ac0`; tests SHA `f1119d226386d5ac3144e34f515fd2038697b0d6fcd19691f56f0de982b94f1b`. Eight generated session contracts match current render bytes.
- Installed human-owned `.repo-harness/rules/memory.md` remained HEAD-identical and non-replaceable; preview has no mutations. Issue #190 is approved/open; the local branch is `feat/rh-w-0091-resume-first`.

Derived strictly from the owner-approved [scope](rh-w-0091-scope.md) and [design](rh-w-0091-design.md). Spanish peer: [rh-w-0091-implementation-plan.md](../es/repo-harness/rh-w-0091-implementation-plan.md).

## Goal And Authority Boundary

Implement only the approved instruction-level resume-first contract: newly created native continuity backups place the exact `Where We Left Off` block before existing detailed continuity; session-start consumes charter, valid language/preference evidence, newest valid resume content, and then deeper owners only for the four approved triggers. Existing backups remain readable without synthesized fields.

This plan does not authorize source, test, generated output, runtime projection, backup, preference, Git, network, review, or delivery changes. The owner's approval of this corrected plan authorizes only the plan. A later explicit implementation authorization is required before Task 1. Maintenance apply, lifecycle closure, Git delivery, and network operations retain their separate current gates.

## Proven Canonical Surfaces And Projection Families

| Surface | Planned responsibility |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill()` | Require the new resume block in newly created backups and preserve refresh, filtering, exclusive write, validation, local-before-Engram order, and independent outcomes. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` | Require the exact startup priority, newest-valid resume read, old-backup fallback, and exactly four deeper-read triggers. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs()` / local `native_memory_contract` | Own the shared exact backup shape, compatibility, authority, privacy, and exclusion clauses without a new schema owner. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_with_skill_mapping()` | Existing path that renders `session_start_skill()`, `session_stop_skill()`, and `rule_outputs()`; no signature, input, callback, or return change. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_repository()` | Existing path that projects canonical product package files to selected runtimes; no projection mechanism change. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` | Own focused integrated and standalone generated-contract assertions. |

Canonical product files are exactly:

- `skill/repo-harness-builder/scripts/harness_tool.py`
- `skill/repo-harness-builder/tests/test_harness_tool.py`

Expected mechanical product-package projection families are:

- `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`
- `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`
- `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`

Expected generated contract families are:

- self-hosted `.repo-harness/skills/repo-harness-session-{start,stop}/SKILL.md`;
- runtime `.claude/skills/repo-harness-session-{start,stop}/SKILL.md`;
- runtime `.opencode/skills/repo-harness-session-{start,stop}/SKILL.md`;
- runtime `.pi/skills/repo-harness-session-{start,stop}/SKILL.md`; and
- the shared self-hosted `.repo-harness/rules/memory.md` output associated with `rule_outputs()`.

These are expected families, not a preapproved replacement list. The normal maintenance preview must determine the final exact replacements from current ownership and bytes. In particular, the plan does not assume that human-owned or human-evolved `.repo-harness/rules/memory.md` is replaceable. If preview omits, blocks, or classifies that path as non-replaceable, stop without forcing, copying over, normalizing, or bypassing ownership and obtain a separate owner decision.

## Completed Strict TDD Execution

The completed implementation used the canonical unittest runner. Each task recorded observed RED before changing canonical implementation text, then the smallest GREEN. A passing existing control was not RED evidence.

### Task 1: Backup Shape And Session-Stop Order

- [x] Add `GeneratedCoreTests.test_resume_first_backup_shape_and_stop_order_are_exact` and `test_resume_first_old_backups_remain_readable_without_synthesized_fields` in the canonical test file.
- [x] Assert that a newly created backup preserves the existing required RH-W-0069 frontmatter as the file beginning, unchanged; its body then has exactly `# Session Continuity`, `## Where We Left Off`, `### Next Action`, `### Pending Work`, and `### Current Blocker` in that order, followed by existing detailed continuity sections. Empty values are `None.` and existing inline evidence labels remain unchanged.
- [x] Assert `session_stop_skill()` refreshes applicable evidence, determines those three values in order, writes and validates that exact body order after the unchanged required RH-W-0069 frontmatter and before existing detailed continuity, and then preserves RH-W-0069 filtering, exclusive creation, local-before-Engram order, validation, and independent outcomes.
- [x] Assert existing valid backups without `Where We Left Off` remain readable through existing detailed continuity and no missing resume field is synthesized, migrated, rewritten, or backfilled.
- [x] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_resume_first_backup_shape_and_stop_order_are_exact GeneratedCoreTests.test_resume_first_old_backups_remain_readable_without_synthesized_fields -v`. Expected RED: the frontmatter-preserving exact body order, resume block, and compatibility clauses are absent while imports and existing controls still work; the expectation does not remove or alter RH-W-0069 frontmatter.
- [x] Minimally update only `session_stop_skill()` and the `native_memory_contract` rendered by `rule_outputs()`. Add instruction text only, with no writer API, parser, migration, or second store.
- [x] Repeat the exact command. Expected GREEN: both tests pass.

### Task 2: Startup Priority, Four Triggers, And Negative Acceptance

- [x] Add `GeneratedCoreTests.test_session_start_uses_exact_resume_first_priority`, `test_session_start_deep_reads_for_exactly_four_triggers`, and `test_session_start_does_not_deep_read_without_a_named_trigger`.
- [x] Assert the exact priority: the bounded charter pointer in `decisions.md`; verified conversation language plus active stable preferences or continuing constraints from existing owners; `Where We Left Off` from the newest valid RH-W-0069 backup; then deeper owners only when triggered.
- [x] Assert exactly these four triggers, without aliases or an open-ended catch-all: explicit objective or `RH-W-####`; contradiction; invalid or missing priority evidence; mandatory reconciliation.
- [x] Assert each trigger selects only its required current owner evidence and never authorizes exhaustive reading, automatic continuation, preference inference, or a new store.
- [x] Require negative acceptance: with valid priority evidence and none of the four triggers, rendered session-start instructions stop after the resume-first read and perform no deeper owner read.
- [x] Assert an old valid backup without the new block falls back to RH-W-0069 detailed continuity without synthesizing fields.
- [x] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_uses_exact_resume_first_priority GeneratedCoreTests.test_session_start_deep_reads_for_exactly_four_triggers GeneratedCoreTests.test_session_start_does_not_deep_read_without_a_named_trigger -v`. Expected RED: exact priority, bounded trigger matrix, or negative no-deep-read contract is absent.
- [x] Minimally update only `session_start_skill()` and concise references to the shared memory contract. Add no reader callback, parser, cache, or orchestration subsystem.
- [x] Repeat the exact command. Expected GREEN: all three tests pass.

### Task 3: Capability And Owner Preservation

- [x] Add `GeneratedCoreTests.test_resume_first_preserves_capability_and_owner_boundaries` and `test_resume_first_excludes_new_state_services_and_preflights`.
- [x] Assert scoped Engram recovery remains availability evidence itself, with no separate Engram probe or preflight and no repeated unchanged-success report; failures and task-relevant discrepancies remain visible.
- [x] Assert Gentle AI is reached only at an applicable enabled routing, review, receipt, or correction boundary and receives no startup probe.
- [x] Assert RH-W-0069 retains schema, privacy, exclusive-write, local-before-Engram, and independent-outcome ownership; RH-W-0078 retains physical generated-skill splitting and progressive disclosure; RH-W-0031, RH-W-0071, RH-W-0082, and RH-W-0018 retain language and preference semantics; roadmap, current state, and changelog retain their current roles.
- [x] Assert no new infrastructure, parser/API, schema owner, cache, daemon, service, scheduler, migration, cleanup, retention behavior, checkpoint, preference framework, Gentle probe, Engram preflight, or second continuity store appears.
- [x] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_resume_first_preserves_capability_and_owner_boundaries GeneratedCoreTests.test_resume_first_excludes_new_state_services_and_preflights -v`. Expected RED: preservation or exclusion clauses are incomplete.
- [x] Add only the minimum shared/start/stop wording needed by the assertions, then run the same command. Expected GREEN: both tests pass.
- [x] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Expected: the complete canonical class passes before projection synchronization.

### Task 4: Mechanical Projection Synchronization

- [x] After canonical `GeneratedCoreTests` is GREEN, synchronize only the canonical script and test bytes through `render_repository()` to the three product-package projection families above. Verify exact SHA-256 parity for `scripts/harness_tool.py` and `tests/test_harness_tool.py` across canonical, Claude, OpenCode, and Pi package roots.
- [x] Run the normal read-only maintenance preview through the canonical `harness_tool.py render` command using current installed manifest values and required verified capability evidence. The preview, not this plan, determines the exact generated replacement set.
- [x] Review every previewed path and classification. Require only candidate paths from the named generated contract families, no backup or preference file, no migration, no deletion, and no manifest/checker schema change. Treat `.repo-harness/rules/memory.md` according to previewed ownership; never assume or force replacement.
- [x] Only after separate implementation and exact maintenance-apply authorization, apply the unchanged reviewed digest. Repeat preview and require no remaining mutating action. A blocked, changed, or broadened preview invalidates apply authority and stops the task.
- [x] Compare every preview-authorized generated session contract against current `render_with_skill_mapping()` output and run `python .repo-harness/check.py`. Expected: checker `current`, exit 0, and zero writes.

### Task 5: Final Verification And Documentation Closure

- [x] Run the focused command covering all seven new tests against the canonical test file, then run `GeneratedCoreTests` against canonical, Claude, OpenCode, and Pi package test paths. Expected: equal passing results.
- [x] Run the full canonical suite once: `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0. Do not rerun byte-identical projected suites unless parity or a runtime-specific check fails.
- [x] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, and zero writes.
- [x] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four pass.
- [x] Focused bilingual parity checks cover the scope, design, plan, maps, roadmap, current state, and changelog peers; `git diff --check` passed.
- [x] Verified implementation and separate lifecycle authority reconciled bilingual execution evidence, roadmap, current state, maps, and changelogs. RH-W-0091 is `completed` with remaining work `none`.

## Rollback

Rollback removes only RH-W-0091 canonical clauses and focused tests, restores canonical source and test bytes first, then uses the normal preview and separately authorized apply to synchronize only the exact eligible prior projections. It preserves every existing backup and Engram observation, never synthesizes fields in old backups, never deletes continuity, and never overwrites human-owned `.repo-harness/rules/memory.md` without a preview-supported and separately authorized path.

## Review Workload Forecast

Expected authored change: 140 to 220 source and test lines. Expected mechanical package and generated projection change: 180 to 320 lines, with the exact set determined by maintenance preview. Risk is medium for ordering, compatibility, and owner preservation, and low for runtime complexity because no executable continuity subsystem is added. Review in four bounded units: backup shape/stop order; startup priority/triggers including the negative case; capability/owner preservation; then mechanical projections and lifecycle. Keep each RED beside its GREEN. The approximately 400-line authored review target is not expected to be exceeded; if preview broadens the authored surface or replacement families, stop and rescope before apply.

## Acceptance, Exclusions, And Decisions

- [x] New backups use the exact resume-first shape and stop order; old backups remain readable without synthesized fields.
- [x] Startup uses the exact approved priority and only the four named deeper-read triggers.
- [x] Negative acceptance proves no deep read occurs without one of those triggers.
- [x] Existing capability, backup, language, preference, progressive-disclosure, lifecycle, privacy, and authority owners remain intact.
- [x] Maintenance preview determines the exact replacement set and human-owned content is preserved.
- [x] No new infrastructure, parser/API, schema owner, cache, daemon, service, scheduler, migration, cleanup, retention behavior, preference framework, Gentle probe, Engram preflight, or second store is introduced.

Unresolved technical or product decisions: none. The completed execution preserved the stated boundaries and the historical failed verification as correction evidence. Lifecycle closure is complete with remaining work `none`; Git delivery, commit, push, PR, merge, release, and publication remain unclaimed.
