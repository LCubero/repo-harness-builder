# RH-W-0081 Host-Local Changelog Chronology Implementation Plan

> **Status: `completed`.** Owner-delegated minimal implementation completed 2026-10-09; delivery pending. Historical instructions below are not an execution queue.

Derived from the owner-approved [design](rh-w-0081-design.md). Spanish peer: [rh-w-0081-implementation-plan.md](../es/repo-harness/rh-w-0081-implementation-plan.md).

## Refreshed Scope (2026-10-09)

The owner delegated the next-objective decision ("decide it yourself, without overengineering"); selecting RH-W-0081 and refreshing its minimal plan under that delegation approved the plan and implementation. One clause rendered into session-stop `references/git-commit.md` now says: "Date each entry by the host-local calendar date of the completed change, preferring the exact change's local Git commit date; never substitute UTC, and never derive it from the UTC continuity backup timestamp. If that evidence is unavailable or ambiguous, ask one question."

One regression test, `GeneratedCoreTests.test_session_stop_dates_changelog_by_host_local_evidence`, observed RED then GREEN. The 2026-08-30 plan's separate UTC-independence test and heavier slices were superseded by this one clause and one test; `.pi` is included. No clock service, timezone logic, state or manifest change. Slice 2 (`fac022c6`), RH-W-0068 disposition and RH-W-0069 backup identity are unchanged.

Maintenance digest `2bbfb37f21057506c420a2786b4da3c3d37025885363484d75f27c4de63f425a` replaced exactly ten paths: script/test copies in `.claude`, `.opencode`, `.pi`, and four session-stop references including `.repo-harness`. Apply VALIDATED; second preview empty; checker current/0 writes; four-root script/test byte parity. Full suite: 631 tests, 11 skipped; only the stale documented-count assertion failed, corrected by this documentation update. No full-suite rerun is claimed here. No push, PR, issue or publication yet; delivery remains pending.

## Proven Surface And Ownership

`session_stop_skill()` renders the human-facing changelog guidance through `render_with_skill_mapping()`. `changelog()` seeds new files but does not derive dates. `DATE_HEADING` recognizes date headings. `GeneratedCoreTests.test_changelog_is_simple_completed_history` and `test_session_skills_treat_changelog_as_completed_history_only` cover chronology boundaries. RH-W-0068 alone decides whether an entry is needed. RH-W-0081 derives its host-local date. RH-W-0069 UTC backup identity and ordering are independent in both directions.

Exact evidence for the completed correction is local Git commit `1e5759e0` at `2026-08-28T22:33:40-06:00` and `c892cfb` at `2026-08-28T22:40:57-06:00`. Commit `fac022c6` moved both English and Spanish entries to the existing `2026-08-28` sections. Commit `b39e1ba` updated only the Plan 30 projection.

## Slice 1: Focused RED And Minimal Generated Contract

- [x] Added the single refreshed regression test and observed RED then GREEN.
- [x] Extended session-stop guidance minimally: host-local completed-change date, exact local Git preference, no UTC substitution or backup-timestamp derivation, one ambiguity question.
- [x] Preserved RH-W-0068 and RH-W-0069 ownership without new mechanics.

The original Slice 1 items below are superseded, not completed claims; distinct tests, timestamp precision assertions and old commands were not executed as written.

- [ ] Add `GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence` and `test_changelog_chronology_is_independent_from_continuity_utc_identity`.
- [ ] Assert the observed host-local system clock supplies chronology for completed non-Git work. For completed Git work, verified local Git evidence tied to the exact change is preferred. Preserve exact seconds and offset when a timestamp is shown, otherwise use local date plus offset.
- [ ] Assert one heading per host-local date, no silent UTC substitution, no invented rounded minute, and one separate resumable clarification when a non-Git clock observation is unavailable or local Git evidence is unavailable, contradictory, or ambiguous. Never derive chronology from filesystem creation, modification, or access timestamps.
- [ ] Assert RH-W-0069 filename timestamps and `timestamp_utc` neither derive nor receive changelog chronology.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence GeneratedCoreTests.test_changelog_chronology_is_independent_from_continuity_utc_identity -v`. Expected RED: missing chronology clauses only.
- [ ] Minimally extend `session_stop_skill()` changelog guidance. Do not add a clock service, timezone database, locale daemon, state, manifest field, checker state, dependency, or generic chronology API.
- [ ] Repeat that command, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_changelog_is_simple_completed_history GeneratedCoreTests.test_session_skills_treat_changelog_as_completed_history_only -v`. Expected GREEN: all pass.
- [ ] Rollback removes only RH-W-0081 test and renderer clauses.

## Slice 2: Historical Completed Bilingual Documentation Correction

- [x] Verified with local Git only: `git show -s --format=%H%n%cI 1e5759e0` and `git show -s --format=%H%n%cI c892cfb` returned `2026-08-28T22:33:40-06:00` and `2026-08-28T22:40:57-06:00`.
- [x] Moved the English entry from the false `2026-08-29` heading into the existing `2026-08-28` section.
- [x] Applied the equivalent move in `CHANGELOG.es.md`, preserving order and semantic parity.
- [x] Changed no entry content beyond the evidence-bound heading merge.
- [x] Kept this correction independent from generated product behavior in commit `fac022c6`.

## Slice 3: Projections, Lifecycle, And Delivery Boundary

Historical instructions below are superseded by the refreshed receipt above and this bounded bilingual reconciliation. Delivery remains pending.

- [ ] Run the canonical full suite after Slice 1 GREEN. Expected: exit 0.
- [ ] Synchronize canonical script and test package projections, then use the normal maintenance renderer for generated session-stop outputs. Review exact preview and digest before separately authorized apply. Expected second preview: no actions.
- [ ] After verified implementation and separate lifecycle authority, reconcile exactly `docs/repo-harness/roadmap.md`, `docs/es/repo-harness/roadmap.md`, `docs/repo-harness/current-state.md`, `docs/es/repo-harness/current-state.md`, `docs/repo-harness/README.md`, `docs/es/repo-harness/README.md`, `docs/repo-harness/rh-w-0081-design.md`, `docs/es/repo-harness/rh-w-0081-design.md`, `docs/repo-harness/rh-w-0081-implementation-plan.md`, `docs/es/repo-harness/rh-w-0081-implementation-plan.md`, `CHANGELOG.md`, `CHANGELOG.es.md`, RH-W-0068 disposition, and Engram `work/RH-W-0081`.
- [ ] Keep Slice 2 as an independently reviewable documentation correction inside this owner. Keep RH-W-0081 issue, commits, review, rollback, and delivery separate from RH-W-0068 and RH-W-0069. This plan authorizes no Git action now.

## Verification

Historical commands below are retained, not a current verification queue or claims of exact execution. The refreshed evidence above is authoritative for implementation; focused documentation checks are reported separately.

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_stop_derives_changelog_date_from_exact_host_local_evidence GeneratedCoreTests.test_changelog_chronology_is_independent_from_continuity_utc_identity GeneratedCoreTests.test_changelog_is_simple_completed_history GeneratedCoreTests.test_session_skills_treat_changelog_as_completed_history_only -v`, then repeat against `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` and `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`. Expected: four equal passes per package.
- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0. Run `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Expected: valid digest and only intended session-stop replacements.
- [ ] After separate apply authorization and without changing inputs, run `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Expected: `VALIDATED` and empty second mutating preview.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four pass.
- [ ] Run `python -c "from pathlib import Path; files=[Path('CHANGELOG.md'),Path('CHANGELOG.es.md')]; bad=[(str(p),p.read_text(encoding='utf-8').splitlines().count('## 2026-08-29'),p.read_text(encoding='utf-8').splitlines().count('## 2026-08-28')) for p in files if p.read_text(encoding='utf-8').splitlines().count('## 2026-08-29') != 0 or p.read_text(encoding='utf-8').splitlines().count('## 2026-08-28') != 1]; assert not bad,bad"` after Slice 2. Expected: exit 0 and no output.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, zero writes.
- [ ] Run `git diff --check` and scan changed Markdown for U+2013 and U+2014. Expected: no findings.

## Review Workload Forecast

Authored change: 90 to 150 lines, including the two changelog heading moves. Mechanical projection change: 30 to 80 lines. Risk: medium because chronology affects visible history, while implementation is prose-only generated guidance. Recommended work units: Slice 1 generated contract, Slice 2 bilingual correction, Slice 3 projections and lifecycle. The 400-line authored review budget is not at risk.

## Acceptance And Decisions

- [ ] Host-local evidence derives chronology without UTC substitution or rounded timestamps.
- [ ] Ambiguity asks one separate resumable question.
- [ ] RH-W-0068 disposition and RH-W-0069 UTC identity remain untouched.
- [ ] English and Spanish heading correction is exact, evidence-bound, and independently reversible.

Unresolved implementation decisions: none. Refreshed plan and implementation approved by owner delegation on 2026-10-09; delivery requires separate authority.
