# RH-W-0069 Native Session Continuity Implementation Plan

> **Status: proposed; owner-approved in the 2026-09-06 conversation.** Implementation remains separately unauthorized. This approval authorizes no product change, lifecycle transition, Git action, publication, or network access.

## Execution Readiness Reconciliation

The design is approved and RH-W-0051 storage support is delivered. In the 2026-09-06 conversation, the owner explicitly approved this reconciled simplified plan with `dale` after the assistant recommended it. That approval is documentary evidence for the plan boundary only. Implementation still requires a later, separate owner authorization, and RH-W-0069 remains `proposed` under the roadmap lifecycle convention.

The commands below use the current authenticated installed values: runtimes `claude-code`, `opencode`, and `pi`; license `MIT`; report language `english`; documentation locales `en` and `es`; conversation language `espanol`; and maintenance activation. Re-read the installed manifest immediately before a future authorized execution and update the commands if those declarations changed. An example must never change installed preferences.

The verification workload is proportional and no longer a separate approval blocker. Task 1 is one canonical TDD work unit with three ordered RED/GREEN behavior groups. Task 2 runs the full canonical suite once, then proves package copies by exact SHA parity and generated outputs by render/checker evidence. Task 3 owns focused documentation and lifecycle closure checks. Rerun a projected suite only if parity fails, runtime-specific source differs, or a focused check exposes an unproved requirement.

**Goal:** Extend the generated session contracts so an executing agent writes and reads native structured Markdown continuity backups with the approved local-first stop order, repository-first start order, independent Engram outcomes, visible reconciliation, and bounded privacy.

**Architecture:** `render()` calls `render_with_skill_mapping()`, which renders `session_start_skill()`, `session_stop_skill()`, and the shared memory rule from `rule_outputs()`. These are instruction contracts, not runtime callers of Python continuity APIs. Add only concise renderer text directing the executing agent to use guarded repository-relative file operations. The memory rule owns shared schema, authority, privacy, and outcome vocabulary so start and stop reference rather than duplicate it.

**Tech Stack:** Python 3.10+ standard library, `unittest`, deterministic generated Markdown, existing renderer and generated-byte checks, and byte-identical canonical, OpenCode, and Claude package projections.

**Lifecycle owner:** [RH-W-0069](roadmap.md#rh-w-0069), status `proposed`. The approved [design](rh-w-0069-design.md) owns behavior, and this plan is owner-approved. Required predecessor: completed and proven RH-W-0051 implementation. Plan approval changes no lifecycle status and grants no implementation authority.

## Proven Call Paths And File Map

| Path or symbol | Planned responsibility and call path |
|---|---|
| `render()` -> `render_with_skill_mapping()` -> `session_start_skill()` | Render repository-first startup instructions into the generated session-start skill. |
| `render()` -> `render_with_skill_mapping()` -> `session_stop_skill()` | Render local-first pause/close instructions into the generated session-stop skill. |
| `rule_outputs()` memory rule | Render the one shared schema, authority, privacy, and source-outcome contract consumed as instructions by both generated skills. |
| `GeneratedCoreTests` | Assert exact integrated and standalone generated contracts, exclusions, and non-duplication. |
| Generated session-start/session-stop and memory rule | Direct an executing agent to perform bounded reads and exclusive writes under `.repo-harness/memory-bk/`; no Python callback or orchestration subsystem is introduced. |
| Package projections and self-hosted generated files | Mechanical exact-byte synchronization only after canonical GREEN. |

The plan adds none of these APIs: `render_continuity_backup`, `filter_continuity_summary`, `write_continuity_backup`, `read_continuity`, `reconcile_continuity`, or `persist_continuity`. It adds no callbacks, generic memory API, database, encryption framework, daemon, scheduler, service, synchronization engine, pluggable storage, hidden state machine, retention cleanup, approval reuse, checkpoint, lifecycle copy, receipt, delivery store, or conflict store.

## Task 1: Canonical TDD For Native Continuity

**Files:** canonical script and tests only.

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`. Expected: five tests pass. Then run `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }; $manifest = Get-Content -LiteralPath ".repo-harness/manifest.json" -Raw | ConvertFrom-Json; if ($manifest.localState.'memory-bk'.path -ne ".repo-harness/memory-bk/") { throw "manifest must reserve memory-bk" }`. Expected: no exception. Stop if either command fails.
### RED/GREEN Group 1: Session-Stop Backup

- [ ] Add `GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup` and `test_session_stop_reports_local_engram_and_cross_check_independently`.
- [ ] Assert generated stop instructions always require the local Markdown backup: refresh repository evidence; build the required structured summary; apply the shared privacy filter; resolve `.repo-harness/memory-bk/` through the selected root and existing guarding rules; lazily create only that direct directory; create `YYYYMMDDTHHMMSSZ-<pause|close>-<short-session-id>.md` exclusively; reopen and structurally verify it; then attempt Engram when available; then cross-check only if both writes succeed.
- [ ] Assert exact aggregate outcomes from the design: local failure is `failed`; local success with unavailable or failed Engram is `degraded`; both success and agreement is `succeeded`; both success with conflict is `degraded`. Each source outcome remains visible.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup GeneratedCoreTests.test_session_stop_reports_local_engram_and_cross_check_independently -v`.
- [ ] Expected RED: required local-first, exclusive-create, readback, and independent-outcome clauses are absent. Existing imports and renderer calls must succeed.
- [ ] Minimally extend `session_stop_skill()` with direct instruction text and a reference to the shared memory rule. Do not add a callable persistence helper, callback, or return-value protocol to `harness_tool.py`.
- [ ] Repeat the command. Expected GREEN: two tests pass.
### RED/GREEN Group 2: Session-Start Recovery

- [ ] Add `test_session_start_requires_repository_first_bounded_continuity_read`, `test_session_start_validates_newest_backup_before_use`, and `test_session_start_reports_conflicts_without_merging`.
- [ ] Assert the generated sequence: validate selected root; read applicable repository owners, source, tests, Git, and checker evidence; inspect only direct regular `.md` files in the guarded backup directory and accept the newest valid file only when filename and frontmatter agree; then read scoped Engram when integrated; read at most one older file only for `newest_invalid`, `objective_names_backup`, `conflict_needs_chronology`, or `newest_lacks_relevant_continuity`.
- [ ] Assert absent directory or empty history is `not_found`; malformed, unsafe, indirect, or contradictory candidates are rejected and reported without repair. Startup performs no write, staging, automatic continuation, or link following.
- [ ] Assert recalled claims are checked against current repository owners and reported as `accepted`, `stale`, `conflicting`, `unavailable`, or `unverifiable`. Repository-resolved conflicts use repository evidence visibly; unresolved material conflicts produce one human decision question and no merge.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_requires_repository_first_bounded_continuity_read GeneratedCoreTests.test_session_start_validates_newest_backup_before_use GeneratedCoreTests.test_session_start_reports_conflicts_without_merging -v`.
- [ ] Expected RED: exact ordering, bounded history, validation, and no-merge clauses are absent.
- [ ] Minimally extend `session_start_skill()` with direct read instructions and a reference to shared authority/outcomes. Do not add a reader, reconciler, callback ordering fixture, or generic subsystem.
- [ ] Repeat the command. Expected GREEN: three tests pass.
### RED/GREEN Group 3: Shared Memory Rule

- [ ] Add `test_memory_rule_declares_native_backup_schema_authority_and_privacy`, `test_memory_rule_omits_external_private_repository_names_and_locations_without_approved_aliases`, `test_integrated_and_standalone_continuity_keep_source_outcomes_independent`, and `test_generated_continuity_excludes_runtime_memory_and_stateful_overreach`.
- [ ] Assert one shared contract defines the exact frontmatter (`project`, `timestamp_utc`, `source`, `session_id`), `# Session Continuity`, and required sections: Goal, Instructions, Discoveries, Accomplished, Next Steps, Relevant Files. Empty sections use `None.`; relevant files are repository-relative; applicable claims carry the approved evidence classification.
- [ ] Assert the shared filter omits credentials, raw environment values, provider payloads, outside-root absolute paths, copied source, diffs, logs, stack traces, generated bytes, test output, sensitive personal data, approvals, checkpoints, lifecycle copies, receipts, delivery state, and conflict state. It reports bounded privacy reduction without copying rejected values.
- [ ] Assert names and locations of external private repositories are omitted unless the applicable repository owner records an approved stable alias. A permitted alias may be retained, but the private name, absolute location, remote URL, and sibling path remain excluded.
- [ ] Assert authority order matches the design: source/tests, roadmap, current state, decisions, changelog, Git/checker, then non-authoritative Engram/local evidence. Built-in runtime memory is ignored and never required. Local backup is required in every mode. Engram is an additional independent destination only when capability evidence makes it available; an unavailable Engram operation is visible and never emulated.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_memory_rule_declares_native_backup_schema_authority_and_privacy GeneratedCoreTests.test_memory_rule_omits_external_private_repository_names_and_locations_without_approved_aliases GeneratedCoreTests.test_integrated_and_standalone_continuity_keep_source_outcomes_independent GeneratedCoreTests.test_generated_continuity_excludes_runtime_memory_and_stateful_overreach -v`.
- [ ] Expected RED: the shared schema, privacy set, authority order, or exclusions are absent. Existing integrated independence assertions may remain passing controls.
- [ ] Extend only the memory-rule renderer and concise references from start/stop. Avoid executable pseudocode duplication and add no parser or secret-detection infrastructure.
- [ ] Repeat the selected-test command. Expected GREEN: four tests pass.
- [ ] After all three groups are GREEN, run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Expected: the complete class passes.
- [ ] Recommended future functional commit boundary after separate authorization: `feat(harness): add native session continuity contracts`. This is a recommendation, not authority to stage or commit. Rollback first reverts canonical renderer and test changes, then uses Task 2 maintenance to regenerate prior bytes; existing backups and Engram observations remain untouched.

## Task 2: Canonical GREEN, Mechanical Projections, And Maintenance Apply

**Files:** canonical script/tests, package projections, and self-hosted generated session/memory files.

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0 before synchronization.
- [ ] Copy only the canonical script and test to matching OpenCode and Claude package paths. Then run `python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] From the repository root, run this exact PowerShell maintenance preview using verified host integration evidence, not memory content:

```powershell
$root = (Resolve-Path -LiteralPath ".").Path
if ([string]::IsNullOrWhiteSpace($env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON)) { throw "REPO_HARNESS_ENGRAM_EVIDENCE_JSON must contain host-verified Engram capability evidence" }
$engramEvidence = $env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON
$engramCapability = $engramEvidence | ConvertFrom-Json
if ($engramCapability.surface -notin @("mcp", "cli") -or $engramCapability.status -ne "verified" -or [string]::IsNullOrWhiteSpace([string]$engramCapability.evidence)) { throw "Engram evidence must have surface mcp|cli, status verified, and non-empty evidence" }
$previewJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json $engramEvidence
if ($LASTEXITCODE -ne 0) { throw "maintenance preview failed" }
$preview = $previewJson | ConvertFrom-Json
$approvedPlanSha256 = [string]$preview.planSha256
if ($approvedPlanSha256 -notmatch '^[0-9a-f]{64}$') { throw "preview did not return a valid planSha256" }
```

The test-safe schema example is `{"surface":"mcp","status":"verified","evidence":"test fixture capability"}`. It is only for parser or fixture tests and must never be supplied to a production preview or apply as proof of live capability.

- [ ] Review the preview. Expected: replacements only for intended generated session/memory outputs and projections; no RH-W-0069 manifest/checker field, backup directory, backup file, migration, or deletion action.
- [ ] Without changing files or arguments, run:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Repeat the same preview command and parse it with `ConvertFrom-Json`. Expected: every action list is empty. Then run `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='espanol'); paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/rules/memory.md','.opencode/skills/repo-harness-session-start/SKILL.md','.opencode/skills/repo-harness-session-stop/SKILL.md','.claude/skills/repo-harness-session-start/SKILL.md','.claude/skills/repo-harness-session-stop/SKILL.md']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad"`. Expected: exit 0 and no output. Finally run `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Expected: no exception.
- [ ] Recommended future mechanical commit boundary: `chore(harness): synchronize native continuity contracts`. This is a recommendation, not authority to stage or commit. Keep it separate from RH-W-0051. Rollback first reverts canonical source changes, then uses this maintenance path to regenerate prior bytes. Never delete backups or Engram observations.

## Task 3: Documentation, Lifecycle, And Delivery Boundary

- [ ] Conditional documentation, changed only when implemented user-visible behavior requires it: `docs/repo-harness/usage.md`, `docs/es/repo-harness/usage.md`, `.repo-harness/rules/memory.md`, `README.md`, `README.es.md`, `docs/repo-harness/README.md`, and `docs/es/repo-harness/README.md`. Keep one owner per fact and do not copy the schema into unrelated documents.
- [ ] Mandatory lifecycle closure paths, changed only after verified implementation and separate lifecycle authority: `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `CHANGELOG.md`, and `CHANGELOG.es.md`, plus Engram topic `work/RH-W-0069`. Conditional guidance updates do not make these lifecycle paths optional after authorized completion.
- [ ] Do not repeat the full canonical suite already run in Task 2 and do not run byte-identical projected suites by default. The Task 2 canonical suite, exact SHA parity, generated-byte comparison, and checker are the regression evidence unless a failure identifies a narrower missing check.
- [ ] Run `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test` once. Expected: `pass`.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: six focused canonical lifecycle and link tests pass. Exact test-file SHA parity from Task 2 covers projected copies.
- [ ] Run `python -c "from pathlib import Path; owner_pairs=[('docs/repo-harness/roadmap.md','docs/es/repo-harness/roadmap.md'),('docs/repo-harness/current-state.md','docs/es/repo-harness/current-state.md'),('CHANGELOG.md','CHANGELOG.es.md')]; plan_pair=('docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md'); map_pair=('docs/repo-harness/README.md','docs/es/repo-harness/README.md'); paths=[p for pair in (*owner_pairs,plan_pair,map_pair) for p in pair]; missing=[p for p in paths if not Path(p).is_file()]; assert not missing,missing; bad=[(a,b) for a,b in owner_pairs if (('RH-W-0069' in Path(a).read_text(encoding='utf-8')) != ('RH-W-0069' in Path(b).read_text(encoding='utf-8')))]; assert not bad,bad; assert all('RH-W-0069' in Path(p).read_text(encoding='utf-8') for p in plan_pair); assert all('rh-w-0069-implementation-plan.md' in Path(p).read_text(encoding='utf-8') for p in map_pair)"`. Expected: exit 0 and no output; this is the exact English/Spanish lifecycle, map, and plan projection selector, including the Spanish projection.
- [ ] Run `git diff --check`; if `$LASTEXITCODE -ne 0`, stop. Then run `git status --short`, `git diff --stat`, and `git diff --name-only`. Inspect those exact paths for callbacks, databases, services, hidden state, migration, deletion, built-in-memory dependency, silent merge, approval reuse, checkpoints, receipts, delivery stores, or unrelated changes. Expected: no whitespace errors, no prohibited implementation, and pre-existing unrelated work remains untouched.
- [ ] Only after verified implementation and separate lifecycle authority, reconcile roadmap, current state, changelogs, Spanish peers, suite count, and Engram topic `work/RH-W-0069`. Continuity evidence does not close lifecycle by itself.
- [ ] Future issue, commit, PR, push, publication, or network action follows `CONTRIBUTING.md` under separate approval. RH-W-0069 retains its own lifecycle and delivery unit.
- [ ] Recommended future documentary lifecycle commit boundary: `docs(harness): close RH-W-0069 lifecycle`. This is a recommendation only after verified implementation and separate lifecycle authority, not authority to stage or commit now.

## Acceptance And Review Workload Forecast

- [ ] RH-W-0051 remains proven as the completed namespace owner; the plan is owner-approved and separate implementation authorization remains.
- [ ] Generated stop instructions require direct exclusive local write and readback before independent Engram work.
- [ ] Generated start instructions require repository-first evidence, bounded valid backup reads, and visible no-merge reconciliation.
- [ ] One shared memory rule owns schema, privacy, authority, outcomes, and exclusions.
- [ ] No Python continuity API, callback orchestration, generic subsystem, manifest field, checker content inspection, migration, or deletion is added.
- [ ] Canonical behavior, mechanical projections, lifecycle, rollback, and delivery remain separate from RH-W-0051.

Forecast: 3 coherent boundaries: functional canonical TDD, mechanical projections, and documentary lifecycle closure. Expected volume remains 280 to 380 authored changed lines plus 180 to 300 mechanical projection or generated lines. Risk is medium because start, stop, privacy, ordering, and partial outcomes intersect, but no runtime subsystem is planned. The 400-line authored review budget is not expected to be exceeded. Commit boundaries are recommendations only; implementation, staging, commits, and lifecycle advancement remain separately unauthorized.
