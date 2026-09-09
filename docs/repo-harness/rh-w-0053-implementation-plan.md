# RH-W-0053 Interrupted-Operation Checkpoint Implementation Plan

> **Status: completed execution record; RH-W-0053 is completed.** The plan was owner-approved on 2026-09-07 and later executed under separate implementation and lifecycle authority. At the implementation-record stage, no Git delivery, staging, commit, push, publication, network access, or runtime registered-name freshness was claimed. The later delivery through issue #170 and PR #171 was squash-merged as `c7290fcf6c703df0839699a9cb8b77686bf18e74`.

**Recorded execution evidence:** control gate 4 passed. RED A recorded 27 failures; RED B recorded 9 failures plus 1 error; RED C recorded 12 failures; each focused GREEN group passed. `GeneratedCoreTests` passed 142 tests with 5 skipped, the final canonical suite passed 523 tests with 9 skipped, and the checker reported `current`, exit 0, and 0 writes. The Pi ownership regression went RED then GREEN: acceptance required eight generated projections but preview offered six until Pi generated-skill ownership was corrected. Digest-bound maintenance apply `15d2f3fcfe3beead92559bf8dd57e55291967d178a8d5a55994fcdd62d91c683` applied all eight projections. The task checklist remains the approved historical instruction record; rollback steps were not executed.

Derived from the owner-approved [design](rh-w-0053-design.md). Spanish peer: [rh-w-0053-implementation-plan.md](../es/repo-harness/rh-w-0053-implementation-plan.md).

## Outcome And Boundaries

Implement one instruction-level checkpoint contract for `.repo-harness/state/checkpoints/active.md`. An executing agent may create or replace that file only after an explicit current human request. Startup may inspect it without mutation, but never continues work automatically. A possible resume requires current repository verification and fresh authority for every still-gated action.

This plan adds no Python checkpoint runtime API. It adds no checkpoint history, parser subsystem, database, daemon, scheduler, service, generic workflow engine, duplicate memory, approval reuse, receipt, delivery authority, manifest field, checker content inspection, automatic deletion, migration, or cleanup.

## Proven Owners And Exact Surfaces

| Surface | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill()` | Own explicit-request-only creation or replacement, exact schema, bounded content, safe replacement, and structural readback instructions. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` | Own exact-path discovery, validation, semantic expiry, repository-first reconciliation, resume proposal, and fresh authority instructions. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render_with_skill_mapping()` | Existing call path that renders both contracts; no new input, callback, state, or output path. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` | Own focused integrated and standalone generated-contract assertions. |
| RH-W-0051 | Retains `.repo-harness/state/`, containment, ignore, manifest taxonomy, and read-only checker ownership. No RH-W-0053 manifest or ignore change. |
| RH-W-0050 | Retains evidence classes, bounded reconciliation, and repository-over-memory authority. |
| RH-W-0069 | Retains `.repo-harness/memory-bk/`, Engram cross-checking, and ordinary pause or close continuity. Checkpoint content is not copied there. |

Canonical implementation files are only the canonical script and test above. Mechanical package projections are `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, and `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`. Generated output synchronization is limited to the canonical and three runtime `repo-harness-session-start` and `repo-harness-session-stop` skill files.

## Task 1: RED/GREEN Group A - Authorized Write And Safe Replacement

- [ ] First run the existing RH-W-0051 controls: `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift -v`. Stop if any control fails.
- [ ] Add `GeneratedCoreTests.test_checkpoint_write_requires_explicit_current_human_request`, `test_checkpoint_schema_is_exact_and_bounded`, and `test_checkpoint_replacement_is_safe_and_read_back`.
- [ ] Assert that interruption, pause, close, continuity authorization, an earlier approval, or implementation authority alone never authorizes checkpoint creation or replacement.
- [ ] Assert the exact design frontmatter, headings, `None.` empty-section rule, repository-relative regular paths, non-sensitive bounded content, `resume_authority: required`, and `step_state` limited to `not_started`, `started_unverified`, or `failed`.
- [ ] Assert lazy creation of only `.repo-harness/state/checkpoints/`, inspection of current regular-file bytes before replacement, a safe same-directory replacement that preserves the old file on failure, structural readback before success, and at most one `active.md`. Assert no history or secondary index.
- [ ] Run the three new tests. Expected RED: only the checkpoint write, schema, and replacement clauses are absent.
- [ ] Minimally extend `session_stop_skill()`. Use imperative Markdown instructions only; add no helper, callback, parser, state machine, or persistence API.
- [ ] Repeat the command. Expected GREEN: three tests pass.

## Task 2: RED/GREEN Group B - Read-Only Discovery And Semantic Expiry

- [ ] Add `GeneratedCoreTests.test_checkpoint_discovery_is_exact_read_only_and_non_continuing`, `test_checkpoint_discovery_classifies_all_outcomes`, and `test_checkpoint_expiry_is_semantic_and_never_deletes`.
- [ ] Assert repository owners, source, tests, applicable Git, checker, lifecycle, and decisions are read before only the exact `active.md` path. Absence is `not_found`; non-regular, indirect, escaping, malformed, and project-mismatched input is rejected without following links.
- [ ] Assert distinct `usable`, `stale`, `contradictory`, `expired`, and `invalid` outcomes with bounded reasons. `usable` may inform only a resume proposal.
- [ ] Assert completion, abandonment, supersession, project mismatch, material contradiction, settled step, lifecycle prohibition, or invalid path or structure causes logical expiry. Age alone is advisory. Discovery never rewrites, deletes, archives, marks consumed, updates timestamps, stages, or continues work.
- [ ] Run the three new tests. Expected RED: exact-path classification and semantic-expiry clauses are absent.
- [ ] Minimally extend `session_start_skill()` without changing RH-W-0069 read order or RH-W-0050 bounds.
- [ ] Repeat the command. Expected GREEN: three tests pass.

## Task 3: RED/GREEN Group C - Fresh Resume Authority And Owner Separation

- [ ] Add `GeneratedCoreTests.test_checkpoint_resume_reverifies_current_evidence_and_authority`, `test_checkpoint_never_reuses_approval_or_grants_delivery`, and `test_checkpoint_contract_stays_separate_from_continuity_and_subsystems`.
- [ ] Assert a resume reconstructs the smallest current objective, re-verifies completed facts and the interrupted step, does not rerun currently verified completion, reclassifies remaining intent against current owners, and excludes stale claims from the proposed route.
- [ ] Assert execution begins only after fresh authority covers the exact current action. Prior approvals, consent, digests, tokens, and session intent are never reused; no checkpoint grants implementation, mutation, Git, publication, receipt, or delivery authority.
- [ ] Assert RH-W-0069 backups and Engram remain separate non-authoritative continuity sources. Assert integrated and standalone renders both work without checkpoint history, generic parsing, database, daemon, scheduler, workflow engine, duplicate memory, conflict store, receipt, or delivery store.
- [ ] Run the three new tests. Expected RED: fresh-authority and separation clauses are absent.
- [ ] Complete only the minimum start/stop wording needed by the assertions, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Expected GREEN: the complete class passes.
- [ ] Rollback for Tasks 1 through 3 removes only the RH-W-0053 clauses and their nine tests. It does not delete an existing `active.md`, backup, or Engram observation.

## Task 4: Canonical Verification And Mechanical Projection Synchronization

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v` once after focused GREEN. Expected: exit 0.
- [ ] Copy the canonical script and test bytes to the three package projection pairs named above. Verify exact SHA-256 equality across all four package roots for both relative paths.
- [ ] Using the installed manifest values, run the normal maintenance preview through `skill/repo-harness-builder/scripts/harness_tool.py render`. Review the exact digest and require replacements only for the intended generated start/stop skills and package projections; require no checkpoint file or directory action, manifest/checker change, deletion, migration, or unrelated projection.
- [ ] Only after separate implementation and maintenance-apply authorization, apply the unchanged reviewed digest. Repeat the same preview and require all mutating action lists to be empty.
- [ ] Compare the eight self-hosted generated skill files - canonical start/stop plus Claude Code, OpenCode, and Pi peers - with `render()` output for installed runtimes `claude-code`, `opencode`, and `pi`, license `MIT`, report language `english`, documentation locales `en` and `es`, and conversation language `espanol`.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, and zero writes. Rerun a projected suite only if SHA parity fails or runtime-specific bytes differ.
- [ ] Projection rollback restores canonical script/tests first, synchronizes their exact package copies, and reruns maintenance rendering to restore prior generated skills. It never removes local checkpoint content automatically.

## Task 5: Documentation And Lifecycle Closure

- [ ] Only after verified implementation and separate lifecycle authority, reconcile `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, both RH-W-0053 design and plan peers, `docs/repo-harness/README.md`, `docs/es/repo-harness/README.md`, `CHANGELOG.md`, `CHANGELOG.es.md`, and Engram topic `work/RH-W-0053`.
- [ ] Update `docs/repo-harness/usage.md` and its Spanish peer only if the implemented user-facing route needs durable human guidance. Keep the schema with its owning contract instead of copying it across documents.
- [ ] Keep RH-W-0053 `proposed` until the lifecycle owner separately authorizes advancement. Plan approval alone does not authorize implementation; verified implementation alone does not authorize lifecycle closure, staging, commit, push, publication, or network access.
- [ ] Run the focused lifecycle, dynamic-count, link, map, and parity checks listed below, then `git diff --check`.

## Verification Commands

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_current_state_portfolio_counts_are_derived_from_the_roadmap GeneratedCoreTests.test_current_state_action_plan_coverage_is_derived_from_the_roadmap GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v
python -c "from pathlib import Path; pairs=[('docs/repo-harness/rh-w-0053-implementation-plan.md','docs/es/repo-harness/rh-w-0053-implementation-plan.md'),('docs/repo-harness/README.md','docs/es/repo-harness/README.md'),('docs/repo-harness/roadmap.md','docs/es/repo-harness/roadmap.md'),('docs/repo-harness/current-state.md','docs/es/repo-harness/current-state.md')]; missing=[p for pair in pairs for p in pair if not Path(p).is_file()]; assert not missing,missing; assert all('RH-W-0053' in Path(p).read_text(encoding='utf-8') for pair in pairs for p in pair)"
python .repo-harness/check.py
git diff --check
```

Expected: all selected tests pass; bilingual owners and links exist; the checker reports `current` with zero writes; and the diff has no whitespace errors. The existing dynamic tests, not typed plan prose, own portfolio and action-plan counts.

## Review Workload Forecast And Remaining Authority

Forecast: 180 to 260 authored implementation and test lines, plus 120 to 220 mechanical package/generated projection lines and focused lifecycle documentation. Risk is high for authority semantics and medium for safe replacement wording, but low for runtime complexity because no executable checkpoint subsystem is added. Review in three bounded units: write/schema, discovery/resume, then projections/lifecycle. Keep RED beside its owning GREEN; the repository's approximately 400-line authored review target is not expected to be exceeded.

Remaining authority: implementation is completed. Any future amendment, maintenance apply, lifecycle advancement, Git delivery, commit, push, publication, network access, or runtime registered-name validation remains governed by its current owner. This historical plan claimed none of those delivery or registered-freshness outcomes at its execution stage. Its later delivery through issue #170 and PR #171 was squash-merged as `c7290fcf6c703df0839699a9cb8b77686bf18e74`; runtime registered-name freshness remains unverified.
