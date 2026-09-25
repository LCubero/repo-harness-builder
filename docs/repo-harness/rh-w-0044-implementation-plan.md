# RH-W-0044 Human-Owned Document Suggestion Implementation Plan

> **Current status: corrected plan accepted; lifecycle `proposed`.** Owner-authorized rulebook-core (`63220c047e4e3c589f274f36ac1462c478618763`), bilingual usage-overview (`f31c49446b770f4502e9f5c40dc217bd75757276`), Visibility (`018cc4aa829c410a8d1f89a6443387205f0ddc81`) and EN/ES Translation Sync (`27ca929c747d7612c03c29ce44eea1fea87de30a`) preview slices are independently verified, locally committed and natively acknowledged; the earlier documentation commit (`9c20eb4721903bac00dd40644a8fba573dba13e4`) is separate. R44-1 remains open with conservative ambiguity pending for other EN/ES families; R44-2/R44-3 are untouched, and R44-4's final whole-feature report is pending. No product push, PR, merge, release, or complete implementation is claimed.

Derived only from the owner-approved [design](rh-w-0044-design.md). Spanish peer: [rh-w-0044-implementation-plan.md](../es/repo-harness/rh-w-0044-implementation-plan.md).

## Proven Canonical Surfaces And Authority

`skill/repo-harness-builder/scripts/harness_tool.py::render_repository()` and `render_with_skill_mapping()` own rendered evidence. `human_owned()`, `semantic_sections()`, `missing_sections()`, and `check()` own current human-document classification. `main()` owns the existing `render` preview/report and approval flow. `plan_changes()`, `plan_digest()`, `validate_write_preconditions()`, `write_outputs()`, and `verify_outputs()` own the content-bound plan, pre-write recheck, atomic rollback, and post-write verification. `skill/repo-harness-builder/tests/test_harness_tool.py` owns behavior tests through a focused `HumanOwnedSuggestionTests` class plus existing `CheckTests`, `AdditiveWriteTests`, and `ApprovalUxTests` controls. Runtime package copies are projections, never independent owners.

A `suggestionId` may be the SHA-256 of a versioned canonical tuple containing locale, repository-relative path, and stable contract-unit ID. This binds semantic identity without a store. A separately displayed and supplied `suggestionEvidenceSha256` content-binds the contract version, `suggestionId`, locale, repository-relative path, stable contract-unit ID, current source SHA-256, action kind, and exact proposed UTF-8 bytes. Selection extends the existing `render` command with one suggestion ID, its evidence digest, and one choice. The builder must re-read and rerender, recompute the digest, and verify equality before constructing the existing plan, so either changed source or changed renderer/proposed bytes invalidates selection. This creates no command, service, state file, semantic model, dependency, or authority path.

## Task 1: Focused RED For Bounded Comparison

- [ ] Add `HumanOwnedSuggestionTests.test_preview_reports_one_deterministic_contract_unit_suggestion_with_zero_writes`, `test_project_truth_and_unstable_prose_never_receive_suggestions`, `test_project_specific_surrounding_prose_does_not_trigger_whole_section_replacement`, and `test_unavailable_git_tracked_file_evidence_fails_closed_with_zero_suggestions_and_writes`.
- [ ] Assert only the approved direct, readable, repository-contained Markdown set participates; generated, ignored, untracked, indirect, locale-incomplete, chronology, lifecycle, decision, plan, design, archive, and arbitrary product prose remain under existing classifications.
- [ ] Assert path/locale/contract-unit identity, classification, reason, evidence, source SHA-256, exact proposed UTF-8 bytes, bounded excerpts, choices, path/unit ordering, separately displayed `suggestionEvidenceSha256`, and `writes: 0`; the rendered whole file remains evidence only. If Git cannot provide tracked-file evidence, classify the set as unavailable/ambiguous, emit zero suggestions, perform zero writes, and never invent a clean tracked set.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_preview_reports_one_deterministic_contract_unit_suggestion_with_zero_writes HumanOwnedSuggestionTests.test_project_truth_and_unstable_prose_never_receive_suggestions HumanOwnedSuggestionTests.test_project_specific_surrounding_prose_does_not_trigger_whole_section_replacement HumanOwnedSuggestionTests.test_unavailable_git_tracked_file_evidence_fails_closed_with_zero_suggestions_and_writes -v`. Expected RED: the `HumanOwnedSuggestionTests` comparison/report and fail-closed Git-evidence contracts are absent while imports succeed.

## Task 2: Focused RED For Choice And Divergence Boundaries

- [ ] Add `HumanOwnedSuggestionTests.test_keep_is_ephemeral_and_each_path_exposes_only_one_suggestion`, `test_accepting_one_suggestion_requires_fresh_comparison_before_another`, and `test_divergence_marker_suppresses_only_one_valid_local_unit`.
- [ ] Add `test_invalid_or_protected_divergence_marker_reports_ambiguity_without_suppression` and `test_english_choice_never_accepts_or_mutates_the_spanish_peer`.
- [ ] Assert `accept`, `keep`, and `declare-intentional-divergence` are the only choices. `keep` creates no retained state. Declaration creation is itself one proposed suggestion. Unknown, duplicate, misplaced, stale, cross-locale, and non-suppressible declarations authorize nothing.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_keep_is_ephemeral_and_each_path_exposes_only_one_suggestion HumanOwnedSuggestionTests.test_accepting_one_suggestion_requires_fresh_comparison_before_another HumanOwnedSuggestionTests.test_divergence_marker_suppresses_only_one_valid_local_unit HumanOwnedSuggestionTests.test_invalid_or_protected_divergence_marker_reports_ambiguity_without_suppression HumanOwnedSuggestionTests.test_english_choice_never_accepts_or_mutates_the_spanish_peer -v`. Expected RED: choice, sequencing, and divergence handling are absent.

## Task 3: Focused RED For Existing Plan Authority

- [ ] Add `HumanOwnedSuggestionTests.test_accept_places_one_human_integration_action_in_the_existing_plan`, `test_changed_source_invalidates_selection_before_plan_construction`, `test_changed_rendered_proposal_invalidates_selection_before_plan_construction`, and `test_mixed_plan_failure_rolls_back_human_and_existing_actions`.
- [ ] Bind one accepted action to path, locale, contract unit, source digest, destination digest, exact bytes, action kind, suggestion ID, supplied `suggestionEvidenceSha256`, and existing plan digest. Before constructing the plan, re-read source, rerender exact proposed bytes, recompute the evidence digest, and reject a mismatch; retain the existing pre-mutation recheck.
- [ ] Prove no two suggestions enter one plan, no approval is reused, and no preview, ambiguity, invalid choice, failed validation, or stale evidence changes human bytes.
- [ ] Make rollback coverage construct one mixed transaction containing the accepted human action plus existing create, append, and replace actions; force a later action to fail; then prove byte-for-byte restoration of every pre-existing destination and removal of every newly created destination across the whole transaction.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests.test_accept_places_one_human_integration_action_in_the_existing_plan HumanOwnedSuggestionTests.test_changed_source_invalidates_selection_before_plan_construction HumanOwnedSuggestionTests.test_changed_rendered_proposal_invalidates_selection_before_plan_construction HumanOwnedSuggestionTests.test_mixed_plan_failure_rolls_back_human_and_existing_actions -v`. Expected RED: selection evidence is not checked before `plan_changes()`, and rollback lacks a mixed human/create/append/replace transaction.

## Task 4: Minimal GREEN, Projection, And Maintenance Workflow

- [ ] Implement deterministic comparison helpers beside current human-owned classification and extend only the existing `render` preview/report. Keep existing `check()` states and generated/directive/locale/legacy reports unchanged.
- [ ] Extend `plan_changes()`, `plan_digest()`, `validate_write_preconditions()`, `write_outputs()`, and `verify_outputs()` with exactly one accepted human integration action. Reuse current containment, approval, digest, atomic rollback, and validation. No direct write from the suggestion report.
- [ ] Repeat Tasks 1-3, then run `python -B skill/repo-harness-builder/tests/test_harness_tool.py HumanOwnedSuggestionTests CheckTests AdditiveWriteTests ApprovalUxTests -v`. Expected GREEN: all pass.
- [ ] Inspect and review the canonical `scripts/harness_tool.py` and `tests/test_harness_tool.py` changes first. Only after the canonical behavior is accepted, synchronize them to `.claude/`, `.opencode/`, and `.pi/`, and verify those runtime copies mechanically by SHA-256/parity rather than reviewing duplicate logic independently. Then use the established maintenance preview, separately authorized digest-bound apply, and empty second preview. No apply, Git, or delivery is authorized by this plan.
- [ ] After verified implementation and separate lifecycle authority, reconcile this plan, both designs, maps, roadmap, current state, changelogs, and Engram `work/RH-W-0044`.

## Rollback

Remove only the RH-W-0044 comparison helpers, existing-render selection inputs/report fields, one human integration action, focused tests, synchronized projections, and lifecycle statements. Preserve all pre-existing generated replacement, directive, localization, RH-W-0045, RH-W-0022, RH-W-0066, authority-context, digest, and rollback behavior. A failed mixed apply restores every pre-existing human, appended, and replaced file byte-for-byte and removes every file created by that transaction.

## Final Verification

- [ ] Run the three focused commands above against the canonical test path. Expected: all pass; inspect canonical script/tests first, then verify synchronized runtime script/test copies mechanically by SHA-256/parity.
- [ ] Run `python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`. Expected: exit 0.
- [ ] Verify SHA-256 parity for `scripts/harness_tool.py` and `tests/test_harness_tool.py` across canonical, Claude, OpenCode, and Pi package roots. Expected: no differences.
- [ ] Run established maintenance preview; after separate apply authorization run digest-bound apply and a second preview. Expected: only intended projections, `VALIDATED`, then no mutating actions.
- [ ] Run `python .repo-harness/check.py`. Expected: `current`, exit 0, zero writes.
- [ ] Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_repository_roadmap_records_have_valid_lifecycle_fields GeneratedCoreTests.test_current_state_projection_agrees_with_the_roadmap_owner GeneratedCoreTests.test_generated_markdown_links_resolve GeneratedCoreTests.test_checked_in_documentation_maps_use_resolving_markdown_links -v`. Expected: four pass.
- [ ] Run bilingual plan/design parity and relative-link checks, then `git diff --check`. Expected: no findings.

## Review Forecast

Authored product and test change: 260-380 lines. Mechanical package projections: 780-1,140 lines. Risk is high for human-byte authority and medium for comparison accuracy. Review as three work units: comparison/report, choice plus divergence, then content-bound action plus projections/lifecycle. If an authored slice approaches 400 lines, split before review; projected duplicates do not justify mixing authority concerns.

## Acceptance And Remaining Decisions

- [ ] Suggestions are deterministic, localized, bounded to approved contract units, and preview-only until one is accepted.
- [ ] `accept`, `keep`, and declaration behavior preserve the one-at-a-time authority boundary and fresh evidence.
- [ ] Existing plan digest, containment, approval, rollback, and post-write validation govern the only write path.
- [ ] No bulk acceptance, semantic equivalence, persistence, service, dependency, new command, or new authority owner appears.

Remaining decision: the corrected plan was accepted, and the owner later authorized four partial preview slices only. Completing R44-1 and implementing R44-2/R44-3 require separate owner authorization; the final R44-4 whole-feature report remains pending. Apply and product delivery remain separate decisions. No technical planning decision remains open.
