# RH-W-0075 External Necessity And Justification Implementation Plan

> **Status: proposed.** Owner approval and separate implementation authorization are required. The record remains `proposed`.

Derived from the owner-approved [design](rh-w-0075-design.md). Spanish peer: [rh-w-0075-implementation-plan.md](../es/repo-harness/rh-w-0075-implementation-plan.md).

## Proven Surface And Boundaries

Canonical ownership is exact: `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs()` owns the full working-agreements necessity and four-field request contract. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` and `::session_stop_skill()` receive only conditional references where they already expose external or network gates. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` owns generated-contract assertions. `render()` calls `render_with_skill_mapping()`, which renders those owners. RH-W-0074 may also edit `.repo-harness/rules/working-agreements.md`, so exact clauses must be serialized or rebased, never merged semantically. RH-W-0075 owns only pre-request necessity and justification. Existing approval gates, RH-W-0074 post-answer behavior, RH-W-0079 optional-capability degradation, and builder digest-bound approval remain unchanged.

## Task 1: Focused RED

- [ ] Add `GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification`, `test_repository_local_solution_suppresses_external_request`, and `test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval` in `skill/repo-harness-builder/tests/test_harness_tool.py`.
- [ ] Assert outside-root paths, sibling repositories, network access, and provider tooling all require proof that the bounded objective cannot be completed in the selected repository.
- [ ] Assert one request names exact target, necessity reason, intended use, and consequence of decline. Necessity grants no access.
- [ ] Assert a repository-local solution produces no external request.
- [ ] Assert RH-W-0080 batch activation and grouped answers neither satisfy necessity nor supply, infer, reuse, or broaden approval for outside-root paths, sibling repositories, network access, or provider tooling.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification GeneratedCoreTests.test_repository_local_solution_suppresses_external_request GeneratedCoreTests.test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval -v`. Expected RED: three contract failures, with renderer import succeeding.

## Task 2: Minimal GREEN And Rollback

- [ ] Extend only the working-agreements content in `rule_outputs()` with the complete pre-request discipline.
- [ ] Add concise references in `session_start_skill()` and `session_stop_skill()` only where those contracts already ask for external or network authority. Do not duplicate the four-field contract.
- [ ] Preserve the separate approval gate. Decline grants no access and creates no remembered authority.
- [ ] Repeat Task 1, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests -v`. Expected GREEN: exit 0.
- [ ] Roll back this owner's tests and exact contract clauses together. Preserve any independent RH-W-0074 clauses in the shared file.

## Task 3: Projections, Documentation, And Lifecycle

- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v` only after focused GREEN. Expected: exit 0.
- [ ] Synchronize canonical script and test to `.opencode/skills/repo-harness-builder/` and `.claude/skills/repo-harness-builder/`, then use the normal maintenance renderer for generated rules and session skills. Review the digest before separately authorized apply. Expected second preview: no actions.
- [ ] After verified implementation and separate lifecycle authority, reconcile applicable guidance, roadmap, current state, maps, Spanish peers, RH-W-0068 changelog disposition, and Engram topic `work/RH-W-0075`.
- [ ] Keep issue, commit, review, rollback, and delivery independent from RH-W-0074. This plan authorizes no Git or delivery action.

## Verification

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_external_request_requires_strict_necessity_and_four_field_justification GeneratedCoreTests.test_repository_local_solution_suppresses_external_request GeneratedCoreTests.test_batch_activation_and_grouped_answers_cannot_bypass_necessity_or_supply_external_approval -v`, then repeat with `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` and `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`. Expected: three passes in each package with equal outcomes.
- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0 before projection synchronization.
- [ ] Run `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Preview with `$root=(Resolve-Path -LiteralPath '.').Path; $e=$env:REPO_HARNESS_ENGRAM_EVIDENCE_JSON; if ([string]::IsNullOrWhiteSpace($e)) { throw 'verified Engram evidence required' }; $j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'preview failed' }; $p=$j|ConvertFrom-Json; $d=[string]$p.planSha256; if ($d -notmatch '^[0-9a-f]{64}$') { throw 'invalid plan digest' }`. Expected: a valid preview and digest, with only intended rule and session projections changed.
- [ ] After separate apply authorization and without changing inputs, run `$j=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e --apply --approved-plan-sha256 $d; if ($LASTEXITCODE -ne 0) { throw 'apply failed' }; $a=$j|ConvertFrom-Json; if ($a.status -ne 'VALIDATED') { throw 'apply not validated' }; $j2=python -B skill/repo-harness-builder/scripts/harness_tool.py render --root $root --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language english --activation maintenance --engram-evidence-json $e; if ($LASTEXITCODE -ne 0) { throw 'second preview failed' }; $p2=$j2|ConvertFrom-Json; if (@($p2.create).Count + @($p2.append).Count + @($p2.replace).Count + @($p2.blocked).Count + @($p2.migrate).Count -ne 0) { throw 'second preview is not empty' }`. Expected: validated apply and empty second mutating preview.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, zero writes.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four pass.
- [ ] Run `git diff --check` and scan changed Markdown for U+2013 and U+2014. Expected: no findings.

## Review Workload Forecast

Authored change: 90 to 150 lines. Mechanical projection change: 60 to 120 lines. Risk: medium because of the RH-W-0074 edit collision, but behavior is additive and pre-request only. Recommended work units: RED plus canonical GREEN, then projections plus lifecycle documentation. The 400-line authored review budget is not at risk.

## Acceptance And Decisions

- [ ] All external classes use strict necessity and the four required fields.
- [ ] Local solutions suppress requests, approval stays separate, and decline grants nothing.
- [ ] No permission store, broker, telemetry, persistence, service, dependency, or framework is added.
- [ ] RH-W-0074 and builder digest-bound authority remain untouched.

Unresolved decisions: none for planning. Plan approval and implementation authorization remain separate.
