# RH-W-0052 Archive-Only Implementation Plan

> **Status: proposed; archive-only implementation complete and verified under separate owner authority.** RH-W-0052 remains `proposed` because only its archive-copy slice is implemented; uninstall, reinstall, and deletion remain undesigned and unauthorized. This execution record authorizes no lifecycle advancement, issue/PR work, Git delivery, network access, commit, push, publication, cleanup, or destructive operation.

Derived from the owner-approved [archive-only design](rh-w-0052-design.md). Spanish peer: [rh-w-0052-implementation-plan.md](../es/repo-harness/rh-w-0052-implementation-plan.md). The [roadmap record](roadmap.md#rh-w-0052) remains the lifecycle owner.

## Outcome And Boundaries

Implement only one explicit archive-copy operation: one named repository-relative direct regular source may produce one new copy beneath the existing `.repo-harness/archive/` namespace when an authenticated current manifest proves that exact source is harness-owned. Inspect and preview write nothing. A current explicit approval covers only the one displayed source-to-new-target mapping; material source, destination, ownership, or evidence change invalidates it. Apply creates only the approved new archive copy and preserves the source.

Every unresolved ownership proof, unsafe or indirect path, source outside the root or inside the archive namespace, destination collision, changed approval evidence, or missing/indirect/non-directory destination parent refuses without writing. The exact parent chain must preexist as direct real directories; apply performs no `mkdir`, rollback deletion, or cleanup. This owner-selected minimum avoids the impossible combination of absent parents, a race collision, zero-write refusal, and no rollback deletion. The operation does not assume Git and preserves all unrelated, unknown, local-state, human-owned, and tool-owned paths, including `.atl/`, `.codegraph/`, and `.engram/`.

This plan adds no uninstall, reinstall, deletion, move, overwrite, cleanup, migration, database, ledger, daemon, scheduler, generic archive framework, manifest schema, digest format, transaction subsystem, or generated command contract. It does not change RH-W-0051 namespace ownership or RH-W-0048 recovery behavior.

## Inspected Owners And Likely Surfaces

| Surface | Planned responsibility |
| --- | --- |
| `skill/repo-harness-builder/scripts/harness_tool.py::guarded_target()` | Reuse existing root containment and indirect-ancestor refusal; do not create a parallel path guard. |
| `skill/repo-harness-builder/scripts/harness_tool.py::authentic_installed_checker()`, `manifest_owned_generated_paths()`, and `generated_skill_paths()` | First prove checker currentness by exact equality with `check_script(digest_bytes(manifest_bytes))`; then use the existing helpers for authenticity and exact manifest coverage. Stale-but-authentic evidence is `ownership-unresolved`; unresolved evidence remains ineligible. |
| `skill/repo-harness-builder/scripts/harness_tool.py::plan_changes()` vicinity | Host one bounded archive-only inspect/preview/apply path, without changing the existing generated-output migration or recovery paths. The exact public invocation remains an implementation detail to be selected only under implementation authority. |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ArchiveOnlyTests` | Add the focused behavior-level contract tests as a new class, keeping legacy migration tests and their removal behavior out of scope. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}`, `.pi/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Mechanical byte-identical package projections only after canonical GREEN. No generated session skill, wrapper, manifest, or checker change is planned unless the authorized preview proves an existing managed projection must change. |

The inspected repository already has `safe_relative()`, `guarded_target()`, exact current-checker equality through `check_script(digest_bytes(manifest_bytes))`, authentication through `authentic_installed_checker()`, exact manifest-derived generated paths, digest-bound preview/apply tests, and direct/indirect path regressions. Those are evidence for reuse, not authority to reuse legacy migration removal, repair, or any Git behavior.

## Task 1: RED/GREEN A — Exact Eligibility And Zero-Write Inspect/Preview

- [ ] Add `ArchiveOnlyTests.test_archive_inspect_accepts_only_one_explicit_direct_regular_manifest_owned_source`, `test_archive_inspect_reports_ownership_unresolved_without_writing`, and `test_archive_preview_emits_one_exact_mapping_with_zero_writes`.
- [ ] Establish RED with an installed authenticated manifest/checker fixture. Assert that only one explicitly named direct regular source, covered by the current authenticated manifest, is eligible; absent, malformed, stale-but-authentic, stale, non-covering, or unauthenticated evidence reports `ownership-unresolved` and leaves every fixture byte unchanged.
- [ ] Assert that inspect and preview reject archive-internal, escaping, indirect, non-regular, and implicitly discovered sources; inspect only the named candidate and applicable ownership/destination evidence.
- [ ] Assert that preview returns exactly one source-to-new-archive-target mapping and `writes: 0`, with no directory creation, copy, move, manifest/checker edit, Git operation, or scan of unrelated or tool-owned paths.
- [ ] Implement the smallest bounded path using the inspected guards and ownership evidence. Do not alter `plan_changes()` migration behavior or add a manifest field.
- [ ] Repeat the focused selection. GREEN requires only these eligibility and zero-write assertions to pass.

## Task 2: RED/GREEN B — Exact Approval And Refusal Boundary

- [ ] Add `ArchiveOnlyTests.test_archive_preview_refuses_collision_and_unsafe_destination_without_writing` and `test_archive_approval_is_invalidated_by_material_mapping_or_ownership_change`.
- [ ] Establish RED for destination collision, unsafe destination, source-byte change, destination change, and ownership-evidence change after preview. Each case must refuse; it must not overwrite, select another target, or inherit a prior approval.
- [ ] Assert that approval is current, explicit, and bound only to the exact previewed mapping. No session, Git, manifest, repair, or prior archive approval supplies authority.
- [ ] Implement the smallest comparison necessary to re-establish the previewed facts immediately before apply. Do not define a new approval format, digest format, or transaction subsystem.
- [ ] Repeat the focused selection. GREEN requires all refusal and invalidation cases to pass with zero archive targets created.

## Task 3: RED/GREEN C — Apply Preservation And Failure Evidence

- [ ] Add `ArchiveOnlyTests.test_archive_apply_creates_only_the_approved_new_copy_and_preserves_source` and `test_archive_apply_refusal_preserves_local_unknown_and_tool_owned_paths`.
- [ ] Establish RED using byte snapshots of the source, unrelated paths, every pre-existing local-state path, and representative `.atl/`, `.codegraph/`, and `.engram/` paths. On success, assert one complete new archive copy and byte-identical preservation of every snapshot; on every refusal, assert no new target and no changed snapshot.
- [ ] Assert that apply rechecks direct regular source kind, containment, authenticated current-manifest coverage, destination absence, and exact current approval immediately before the one creation.
- [ ] Implement only the successful one-copy path and bounded refusal reporting. Do not move, delete, overwrite, normalize, repair, or clean up source or unrelated paths, and do not introduce rollback deletion.
- [ ] Repeat the focused selection, then run `ArchiveOnlyTests -v`. GREEN requires the class to pass.

## Task 4: Focused Verification And Mechanical Projection Conditions

- [ ] After focused GREEN, run the existing canonical standard-library suite required by `CONTRIBUTING.md` for product changes: `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"`.
- [ ] Copy only the canonical script and test bytes to the three exact package projection pairs named above; verify byte equality across all four package roots. Do not synchronize generated session skills, wrappers, manifests, or checkers unless the authorized maintenance preview identifies an existing managed consequence.
- [ ] Under separate implementation and maintenance authority, inspect the normal maintenance preview before any apply. It must show no archive source, local-state, tool-owned, deletion, migration, or unrelated action. Apply only an unchanged separately approved preview, then repeat preview and require no remaining managed action.
- [ ] Run `python .repo-harness/check.py`; require `current`, exit 0, and `writes: 0`. Run `git diff --check` and stop on whitespace errors. No Git repository, index, history, or network evidence is a prerequisite or a test input.
- [ ] If the full suite, projection parity, authorized preview, or checker condition fails, stop before lifecycle reconciliation. Do not broaden the implementation to repair unrelated drift.

## Task 5: Documentation, Lifecycle, And Review Boundary

- [ ] Only after verified implementation and separate lifecycle authority, reconcile the bilingual roadmap, current-state projection, documentation maps, this plan and design peers, both changelogs, and the `work/RH-W-0052` continuity topic if its capability is available. Keep the roadmap record `proposed` until separately authorized to advance it.
- [ ] Record only verified implementation receipts in the changelog. Plan approval, implementation, lifecycle advancement, issue/PR work, commit, push, publication, and network action remain distinct owner decisions.
- [ ] Review Task 1, Task 2, and Task 3 as separate behavior units. Keep each RED adjacent to its GREEN and keep each canonical authored unit below the repository's approximately 400 changed-line target when practical. Split a future implementation only if the observed focused diff exceeds that reviewable boundary or a projection consequence is independently material.

## Verified Strict-TDD Execution Evidence

The task sections above are retained as the approved historical plan. The separate archive-only implementation completed its three original RED/GREEN groups. Independent review first reported one high and one low finding. The correction then went RED and GREEN; a raced-directory case produced a medium finding. The owner selected the parent-precondition correction described above, followed by final correction and independent PASS.

Final receipts: `ArchiveOnlyTests` 9 passed/1 skipped; neighboring tests 132 passed/2 skipped; full canonical suite 545 passed/10 skipped. The checker reported `current`, exit 0, `writes: 0`, and 63 projections. Canonical, Claude, OpenCode, and Pi package bytes have exact parity; `git diff --check` was clean. The Windows source-symlink case was skipped because privilege was unavailable, while junction-parent refusal was exercised successfully. Native review stopped before START because managed Pi assets were outdated; it produced no lineage and no verdict.

## Preservation, Rollback, And Remaining Decisions

Preservation evidence is byte-for-byte before/after comparison of the explicit source and all protected fixture paths. A failure or refusal proves preservation by leaving those bytes and every pre-existing archive target unchanged; this plan authorizes neither cleanup nor deletion. A successful apply proves preservation by adding only the one approved archive target while retaining the source.

No design decision remains open for the archive-only slice, and no implementation work remains for that slice. The owner selected the direct-real-parent precondition after the raced-directory finding. Implementation details not selected by the design — including a public invocation shape and archive-target naming representation — remain minimal, deterministic, and reviewable without becoming a new schema or framework. RH-W-0052 remains `proposed`: uninstall, reinstall, deletion, cleanup, lifecycle advancement, issue/PR work, Git delivery, network access, commit, push, and publication remain separately undesigned or unauthorized.
