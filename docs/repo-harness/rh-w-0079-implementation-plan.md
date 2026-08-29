# RH-W-0079 Optional Capability Implementation Plan

Status: `ready`, complete and owner-approved on 2026-08-28 after a fresh final review returned PASS. The owner-approved [design](rh-w-0079-design.md) is authoritative. The owner's `ok comitear entonces ?` and selection of the normal contribution flow approved committing the reviewed plan and therefore approved the plan, but did not authorize implementation. The documentation, lifecycle, design, and planning contribution was delivered through closed issue [#129](https://github.com/LCubero/repo-harness-builder/issues/129) and merged PR [#130](https://github.com/LCubero/repo-harness-builder/pull/130) at `dedf74ec7fe3a2d3f2f7ead0fe6f98dc93eb0f55`. This plan authorizes no implementation, release, Git initialization, further commit, push, issue, or pull request.

Implementation progress: Slices 1 through 3 retain their documented delivery evidence. Slice 4 was delivered through issue #145, feature commit `9313d5768319602f0ca34ff3d97b195fb9405638`, and merged PR #146 at `94fe46933a04b82ae9ce5cbbd6dd60f59b04e9a4`. Slice 5 was delivered through issue #147 and PR #148, reconciling documentation, continuity, and release acceptance. The plan and RH-W-0079 are `completed`. No actual release, tag, publication, version bump, RH-W-0048 change, or RH-W-0052 change is claimed.

Corrective Slice 1 evidence: `python -B -m unittest skill.repo-harness-builder.tests.test_harness_tool.CapabilityAwareIntegrationContractTests.test_policy_required_is_a_contextual_selection_state skill.repo-harness-builder.tests.test_harness_tool.CapabilityAwareIntegrationContractTests.test_cli_default_preview_proceeds_for_every_capability_pair skill.repo-harness-builder.tests.test_harness_tool.CapabilityAwareIntegrationContractTests.test_cli_apply_still_fails_closed_for_every_unverified_pair` first ran 3 tests with 9 errors and 9 failures. Successful previews lacked `capabilityPreflight`, `policy-required` collapsed to `unavailable`, and unverified apply reached the plan-digest gate instead of the existing paired-capability refusal. After the minimum correction, the same 3 tests passed. This corrective RED is separate from the original paired-preflight RED: Slice 1 now composes only the read-only capability projection into successful preview output, while unverified apply retains its pre-Slice-3 failure behavior.

Fresh independent-review coverage added one table-driven successful non-standalone CLI preview test spanning both capability owners across effective `available`, `unavailable`, `partial`, `disabled`, and contextual `policy-required` states in single-capability and both-capability compositions. Its exact focused command passed immediately, characterizing production behavior that already satisfied the boundary; no production change was made to fabricate a RED. The test proves output sanitization, unchanged detector vocabulary, contextual `requiredFor`, and fail-closed apply eligibility in the emitted `capabilityPreflight` projection.

Spanish peer: [Plan de implementacion RH-W-0079](../es/repo-harness/rh-w-0079-implementation-plan.md).

## Execution Contract

- Execute test-first in independently reviewable slices. Preserve all existing bytes and uncommitted work.
- Use temporary repositories for state matrices. Never mutate consumer fixtures in place or use network/live providers.
- Change canonical source and tests only after RED. Synchronize `.opencode/` and `.claude/` projections through the existing renderer, never by hand.
- Serialize every shared canonical change, and keep each RED in the same review slice as its owning GREEN.
- Keep RH-W-0025 detectors and RH-W-0051 local-state ownership closed. Add no universal adapter, plugin framework, state machine, store, ledger, migration registry, or second authority route.
- Keep actual release authority separate from the completed Slice 5 documentation reconciliation.
- Treat work-unit commits below as planning boundaries only. Discover the selected repository's delivery authority at execution time. Never make GitHub, issues, pull requests, or `CONTRIBUTING.md` generic prerequisites.

## Design Traceability

| Design clauses | Planned owner and proof |
|---|---|
| Purpose and Capability Ownership Model, lines 5-43 | Slices 1 and 4 classify baseline, independent outcomes, and release inventory. |
| Detection, Use, Mutation, and Capability State Matrix, lines 45-64 | Slices 1-3 separate read-only detection, use, mutation gates, five outcomes, and apply drift. |
| Git States and Recommendation, lines 66-79 | Slice 2 proves useful non-Git operation, one recommendation, state-limited claims, and approval before `git init`. |
| Repository Policy and Provider Behavior, lines 81-87 | Slices 2 and 4 reject inferred GitHub, issue-first, pull-request, provider, and contribution policy. |
| Selection and Apply Contract, lines 89-101 | Slices 1 and 3 replace paired preflight, preserve standalone, migrate integrated output, and bind material capability configuration. |
| RH-W-0048 and RH-W-0052 Coordination, lines 103-108 | Slices 3 and 4 preserve separate recovery, reinstall, rollback, RED, and delivery ownership. |
| Approval Boundaries, lines 110-116 | Slices 2, 3, and 5 retain separate mutation, network, provider, Git, publication, and external-access gates. |
| Focused RED Tests, lines 118-138 | Slices 1-4 place all 15 required failures before their owning implementation or test fixture in the same slice. |
| Cross-platform and Agent Acceptance, lines 140-146 | Slice 4 records only available host, CI, runtime, and agent evidence. |
| Release and Rollback Acceptance, lines 148-159 | Slices 4-5 cover classification, projections, replay, rollback, documentation, and continuity. |
| Rejected Overengineering, lines 161-169 | Every slice includes negative assertions and a scope review. |

## Measured Baseline Before RED

Record commands, exit codes, JSON fields, SHA-256 values, focused test count, checker result, and initially dirty paths before source changes. Never copy a stale suite total into an acceptance gate.

1. In a temporary empty folder run canonical `render` without `--standalone` and with neither integration verified. Record current exit 2, `FAILED_CLOSED`, and paired-capability error from `main()` lines 3707-3725.
2. Repeat with Gentle AI only, Engram only, and both. Record that `select_integration_mode("integrated", ...)` currently permits only both `verified`. Preserve RH-W-0025 results `not_found`, `found_on_path`, and `verified`.
3. Preview and apply explicit `--standalone`. Record successful repository-only output and digest-bound apply.
4. Exercise current generated session-start in a temporary non-Git folder. Record useful behavior, withheld Git history/branch/diff/rollback/upstream/delivery guarantees, and current recommendation behavior.
5. Record `expected_authority_builder_config()` and authority schema accepting only `integrationMode: integrated|standalone`. This prices migration without assuming schema expansion.
6. Record parity for canonical and package copies of `harness_tool.py` and `test_harness_tool.py`, plus checker status and writes.

Likely baseline surfaces: `skill/repo-harness-builder/scripts/harness_tool.py`, `skill/repo-harness-builder/tests/test_harness_tool.py`, existing session templates, authority configuration, generation binding, and read-only package copies.

## Slice 1: Independent Capability Selection

**RED first:** add focused canonical cases for default preview with neither integration, each alone, and both; require useful repository-only preview in all cases and only capability-owned additions. Table-drive `available`, `unavailable`, `partial`, and `disabled` for Gentle AI and Engram, plus contextual `policy-required` blocking only one workflow. Treat `partial` and `disabled` as effective capability states, never as new RH-W-0025 detector classifications. Cover explicit standalone. Pin RH-W-0025 vocabulary and RH-W-0051 fields. Assert no manifest field, namespace, migration file, store, or adapter registry appears. Run the named tests before source changes and capture their intended failures against current paired preflight; keep these tests in this slice through GREEN.

**Implementation:** minimally replace `select_integration_mode()` and its `main()` call with an effective capability projection. Preserve detectors, `CAPABILITY_CLASSIFICATIONS`, and RH-W-0025 evidence. Keep explicit standalone and make no-capability default useful repository-only behavior. Add owned clauses through existing template builders, without adapters, services, plugins, or a state machine.

**Surface:** canonical script, existing session template functions, and canonical tests. **Verification:** named combinations, detector regression, standalone, and changed-path bound. **Rollback:** revert this slice coherently. **Work unit:** `feat(harness): compose optional capabilities independently`.

## Slice 2: Optional Git and Provider Boundaries

**RED first:** use temporary folders/repositories for executable absent, uninitialized, unborn, detached, dirty, clean, no-upstream, and diverged states. Require useful output, only supported claims, at most one concrete recommendation, no persistent warning state, and no automatic `git init`. Add Git-without-provider, non-GitHub provider, no remote, and missing `CONTRIBUTING.md` cases. Prove explicit current approval is required before initialization and initialization is never a render, repair, recovery, or apply side effect.

**Implementation:** reuse existing cross-platform subprocess and temporary-repository boundaries. Add only necessary Git detection. Keep provider recognition and contribution policy separate. Never persist recommendation counts.

**Surface:** existing Git/session-start helpers, generated contract templates, and canonical tests. **Verification:** focused matrix on current host, provider fakes, zero-write checks, and forbidden-assumption scan. **Coordination:** serialize any overlapping generated daily-contract changes with RH-W-0060 while RH-W-0060 retains branch-listing and committed-blob comparison ownership. **Rollback:** revert Git clauses independently while preserving Slice 1 coherence. **Work unit:** `feat(harness): bound optional git guidance`.

## Slice 3: Preview, Apply, and Migration Binding

**RED first:** test stable absence, stable combinations, and material capability drift. Stable absence succeeds; drift rejects only when approved plan bytes, target, scope, or authority are invalidated. Add explicit standalone and existing integrated-install migration fixtures using ordinary output replacement. Boundary tests must fail against current behavior because they exercise the new migration behavior, not because they add already-green negative assertions. Prove existing plan/generation digests still protect separate-process apply, evidence, target, and configuration. Assert no recovery, archive, uninstall, reinstall, deletion, or restoration behavior appears.

**Implementation:** first bind the effective configuration through `expected_authority_builder_config()`, existing context equality, plan bytes, and `generationSha256`. Change schema only if RED proves current binding insufficient. If a new field or authority route is required, stop for a revised owner decision. Replace owned generated output through normal render/apply.

**Coordination:** RH-W-0048 retains additive runtime recovery and tracked/local policy. RH-W-0052 retains archive, uninstall, reinstall, deletion approval, and rollback consequences.

**Surface:** authority configuration/schema only if proven, generation binding, apply preconditions, render configuration, and tests. **Verification:** authority/apply tests, migration fixtures, zero writes on refusal, closed-owner assertions. **Rollback:** restore selection, authority, apply, tests, and generated bytes as one versioned unit, never consumer state or Git. **Work unit:** `feat(harness): bind effective capabilities to apply`.

## Slice 4: Three-Way Classification, Renderer Synchronization, and Compatibility

**RED first:** derive a changed-behavior inventory from the actual diff and require each behavior exactly once as `builder-only scaffolding`, `repository-specific policy`, or `generic consumer contract`. Fail missing or duplicate classifications. Reject mandatory GitHub, issues, pull requests, checks, `CONTRIBUTING.md`, bilingual self-hosting, Gentle AI, Engram, CodeGraph, issue-first, branch, commit, push, or publication policy in generic output. Positively prove local self-hosted policy remains local.

**Implementation:** prefer a focused test fixture, not a runtime registry or consumer store. After the classification GREEN, synchronize the resulting canonical product work through the renderer in this same slice:

1. Preview the existing maintenance renderer and reject unrelated, human-owned, or stale projection changes.
2. Apply only the approved digest-bound plan. Never hand-edit package projections.
3. Verify SHA-256 parity and checker `current` with `writes: 0`.
4. Run focused canonical/projection tests required by changed ownership. Run the full suite only if focused evidence proves broader coupling.
5. Verify OpenCode, Claude-compatible, Pi-compatible, and generic file-following semantics on surfaces each supports. Report unavailable runtime/hardware evidence, never infer success.
6. Record current-host evidence and existing Windows/Linux/macOS CI evidence only where available.

**Surface:** canonical tests, at most one test-owned fixture, and renderer-owned package or daily projections only when planned. **Verification:** completeness test, forbidden-policy assertions, manual three-way diff review, projection parity, checker, focused compatibility, and available platform evidence. Do not invent a Pi package source. **Rollback:** remove the test inventory and render reverted canonical bytes without touching unknown files. **Work unit:** `chore(harness): classify and synchronize capability release`.

## Slice 5: Documentation, Continuity, and Release Acceptance

Execution status: completed through issue #147 and PR #148. The focused documentation contract failed before the standing decision changed and passes after the bilingual reconciliation. Repository documentation records Slice 4 delivery and Slice 5 release-acceptance reconciliation without claiming an actual release. Runtime and platform evidence unavailable to this work remains explicitly unknown.

1. Reconcile roadmap plan/record, current state, decisions only for a new durable decision, changelogs only for completed behavior, maps, and localized peers.
2. **RED first:** add a focused documentation assertion that fails while the standing paired-mode decision in `decisions.md` says `integrated` uses Gentle AI and Engram together and `standalone` applies when neither is available. **Documentation GREEN:** replace that contradictory wording with the approved independent optional-capability model while preserving RH-W-0025 detector semantics and operation-level degradation.
3. Update Engram topic `work/RH-W-0079` after repository evidence, preserving repository authority and operation-level outcomes.
4. Run focused lifecycle/documentation tests, relative links, checker, bilingual parity, `git diff --check`, and U+2013/U+2014 scan.
5. Replay temporary non-Git, local Git without provider, and self-hosted read-only/preview scenarios. Prove no initialization, provider inference, duplicate state, unsupported guarantee, or policy leakage.
6. Re-run final classification and confirm every design RED is GREEN. Name unavailable platform/agent proof.
7. Record rollback and discover delivery from the selected repository at execution time. Do not bake provider policy into product behavior.
8. Stop for separate owner release and delivery decisions.

**Rollback:** documentation/continuity may be corrected separately, but product rollback restores canonical selection, apply, tests, and projections consistently. **Work unit:** `docs(harness): reconcile optional capability delivery`.

## Focused Verification Matrix

| Evidence | Minimum method | Success |
|---|---|---|
| RED/GREEN | Named canonical `unittest` methods | Intended failure before source change, same test passes after minimal change. |
| Apply | Existing authority/apply classes plus drift cases | Stable absence succeeds; material drift refuses before writes. |
| Git | Temporary repositories and mocked provider boundaries | Claims match state; no network, automatic init, or persistence. |
| Migration | Temporary standalone and integrated installs | Authorized render/apply replaces owned bytes without duplicate state. |
| Projections | SHA-256 and checker | Required parity; checker `current`, writes 0. |
| Documentation | Focused lifecycle tests and relative links | Maps, roadmap, current state, changelogs, and peers agree. |
| Text | `git diff --check` and changed-file scan | No whitespace errors or U+2013/U+2014. |
| Classification | Final inventory | Every behavior exactly once, no self-hosted leakage. |

## Collision Analysis

| Owner | Classification | Collision and execution rule |
|---|---|---|
| RH-W-0048 | Sequencing dependency plus shared-file serialization | Owns checker-led additive recovery, tracked/local policy, and projection restoration. May share script, tests, checker templates, and projections. Serialize; RH-W-0079 implements no recovery. RH-W-0052 still waits for RH-W-0048 proof. |
| RH-W-0052 | Sequencing dependency | Owns archive, uninstall, reinstall, deletion, rollback, and non-Git recovery consequences. No blocker to selection slices, but later reinstall acceptance consumes the final contract. |
| RH-W-0046/RH-W-0018 | Semantic coordination | Joint consent design owns read-only network and remembered consent. Preserve gates; add no consent storage or suppression. |
| RH-W-0053 | None | Typed interrupted checkpoints and resumption remain separate. |
| RH-W-0060 | Semantic coordination plus shared-file serialization | Retains branch-listing and committed-blob comparison ownership. Slice 2 must serialize overlapping generated daily-contract changes and must not absorb either behavior. |
| RH-W-0069 | Semantic coordination | Owns backup and Engram continuity. Engram becomes optional here, but no backup schema is added. Serialize only overlapping session contracts. |
| RH-W-0068 | Semantic coordination plus shared-file serialization | Owns closure ordering and remaining close defects. Preserve gates, solve none of its defects, serialize session-stop changes. |
| RH-W-0071 | Shared-file serialization | May edit daily contracts, script/tests, and projections. Semantics differ; implement separately and regenerate from combined canonical state. |
| RH-W-0072 | Shared-file serialization | Owns product documentation drift and one pinning test. Do not absorb license, Pi, localState, or documentation inventory. |
| RH-W-0074 | Semantic coordination plus shared-file serialization | Owns warn-once compliance and outcome correction. Git recommendation does not override consent. Serialize session-stop. |
| RH-W-0075 | Semantic coordination | Owns necessity/justification for external access. Do not redefine external or request policy. |
| RH-W-0076 | Shared-file serialization | May change builder documentation and development controls. Preserve scope and avoid concurrent regeneration. |
| RH-W-0078 | Conditional sequencing plus shared-file serialization | May change generated topology, manifest/checker, and projections if multi-file remedy wins. RH-W-0079 adds no topology. If it lands first, use its renderer; otherwise do not touch its scope. |
| Completed RH-W-0025 | Semantic coordination | Reuse detector vocabulary/evidence unchanged, with no reopen. |
| Completed RH-W-0051 | None | Keep taxonomy unchanged, with no namespace, field, store, or checker behavior. |

No true blocker is proven for Slices 1-4. Shared-file owners require serialization. RH-W-0048 remains the sequencing owner before RH-W-0052 can advance. This durable relationship extends Plan Coherence Review item 11 without merging lifecycle units.

## Review Workload Forecast

| Slice | Estimated changed lines | Files/surfaces | Risk |
|---|---:|---:|---|
| 1 | 220-360 | 2 canonical files, later projections | High |
| 2 | 180-320 | 2 canonical files and generated text | High |
| 3 | 160-280 | 2 canonical files and fixtures | High |
| 4 | 80-230 authored, 300-700 generated | 1-2 test-owned surfaces and 4-10 projections | Medium |
| 5 | 100-220 | 6-10 bilingual surfaces plus Engram | Medium |

Forecast: 740-1,410 authored changed lines plus 300-700 renderer-owned lines across roughly 10-18 files. This exceeds 400 lines, so one combined review is NOT recommended. Use five slices, keep each focused RED with its owning GREEN in the same independently reviewable slice, and keep lifecycle documentation separate from product work. Counts remain estimates until baseline and renderer previews provide exact diffs.

## Independent Self-Review Gate

- Every design requirement maps to a task and proof; RED precedes implementation.
- No adapter framework, state machine, store, ledger, registry, daemon, or universal provider abstraction appears.
- Git is optional; GitHub/issues/PR/provider/`CONTRIBUTING.md` are not generic; no Git guarantee exists without Git; `git init` always requires approval.
- RH-W-0025 and RH-W-0051 remain closed; RH-W-0048 and RH-W-0052 remain separate.
- Counts are estimates, no stale suite total is a gate, collisions cover every plausible owner, and unavailable platform/agent evidence is explicit.
- Rollback never deletes state, initializes/removes Git, fabricates restoration, or releases mandatory paired preflight as the final direction.
- English and Spanish peers have the same headings, five slices, traceability, collision rows, workload ranges, and owner gate.

## Remaining Human Decisions

1. Any actual release, tag, package publication, version bump, provider action, or later delivery remains a separate owner decision.

## Owner Gate

`next_recommended`: no RH-W-0079 work remains. Any actual release, tag, package publication, version bump, push, or unrelated work requires separate authority.
