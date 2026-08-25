# RH-W-0051 Local-State Taxonomy Implementation Plan

> **Status: COMPLETED ON 2026-08-25.** The execution instructions below are retained as historical evidence. Before execution, this plan granted no implementation, lifecycle, Git, publication, or network authority; implementation later proceeded under separate owner authorization.

**Goal:** Add the approved fixed local-state taxonomy, exact root ignore directives, one minimal `localState` manifest policy, and read-only checker validation without adding consumer behavior.

**Architecture:** Declare four constants and reuse existing `guarded_target()` wherever an authorized future consumer resolves a declared path. Extend the existing additive `.gitignore` plan and authenticated manifest/checker replacement path. Do not create directories, probe local contents, migrate files, or introduce another path API.

**Tech Stack:** Python 3.10+ standard library, `unittest`, deterministic JSON and Markdown rendering, and byte-identical canonical, OpenCode, and Claude package projections.

**Lifecycle owner:** [RH-W-0051](roadmap.md#rh-w-0051), status `completed`. The approved [design](rh-w-0051-design.md) owns behavior. RH-W-0069's storage prerequisite is proven. Historically, planning changed no status and granted no implementation authority.

## File Map

| Path or symbol | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Fixed taxonomy and ignore constants, manifest emission, existing additive ignore planning, and checker generation. |
| `guarded_target()` | Existing containment and indirect-ancestor protection; no replacement helper is added. |
| `render()` and `render_with_skill_mapping()` | Emit one ordered `localState` manifest field and four exact root directives. |
| `plan_changes()` and `check()` | Use existing authenticated replacement and read-only drift classification. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Focused RED/GREEN coverage for declaration, directives, replacement, absence, and zero writes. |
| OpenCode and Claude package projections | Mechanical byte-identical copies after canonical GREEN. |
| `.repo-harness/manifest.json`, `.repo-harness/check.py`, `.gitignore` | Self-hosted outputs changed only through reviewed maintenance preview/apply. |

No session behavior, local directory creation, content parser, retention engine, override syntax, generic state API, migration matrix, workspace discovery, database, service, daemon, or background work is added.

## Task 1: RED And GREEN For The Fixed Declaration

**Files:** canonical script and tests only.

- [ ] Add `LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy`, asserting this exact value at `json.loads(TOOL.render("Example", [])[TOOL.MANIFEST_PATH])["localState"]`:

```python
{
    "tmp": {"path": ".repo-harness/tmp/", "class": "disposable"},
    "archive": {"path": ".repo-harness/archive/", "class": "recoverable"},
    "state": {"path": ".repo-harness/state/", "class": "operational"},
    "memory-bk": {"path": ".repo-harness/memory-bk/", "class": "continuity-backup"},
}
```

- [ ] Add `test_render_adds_exact_root_local_state_ignore_directives`, asserting one occurrence each of `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/`, and `/.repo-harness/memory-bk/`, with no broad `/.repo-harness/` ignore.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_render_declares_exact_ordered_local_state_taxonomy LocalStateTaxonomyTests.test_render_adds_exact_root_local_state_ignore_directives -v`.
- [ ] Expected RED: the first test raises `KeyError: 'localState'`; the second reports the first missing directive. Import, syntax, or fixture failures are invalid RED.
- [ ] Add only fixed constants, manifest emission, and the four directives. Keep `guarded_target()` unchanged and use it later only from real consumers.
- [ ] Repeat the command. Expected GREEN: two tests pass.
- [ ] Planned work-unit commit after separate authorization: `feat(harness): declare local-state taxonomy`. Rollback first reverts these canonical source and test changes, then uses the maintenance path in Task 3 to regenerate prior bytes.

## Task 2: RED And GREEN For Checker And Replacement Policy

**Files:** canonical script and tests only.

- [ ] Add `test_check_accepts_absent_local_state_directories_with_zero_writes`, installing rendered outputs into a temporary root, asserting all four directories are absent, then asserting `status == "current"` and `writes == 0`.
- [ ] Add `test_check_reports_local_state_manifest_or_ignore_drift`, varying one taxonomy class and omitting one exact directive in subtests. Assert bounded drift without creating or reading a declared directory.
- [ ] Extend the existing `test_manifest_and_checker_change_as_one_consistent_replacement_pair` fixture with the desired `localState` field instead of adding a duplicate replacement test. Assert the authenticated pair remains one digest-bound replacement.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py LocalStateTaxonomyTests.test_check_accepts_absent_local_state_directories_with_zero_writes LocalStateTaxonomyTests.test_check_reports_local_state_manifest_or_ignore_drift ApprovedGeneratedOutputReplacementTests.test_manifest_and_checker_change_as_one_consistent_replacement_pair -v`.
- [ ] Expected RED: absent required directives or the undeclared field prevent `current`, and the replacement fixture does not yet contain `localState`. The absent-directory assertion is a passing negative control.
- [ ] Update exact manifest validation and generated checker constants. Reuse existing additive directive detection and authenticated replacement; add no migration branch and no filesystem probe.
- [ ] Repeat the command. Expected GREEN: all selected tests pass, no local-state directory exists, and replacement remains atomic.
- [ ] Planned work-unit commit: `feat(checker): validate local-state taxonomy`. Rollback first reverts canonical checker, manifest-policy, and test changes, then uses the maintenance path in Task 3 to regenerate prior bytes. Never delete local state.

## Task 3: Canonical GREEN, Mechanical Projections, And Maintenance Apply

**Files:** canonical script/tests, four package projections, `.repo-harness/manifest.json`, `.repo-harness/check.py`, and `.gitignore`.

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0. Stop before synchronization on failure.
- [ ] Copy only `scripts/harness_tool.py` and `tests/test_harness_tool.py` from the canonical package to matching OpenCode and Claude paths. Then run `python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] From the repository root, run this exact PowerShell maintenance preview using verified host integration evidence. Do not substitute memory content for capability evidence:

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

- [ ] Review the preview. Expected: only four exact ignore additions and authenticated generated-output replacements required by the canonical change; no local-state directory or file action.
- [ ] Without changing files or arguments between preview and apply, run:

```powershell
$applyJson = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $approvedPlanSha256
if ($LASTEXITCODE -ne 0) { throw "maintenance apply failed" }
$apply = $applyJson | ConvertFrom-Json
if ($apply.status -ne "VALIDATED") { throw "maintenance apply was not VALIDATED" }
```

- [ ] Run the same preview command again and parse it with `ConvertFrom-Json`. Expected: every action list is empty. Then run `python -c "from pathlib import Path; import importlib.util; p=Path('skill/repo-harness-builder/scripts/harness_tool.py'); s=importlib.util.spec_from_file_location('harness_tool',p); m=importlib.util.module_from_spec(s); s.loader.exec_module(m); out=m.render('repo-harness-builder',['claude-code','opencode','pi'],license_id='MIT',report_language='english',documentation_languages=['en','es'],conversation_language='english'); paths=[m.MANIFEST_PATH,'.repo-harness/check.py']; bad=[x for x in paths if Path(x).read_text(encoding='utf-8') != out[x]]; assert not bad,bad; absent=[x['path'] for x in __import__('json').loads(out[m.MANIFEST_PATH])['localState'].values() if Path(x['path']).exists()]; assert not absent,absent"`. Expected: exit 0 and no output.
- [ ] Planned mechanical commit: `chore(harness): synchronize local-state taxonomy`. Keep it separate from RH-W-0069. Rollback first reverts canonical source changes, then uses this maintenance path to regenerate prior bytes.

## Task 4: Full Verification, Lifecycle Evidence, And Delivery Boundary

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`, `python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v`, and `python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v`. Expected: each exits 0 and reports the same test total; runtime-specific skips may differ.
- [ ] Run `python -B skill/repo-harness-builder/scripts/harness_tool.py self-test`; then run the exact projection parity and generated-byte commands from Task 3. Expected: self-test prints `pass`; both commands exit 0 with no output.
- [ ] Run `$checkerJson = python .repo-harness/check.py; if ($LASTEXITCODE -ne 0) { throw "checker failed" }; $checker = $checkerJson | ConvertFrom-Json; if ($checker.status -ne "current" -or [int]$checker.writes -ne 0) { throw "checker must be current with zero writes" }`. Expected: no exception.
- [ ] Treat the three full-suite commands above as the required regression evidence for established path, non-Git, multi-root, no-probe, migration, and preservation behavior. Add no duplicate negative matrix unless one of those commands exposes an unproved requirement.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_documented_suite_count_matches_the_real_suite GeneratedCoreTests.test_checked_in_documents_do_not_mix_line_endings -v`. Expected: four tests pass.
- [ ] Run `python -c "from pathlib import Path; import re; roots=[Path('README.md'),Path('README.es.md'),Path('REPO-HARNESS.md'),Path('REPO-HARNESS.es.md'),Path('CONTRIBUTING.md'),Path('docs')]; files=[p for r in roots for p in ([r] if r.is_file() else r.rglob('*.md'))]; bad=[]; pattern=re.compile(r'\[[^\]]+\]\(([^)]+)\)'); [(bad.append(f'{p}:{target}') if not (target.startswith(('#','http://','https://','mailto:')) or (p.parent/target.split('#',1)[0]).resolve().exists()) else None) for p in files for target in pattern.findall(p.read_text(encoding='utf-8'))]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `python -c "from pathlib import Path; files=[Path(p) for p in ['docs/repo-harness/rh-w-0051-implementation-plan.md','docs/es/repo-harness/rh-w-0051-implementation-plan.md','docs/repo-harness/rh-w-0069-implementation-plan.md','docs/es/repo-harness/rh-w-0069-implementation-plan.md']]; bad=[str(p) for p in files if any(c in p.read_text(encoding='utf-8') for c in (chr(0x2013),chr(0x2014)))]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `git diff --check`; if `$LASTEXITCODE -ne 0`, stop. Then run `git status --short`, `git diff --stat`, and `git diff --name-only`. Expected: no whitespace errors, no new unrelated paths, and pre-existing unrelated work remains untouched.
- [ ] Only after verified implementation and separate lifecycle authority, reconcile roadmap, current state, changelogs, Spanish peers, suite count, and Engram topic `work/RH-W-0051`. Local GREEN does not advance status or authorize delivery.
- [ ] Future issue, commit, PR, push, publication, or network action follows `CONTRIBUTING.md` under separate approval.

## Acceptance And Forecast

- [ ] Four exact namespaces, one ordered `localState` field, and four exact root directives exist.
- [ ] Existing `guarded_target()` remains the sole path guard; no speculative local-state API exists.
- [ ] Checker validation is read-only, accepts absent directories, reports declaration or directive drift, and uses authenticated replacement.
- [ ] No consumer, migration, directory creation, local-content inspection, or unrelated storage behavior is added.
- [ ] RH-W-0051 remains a separate lifecycle and delivery unit before RH-W-0069.

Historical forecast: 4 tasks, 140 to 190 authored changed lines plus mechanical package projections and self-hosted generated bytes. The implementation reused existing guarding, ignore planning, checker, and replacement behavior, then completed under separate authorization.
