# RH-W-0048 Runtime Projection Recovery Implementation Plan

> **Status: proposed (reopened); historical plan owner-approved.** The owner explicitly approved this bilingual plan, and its implementation was separately owner-authorized, completed locally, and verified. The only remaining work is mutability-contract reconciliation and independent re-verification. Native review is unavailable because the package-local binary is missing, so no lineage exists; that is not a verdict. Delivery—including Git delivery, network, commit, push, PR, release, and publication—remains separate and unperformed.

Derived from the owner-approved [recovery design](rh-w-0048-design.md). Spanish peer: [rh-w-0048-implementation-plan.md](../es/repo-harness/rh-w-0048-implementation-plan.md). The [roadmap record](roadmap.md#rh-w-0048) remains the lifecycle owner.

## Exact Architecture And File Map

| Owner | Planned responsibility |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::check_script()` | Extend only the emitted installed checker. Reuse its authenticated `parse_manifest()`, guarded path checks, canonical skill/command/projection mappings, and `check_projections()` comparison boundary. Add the generated-checker `repair` parser, read-only preflight, deterministic preview digest, exclusive creation, rollback, and post-check report here. Do not route repair through the builder CLI `main()`. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCheckScriptTests` | Own isolated installed-checker RED/GREEN behavior with temporary roots, including a temporary Git index for exact tracked-path evidence. |
| `.repo-harness/check.py` | Regenerated self-hosted checker only after canonical GREEN and separately authorized maintenance apply. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Mechanical projections of the two canonical package files; all three runtime packages remain byte-identical to their canonical peers. |

No manifest schema, `canonicalProjections` shape, `.gitignore`, `.gitattributes`, renderer update authority, runtime reload behavior, or human documentation is a product implementation target. `repair` is emitted by the installed checker because the installed harness must recover without an external builder installation.

## Strict-TDD Execution (Historical Executed Evidence)

The unchecked boxes preserve the pre-execution plan notation only. The RED, GREEN, synchronization, and verification work below was later executed and verified; the roadmap retains the authoritative receipts and completed lifecycle record.

### RED A: Plain Check And Dynamic Classification

- [ ] Add `GeneratedCheckScriptTests.test_plain_check_remains_read_only_when_repair_is_available` and `test_repair_preview_derives_missing_local_targets_from_authenticated_manifest`.
- [ ] Assert plain `python .repo-harness/check.py` preserves `writes: 0`, accepts no implicit repair behavior, and retains ordinary drift semantics.
- [ ] Derive expected targets from the authenticated manifest's selected `canonicalSkills`, `canonicalCommands`, and applicable `canonicalProjections`; calculate the expected set in the fixture. Assert a preview classification for every derived target, with every exclusion carrying a bounded reason; do not assert the historical six missing projections as a current count.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_plain_check_remains_read_only_when_repair_is_available GeneratedCheckScriptTests.test_repair_preview_derives_missing_local_targets_from_authenticated_manifest -v`. Expected RED: missing repair contract failures only.

### RED B: Preservation And Authority

- [ ] Add `test_repair_reports_missing_tracked_and_preserves_stale_targets` and `test_repair_apply_requires_current_preview_digest_and_rechecks_preflight`.
- [ ] In a temporary initialized Git repository, add the exact target to the index, remove it from the worktree, and require `missing-tracked`, no creation, and preserved drift. In a non-Git or untracked fixture, require `missing-local` eligibility.
- [ ] Assert stale, unknown, indirect, unsafe, unreadable, and canonical-missing cases write nothing; stale means existing bytes differ and remains distinct from repair-report vocabulary such as `skipped`, `refused`, `created`, `rollback`, and `post-check`.
- [ ] Assert that `planSha256` binds the authenticated manifest digest together with sorted relative creation paths and their expected-content digests. Separately assert that the preview response reports the authenticated manifest digest, every target classification, exclusions with bounded reasons, and `writes: 0`, and explicitly succeeds when the eligible set is empty; apply accepts only the unchanged digest and repeats preflight before its first write.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_repair_reports_missing_tracked_and_preserves_stale_targets GeneratedCheckScriptTests.test_repair_apply_requires_current_preview_digest_and_rechecks_preflight -v`. Expected RED: missing classification and authority failures only.

### RED C: Transaction And Honest Result

- [ ] Add `test_repair_creates_only_dynamic_missing_local_targets_and_post_checks` and `test_repair_rolls_back_only_unchanged_files_created_by_that_invocation`.
- [ ] Cover a fresh-clone and partial-tree fixture for each selected Claude Code, OpenCode, and Pi target family; expected creation counts remain manifest-derived.
- [ ] Force a later creation or verification failure. Assert rollback removes only same-invocation files whose bytes still match, may remove only same-invocation empty parents, and preserves pre-existing or changed bytes.
- [ ] Assert remaining stale, tracked-missing, canonical failure, partial rollback, and unverified runtime pickup are reported separately and never summarized as recovered.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests.test_repair_creates_only_dynamic_missing_local_targets_and_post_checks GeneratedCheckScriptTests.test_repair_rolls_back_only_unchanged_files_created_by_that_invocation -v`. Expected RED: missing transaction and result failures only.

### GREEN: Minimum Implementation And Projection Synchronization

- [ ] Implement only the generated-checker behavior named above. Preserve `check_projections()` as the plain-check owner; classify every target and report exclusions with bounded reasons before any repair decision; keep preview read-only (`writes: 0`) and explicitly successful for an empty eligible set; and bind the authenticated manifest digest together with sorted relative creation paths and expected-content digests through `planSha256`.
- [ ] Repeat each RED command, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests -v`. Expected GREEN: exit 0.
- [ ] Run the existing maintenance preview, review its digest, and only under separately authorized apply regenerate `.repo-harness/check.py` and all three complete builder-package projections. Do not hand-edit projections.
- [ ] Verify the manifest and `.gitattributes` retain their exact bytes; local-only recovery adds no attribute rule. A stale target receives an update recommendation, not a repair overwrite.

## Focused Verification

- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCheckScriptTests -v`, then repeat it from each of `.claude/skills/repo-harness-builder/tests/test_harness_tool.py`, `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py`, and `.pi/skills/repo-harness-builder/tests/test_harness_tool.py`. Expected: equal passing outcomes.
- [ ] Run `python -B .repo-harness/check.py`. Expected: `current`, exit 0, and `writes: 0` after projection synchronization.
- [ ] Run `python -c "from pathlib import Path; import hashlib; rel=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.claude/skills/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.pi/skills/repo-harness-builder')]; bad=[p for p in rel if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"`. Expected: exit 0 and no output.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four passes after separately authorized documentation/lifecycle closure.
- [ ] Run `git diff --check`. Expected: exit 0.

## Preservation, Closure, And Delivery Separation

Repair never overwrites, normalizes, stages, commits, pushes, deletes, initializes Git, reloads a runtime, or reaches the network. Its rollback boundary is limited to unchanged files and empty parents created by that invocation. Update remains separately authorized; RH-W-0052 retains archive, uninstall, reinstall, and deletion.

The separately owner-authorized implementation is completed locally and verified historical evidence. This plan, both maps, roadmap, current-state, Spanish peers, and chronology retain that evidence while RH-W-0048 is reopened only for mutability-contract reconciliation and independent re-verification. Plan approval and implementation authority remain distinct. Native review is unavailable because the package-local binary is missing, so no lineage exists; that is not a verdict. Commit, push, PR, delivery, publication, release, and network remain independent, unauthorized, and unperformed.

## Review Workload Forecast And Non-Goals

Historical forecast (superseded): 220 to 360 authored lines across the canonical script and tests, with a sub-400-line authored review budget. Actual canonical source/test churn is 648 additions and 7 deletions, superseding that forecast and budget. Independent verification found no findings. Native review was unavailable because the package-local binary was missing, so no lineage was created; no native review is claimed. Mechanical synchronization: seven generated files. Risk remains medium-high because the installed checker mutates only under explicit repair authority; the historical review slices are RED A/B, RED C plus GREEN, then projections and owner-authorized lifecycle closure.

Non-goals: a second manifest or inventory, tracked/local manifest mode, copied template store, cache, ledger, database, service, daemon, scheduler, runtime API, migration, reinstall, archive, uninstall, deletion, `.gitattributes` redesign, external builder dependency, or any product implementation beyond the approved recovery contract.

Resolved owner decision: the owner approved this plan; implementation was separately owner-authorized, completed locally, and verified. RH-W-0048 is `proposed`, reopened only for mutability-contract reconciliation and independent re-verification. Historical independent verification found no findings. Native review is unavailable because the package-local binary is missing, so no lineage exists; that is not a verdict. Delivery remains unclaimed and unperformed.
