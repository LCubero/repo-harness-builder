# RH-W-0069 Native Session Continuity Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILLS: use `test-driven-development` for every behavior change, `work-unit-commits` for the planned boundaries, and `verification-before-completion` before delivery. Execute tasks in order and only after RH-W-0051 is proven. This plan authorizes no implementation, lifecycle transition, Git action, publication, or network access.

**Goal:** Extend the generated session contracts so an executing agent writes and reads native structured Markdown continuity backups with the approved local-first stop order, repository-first start order, independent Engram outcomes, visible reconciliation, and bounded privacy.

**Architecture:** `session_start_skill()` (currently defined near line 1134) and `session_stop_skill()` (currently defined near line 1266) render instruction contracts; they are not runtime callers of Python continuity APIs. Add only concise renderer text that directs the executing agent to use existing guarded repository-relative file operations. The memory-rule renderer owns shared schema, authority, privacy, and outcome vocabulary so start and stop reference rather than duplicate it.

**Tech Stack:** Python 3.10+ standard library, `unittest`, deterministic generated Markdown, existing renderer and generated-byte checks, and byte-identical canonical, OpenCode, and Claude package projections.

**Lifecycle owner:** [RH-W-0069](roadmap.md#rh-w-0069), status `proposed`. The approved [design](rh-w-0069-design.md) owns behavior. Required predecessor: completed and proven RH-W-0051 implementation. Planning changes no status and grants no implementation authority.

## Proven Call Paths And File Map

| Path or symbol | Planned responsibility and call path |
|---|---|
| `render()` -> `render_with_skill_mapping()` -> `session_start_skill()` | Render repository-first startup instructions into the generated session-start skill. |
| `render()` -> `render_with_skill_mapping()` -> `session_stop_skill()` | Render local-first pause/close instructions into the generated session-stop skill. |
| Existing memory-rule renderer | Render the one shared schema, authority, privacy, and source-outcome contract consumed as instructions by both generated skills. |
| `GeneratedCoreTests` | Assert exact integrated and standalone generated contracts, exclusions, and non-duplication. |
| Generated session-start/session-stop and memory rule | Direct an executing agent to perform bounded reads and exclusive writes under `.repo-harness/memory-bk/`; no Python callback or orchestration subsystem is introduced. |
| Package projections and self-hosted generated files | Mechanical exact-byte synchronization only after canonical GREEN. |

The plan adds none of these APIs: `render_continuity_backup`, `filter_continuity_summary`, `write_continuity_backup`, `read_continuity`, `reconcile_continuity`, or `persist_continuity`. It adds no callbacks, generic memory API, database, encryption framework, daemon, scheduler, service, synchronization engine, pluggable storage, hidden state machine, retention cleanup, approval reuse, checkpoint, lifecycle copy, receipt, delivery store, or conflict store.

## Task 1: Predecessor Gate And RED/GREEN For Session-Stop

**Files:** canonical script and tests only.

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`. Expected: five tests pass. Then run `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }; $manifest = Get-Content -LiteralPath ".repo-harness/manifest.json" -Raw | ConvertFrom-Json; if ($manifest.localState.'memory-bk'.path -ne ".repo-harness/memory-bk/") { throw "manifest must reserve memory-bk" }`. Expected: no exception. Stop if either command fails.
- [ ] Add `GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup` and `test_session_stop_reports_local_engram_and_cross_check_independently`.
- [ ] Assert generated stop instructions require: refresh repository evidence; build the required structured summary; apply the shared privacy filter; resolve `.repo-harness/memory-bk/` through the selected root and existing guarding rules; lazily create only that direct directory; create `YYYYMMDDTHHMMSSZ-<pause|close>-<short-session-id>.md` exclusively; reopen and structurally verify it; then attempt Engram when integrated; then cross-check only if both writes succeed.
- [ ] Assert exact aggregate outcomes from the design: local failure is `failed`; local success with unavailable or failed Engram is `degraded`; both success and agreement is `succeeded`; both success with conflict is `degraded`. Each source outcome remains visible.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_requires_native_local_first_pause_and_close_backup GeneratedCoreTests.test_session_stop_reports_local_engram_and_cross_check_independently -v`.
- [ ] Expected RED: required local-first, exclusive-create, readback, and independent-outcome clauses are absent. Existing imports and renderer calls must succeed.
- [ ] Minimally extend `session_stop_skill()` with direct instruction text and a reference to the shared memory rule. Do not add a callable persistence helper, callback, or return-value protocol to `harness_tool.py`.
- [ ] Repeat the command. Expected GREEN: two tests pass.
- [ ] Planned lifecycle behavior commit after separate authorization: `feat(harness): require local-first continuity on session stop`. Rollback first reverts canonical renderer and test changes, then uses the maintenance path in Task 4 to regenerate prior bytes. Existing backups and Engram observations remain untouched.

## Task 2: RED/GREEN For Session-Start And Reconciliation

**Files:** canonical script and tests only.

- [ ] Add `test_session_start_requires_repository_first_bounded_continuity_read`, `test_session_start_validates_newest_backup_before_use`, and `test_session_start_reports_conflicts_without_merging`.
- [ ] Assert the generated sequence: validate selected root; read applicable repository owners, source, tests, Git, and checker evidence; read scoped Engram when integrated; inspect only direct regular `.md` files in the guarded backup directory; accept the newest only when filename and frontmatter agree; read at most one older file only for `newest_invalid`, `objective_names_backup`, `conflict_needs_chronology`, or `newest_lacks_relevant_continuity`.
- [ ] Assert absent directory or empty history is `not_found`; malformed, unsafe, indirect, or contradictory candidates are rejected and reported without repair. Startup performs no write, staging, automatic continuation, or link following.
- [ ] Assert recalled claims are checked against current repository owners and reported as `accepted`, `stale`, `conflicting`, `unavailable`, or `unverifiable`. Repository-resolved conflicts use repository evidence visibly; unresolved material conflicts produce one human decision question and no merge.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_requires_repository_first_bounded_continuity_read GeneratedCoreTests.test_session_start_validates_newest_backup_before_use GeneratedCoreTests.test_session_start_reports_conflicts_without_merging -v`.
- [ ] Expected RED: exact ordering, bounded history, validation, and no-merge clauses are absent.
- [ ] Minimally extend `session_start_skill()` with direct read instructions and a reference to shared authority/outcomes. Do not add a reader, reconciler, callback ordering fixture, or generic subsystem.
- [ ] Repeat the command. Expected GREEN: three tests pass.
- [ ] Planned lifecycle behavior commit: `feat(harness): require bounded continuity on session start`. Rollback first reverts canonical renderer and test changes, then uses the maintenance path in Task 4 to regenerate prior bytes.

## Task 3: RED/GREEN For The Shared Schema, Privacy, And Exclusions

**Files:** canonical memory-rule renderer and `GeneratedCoreTests` only.

- [ ] Add `test_memory_rule_declares_native_backup_schema_authority_and_privacy`, `test_integrated_and_standalone_continuity_keep_source_outcomes_independent`, and `test_generated_continuity_excludes_runtime_memory_and_stateful_overreach`.
- [ ] Assert one shared contract defines the exact frontmatter (`project`, `timestamp_utc`, `source`, `session_id`), `# Session Continuity`, and required sections: Goal, Instructions, Discoveries, Accomplished, Next Steps, Relevant Files. Empty sections use `None.`; relevant files are repository-relative; applicable claims carry the approved evidence classification.
- [ ] Assert the shared filter omits credentials, raw environment values, provider payloads, outside-root absolute paths, copied source, diffs, logs, stack traces, generated bytes, test output, sensitive personal data, approvals, checkpoints, lifecycle copies, receipts, delivery state, and conflict state. It reports bounded privacy reduction without copying rejected values.
- [ ] Assert authority order matches the design: source/tests, roadmap, current state, decisions, changelog, Git/checker, then non-authoritative Engram/local evidence. Built-in runtime memory is ignored and never required. Integrated mode keeps Engram independent; standalone mode marks Engram `not_applicable` and does not emulate it.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_memory_rule_declares_native_backup_schema_authority_and_privacy GeneratedCoreTests.test_integrated_and_standalone_continuity_keep_source_outcomes_independent GeneratedCoreTests.test_generated_continuity_excludes_runtime_memory_and_stateful_overreach -v`.
- [ ] Expected RED: the shared schema, privacy set, authority order, or exclusions are absent. Existing integrated independence assertions may remain passing controls.
- [ ] Extend only the memory-rule renderer and concise references from start/stop. Avoid executable pseudocode duplication and add no parser or secret-detection infrastructure.
- [ ] Repeat the selected-test command, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Expected GREEN: the selected tests pass and the complete `GeneratedCoreTests` class passes.
- [ ] Planned shared-contract commit: `feat(harness): define native continuity contract`. Rollback first reverts canonical memory-rule and test changes, then uses the maintenance path in Task 4 to regenerate prior bytes.

## Task 4: Canonical GREEN, Mechanical Projections, And Maintenance Apply

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
$previewJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence
if ($LASTEXITCODE -ne 0) { throw "maintenance preview failed" }
$preview = $previewJson | ConvertFrom-Json
$approvedPlanSha256 = [string]$preview.planSha256
if ($approvedPlanSha256 -notmatch '^[0-9a-f]{64}$') { throw "preview did not return a valid planSha256" }
```

The test-safe schema example is `{"surface":"mcp","status":"verified","evidence":"test fixture capability"}`. It is only for parser or fixture tests and must never be supplied to a production preview or apply as proof of live capability.

- [ ] Review the preview. Expected: replacements only for intended generated session/memory outputs and projections; no RH-W-0069 manifest/checker field, backup directory, backup file, migration, or deletion action.
- [ ] Without changing files or arguments, run:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Repeat the same preview command and parse it with `ConvertFrom-Json`. Expected: every action list is empty. Then run `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='english'); paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/rules/memory.md','.opencode/skills/repo-harness-session-start/SKILL.md','.opencode/skills/repo-harness-session-stop/SKILL.md','.claude/skills/repo-harness-session-start/SKILL.md','.claude/skills/repo-harness-session-stop/SKILL.md']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad"`. Expected: exit 0 and no output. Finally run `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Expected: no exception.
- [ ] Planned mechanical commit: `chore(harness): synchronize native continuity contracts`. Keep it separate from RH-W-0051. Rollback first reverts canonical source changes, then uses this maintenance path to regenerate prior bytes. Never delete backups or Engram observations.

## Task 5: Documentation, Full Verification, Lifecycle, And Delivery Boundary

- [ ] Update usage and memory documentation plus bilingual durable peers only where implemented user-visible instructions require it. Keep one owner per fact; do not copy the schema into unrelated documents.
- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`, `python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v`, and `python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v`. Expected: each exits 0 and reports the same test total; runtime-specific skips may differ.
- [ ] Run `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test`; then run the exact projection parity, generated-byte, and checker commands from Task 4. Expected: self-test prints `pass`; parity and byte checks exit 0 with no output; checker is `current` with zero writes.
- [ ] Treat the exact full-suite, checker, projection-parity, and generated-byte commands above as the required regression evidence for path safety, no-write, replacement, and preservation. Add no duplicate negative matrix unless one of those commands exposes an unproved requirement.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings -v`. Expected: four tests pass.
- [ ] Run `python -c "from pathlib import Path; import re; roots=[Path('README.md'),Path('README.es.md'),Path('REPO-HARNESS.md'),Path('REPO-HARNESS.es.md'),Path('CONTRIBUTING.md'),Path('docs')]; files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*.md'))]; bad=[]; pattern=re.compile(r'\[[^\]]+\]\(([^)]+)\)'); [(bad.append(f'{p}:{target}') if not (target.startswith(('#','http://','https://','mailto:')) or (p.parent/target.split('#',1)[0]).resolve().exists()) else None) for p in files for target in pattern.findall(p.read_text(encoding='utf-8'))]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `python -c "from pathlib import Path; files=[Path(p) for p in ['docs/repo-harness/rh-w-0051-implementation-plan.md','docs/es/repo-harness/rh-w-0051-implementation-plan.md','docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md']]; bad=[str(p) for p in files if any(c in p.read_text(encoding='utf-8') for c in (chr(0x2013),chr(0x2014)))]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `python -c "from pathlib import Path; names=('render_continuity_backup','filter_continuity_summary','write_continuity_backup','read_continuity','reconcile_continuity','persist_continuity','continuity_orchestrator','continuity_callback','continuity_database','continuity_daemon','continuity_conflict_store'); roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder'),Path('.repo-harness/skills'),Path('.repo-harness/rules')]; files=[p for r in roots for p in r.rglob('*') if p.is_file()]; bad=[f'{p}:{name}' for p in files for name in names if name in p.read_text(encoding='utf-8',errors='ignore')]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `git diff --check`; if `$LASTEXITCODE -ne 0`, stop. Then run `git status --short`, `git diff --stat`, and `git diff --name-only`. Inspect those exact paths for callbacks, databases, services, hidden state, migration, deletion, built-in-memory dependency, silent merge, approval reuse, checkpoints, receipts, delivery stores, or unrelated changes. Expected: no whitespace errors, no prohibited implementation, and pre-existing unrelated work remains untouched.
- [ ] Only after verified implementation and separate lifecycle authority, reconcile roadmap, current state, changelogs, Spanish peers, suite count, and Engram topic `work/RH-W-0069`. Continuity evidence does not close lifecycle by itself.
- [ ] Future issue, commit, PR, push, publication, or network action follows `CONTRIBUTING.md` under separate approval. RH-W-0069 retains its own lifecycle and delivery unit.

## Acceptance And Forecast

- [ ] RH-W-0051 is proven first.
- [ ] Generated stop instructions require direct exclusive local write and readback before independent Engram work.
- [ ] Generated start instructions require repository-first evidence, bounded valid backup reads, and visible no-merge reconciliation.
- [ ] One shared memory rule owns schema, privacy, authority, outcomes, and exclusions.
- [ ] No Python continuity API, callback orchestration, generic subsystem, manifest field, checker content inspection, migration, or deletion is added.
- [ ] Canonical behavior, mechanical projections, lifecycle, rollback, and delivery remain separate from RH-W-0051.

Forecast: 5 tasks, 280 to 380 authored changed lines plus mechanical package projections and self-hosted generated bytes. The range covers renderer text, focused generated-contract tests, and only required user-facing documentation; no runtime subsystem is forecast. Implementation and lifecycle advancement remain separately unauthorized.
