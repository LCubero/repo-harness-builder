# RH-W-0025 Capability-Aware Integration Implementation Plan

> **Status: RH-W-0025 is `completed`.** The [roadmap record](roadmap.md#rh-w-0025) owns its outcome and delivery evidence; the plan below is retained as historical execution evidence, not as instructions to follow.

> **For agentic workers:** use `test-driven-development` for every behavior slice and `verification-before-completion` before delivery. Work only on approved unit RH-W-0025.

**Goal:** Generate a generic repository harness that coordinates repository authority, Engram continuity, and optional Gentle AI capabilities without copying owner state or creating another lifecycle.

**Architecture:** The repository remains durable truth. `integrated` generation selects Gentle AI plus Engram; `standalone` selects neither. Installation-time builder preflight validates both capabilities before integrated generation is selected or applied. After installation, loss or failure degrades only the affected component unless repository policy makes it mandatory. Partial availability is not a third mode or an aggregate authority verdict.

**Tech stack:** Python 3.10+ standard library, `unittest`, generated Markdown, JSON manifest, and checked-in `.opencode/` and `.claude/` package projections.

**Canonical inputs:** [integration research](integration-research.md), [RH-W-0025](roadmap.md#rh-w-0025), and [standing decisions](decisions.md). The [Spanish plan](../es/repo-harness/rh-w-0025-implementation-plan.md) is a complete human mirror.

**Execution result:** completed. RH-W-0008 delivered the content-bound replacement executor and applied the verified RH-W-0025 consumer plan on 2026-08-16.

## Detection Contract

Run the advertised read-only command `gentle-ai version` through PATH with a bounded timeout.

| Classification | Evidence | Authorization |
|---|---|---|
| `not_found` | Process creation reports that the executable cannot be found. | Gentle AI is unavailable. |
| `found_on_path` | The process starts but times out, exits nonzero, or returns output outside the required shape. | Gentle AI is not verified and cannot be used. Preserve exact failure evidence. |
| `verified` | The process exits 0 within the timeout and normalized stdout matches `gentle-ai <version>` with a non-empty version token. | Gentle AI may be selected, subject to Engram validation and repository policy. |

Executable presence alone never produces `verified`. Human confirmation is not the normal detection path and cannot replace process evidence. Do not inspect private installation layouts, process tables, or guessed files.

Engram detection must use its supported local capability surface and preserve its exact evidence. Define a concrete adapter only after current source and tests identify the existing boundary. Do not infer Engram from unrelated files or memory content.

Integrated selection and apply require Gentle AI `verified` and Engram verified. Any other combination refuses integrated generation and may offer `standalone` only through the existing approved selection boundary.

After integrated installation, later Gentle AI loss degrades Gentle AI review and delivery only. Later Engram loss degrades continuity only. Repository policy may make either capability mandatory and block its owning workflow.

## Scope Guards

- [ ] Keep generated consumer wording generic and repository-first.
- [ ] Preserve repository ownership of durable truth, source, tests, policy, backlog, and approvals.
- [ ] Keep only `integrated` and `standalone` generation modes.
- [ ] Validate both capabilities before selecting or applying integrated generation.
- [ ] Treat post-installation loss as component degradation unless repository policy makes it mandatory.
- [ ] Promote reusable knowledge only after repository verification, canonical-owner selection, and human approval.
- [ ] Add no service, daemon, scheduler, network client, provider mutation, private-layout probe, copied owner store, synthetic SDD, fabricated approval, or unbounded retry.
- [ ] Keep RH-W-0022 related and independent.
- [ ] Use the completed RH-W-0008 replacement executor for authenticated existing generated owners. Do not restore an additive-writer limitation or invent a second replacement path.
- [ ] Preserve human-owned files and unrelated work.

## Planned File Map

| Path | Responsibility |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Canonical RED/GREEN behavior and generated-contract tests |
| `skill/repo-harness-builder/scripts/harness_tool.py` | Capability execution, classification, selection, and generated contracts |
| `skill/repo-harness-builder/SKILL.md` | Builder preflight and selection contract |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Read-only detection and evidence rules |
| `skill/repo-harness-builder/references/architecture.md` | Owner boundaries, preflight, and runtime degradation |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Approval, promotion, retry, and non-copying constraints |
| `skill/repo-harness-builder/references/validation.md` | Capability and generated-harness validation |
| `.opencode/skills/repo-harness-builder/**`, `.claude/skills/repo-harness-builder/**` | Exact package projections of changed canonical files |
| Consumer owners returned by `TOOL.render(...)` | Preview and apply through the existing RH-W-0008 executor |
| Roadmap, current state, Spanish peers, and changelog | Lifecycle reconciliation only after implementation and verified consumer apply |

Production names below are not prescribed unless marked as **design targets**. Reuse existing boundaries when they fit.

## Task 1: Characterize Existing Contracts

- [ ] Add one focused capability-aware test class beside existing generated-contract tests.
- [ ] Prove missing or incomplete repository, Engram, Gentle AI, mode, promotion, degradation, and anti-duplication wording.
- [ ] Confirm existing `integrationMode` behavior without adding test-only capability inputs to `render()`.
- [ ] Run the focused class. Only assertion failures are valid RED evidence.

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v
```

## Task 2: Implement Detection and Classification

This is one focused RED/GREEN behavioral slice. Inject or substitute process execution, time, and Engram evidence. Tests must not depend on the machine, PATH contents, real waits, network access, or actual installations.

- [ ] RED: execution uses exactly `gentle-ai version`, a bounded timeout, captured stdout and stderr, and no shell expansion.
- [ ] RED: executable missing classifies exactly as `not_found`.
- [ ] RED: timeout classifies exactly as `found_on_path` and preserves timeout evidence.
- [ ] RED: nonzero exit classifies exactly as `found_on_path` and preserves exit code and bounded output.
- [ ] RED: exit 0 with empty, malformed, multiline, or wrong-prefix stdout classifies exactly as `found_on_path`.
- [ ] RED: exit 0 with normalized `gentle-ai <version>` and a non-empty version token classifies exactly as `verified`.
- [ ] RED: parameterized coverage proves the result set is exactly `not_found`, `found_on_path`, and `verified`, with no boolean shortcut or fourth state.
- [ ] RED: Engram detection covers verified, missing, failed, and malformed evidence through its supported local boundary.
- [ ] RED: combined classification selects `integrated` only when both capabilities verify; otherwise it refuses integrated selection or apply and preserves both component outcomes.
- [ ] RED: fakes complete immediately, output fixtures are bounded, and timeout tests simulate the timeout exception instead of waiting.
- [ ] GREEN: implement the smallest behavior at existing process and selection boundaries.
- [ ] GREEN: any proposed seam name is a **design target**, not an approved API. Choose it only after inspecting current code and keep it private unless an existing public contract requires otherwise.
- [ ] GREEN: rerun the focused slice and confirm deterministic results.

## Task 3: Add Generic Owner Contracts

- [ ] Add RED tests assigning durable truth to the repository, continuity to Engram, and optional review and delivery evidence to Gentle AI.
- [ ] Reject product-specific consumer wording, copied owner stores, fabricated approval, synthetic SDD, and retry-until-clean behavior.
- [ ] Update only existing generator and reference bodies where each concern belongs.
- [ ] Keep rationale in references rather than always-loaded root pointers.
- [ ] Run the focused class GREEN.

## Task 4: Separate Preflight From Runtime Degradation

- [ ] Prove integrated selection and apply require both capabilities during builder preflight.
- [ ] Cover all post-installation runtime combinations: both available, only Gentle AI, only Engram, and neither.
- [ ] Prove later loss affects only its component and never fabricates another owner's result.
- [ ] Prove repository policy can elevate either component from optional degradation to a blocking requirement.
- [ ] Keep manifest modes exactly `integrated` and `standalone`; do not persist runtime detection or add a partial mode.
- [ ] Run the focused class GREEN.

## Task 5: Implement Selective Promotion

- [ ] Require verified relevance, a canonical repository owner, and human approval before promotion.
- [ ] Date and revalidate changing external facts before reuse.
- [ ] Reject incidental noise and any memory-owned backlog or promotion queue.
- [ ] Implement minimal generated documentation and memory contract changes, then run focused tests GREEN.

## Task 6: Add Component Status and Bounded Failure Guidance

- [ ] Test distinct repository, Gentle AI, and Engram outcomes, exact degradation evidence, and one next action.
- [ ] Prove malformed owner output stops only that optional branch and is never reconstructed as success.
- [ ] Reject receipt caches, conflict stores, attempt ledgers, combined approval state, and endless retries.
- [ ] Implement prose guidance without a new aggregate schema unless a current owner contract requires one.

## Task 7: Prove Stability

- [ ] Assert only `integrated` and `standalone` are emitted.
- [ ] Preserve runtime, locale, pointer, manifest, and ownership behavior.
- [ ] Assert byte-identical repeated renders and no U+2013 or U+2014 in changed text.
- [ ] Run the complete canonical suite and stop on unrelated failures.

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

## Task 8: Synchronize Package Projections

- [ ] Determine changed canonical package paths from the final diff, not a speculative fixed list.
- [ ] Prove at least one corresponding projection differs before copying.
- [ ] Copy each changed canonical package file to matching `.opencode/` and `.claude/` paths.
- [ ] Verify SHA-256 equality and run all three package suites with matching totals and supported skips.

## Task 9: Apply Consumer Replacements

RH-W-0008 is complete and applied. Its replacement executor is the only mutation path for authenticated existing generated owners.

- [ ] Render exact consumer output with every installed dimension explicit.
- [ ] Generate a fresh content-bound preview from current repository bytes.
- [ ] Review exact create, append, replace, migrate, and blocked actions. Do not reuse historical path lists or digests.
- [ ] Obtain approval required by the active builder boundary and apply the unchanged digest-bound plan through the existing executor.
- [ ] Verify a second identical preview has zero actions and both checkers report `current` with 0 writes.
- [ ] Never substitute hand edits, deletion, package copy, or a new additive writer.

## Task 10: Reconcile and Deliver

- [ ] Only after implementation and consumer apply verify, change RH-W-0025 from `ready` to `completed`, set remaining work to `none`, and reconcile current state, Spanish peers, changelog, and Engram.
- [ ] Do not change RH-W-0022 status or dependency.
- [ ] Validate links, bilingual semantic parity, forbidden characters, and exact changed paths.
- [ ] Run all package suites, `python .repo-harness/check.py`, and `git diff --check`.
- [ ] Inspect status, stat, and focused diff before any separately approved Git action.
- [ ] On failure, restore only this unit through its patch or executor rollback. Never use destructive Git recovery.

## Forecast

- Scope: canonical source, focused tests, references, exact package projections, and generated owners selected by a fresh RH-W-0008 preview.
- Risks: weak output validation, nondeterministic process tests, conflating installation preflight with runtime degradation, copied authority, and premature lifecycle completion.
- Rollback: complete RH-W-0025 patch plus bounded executor rollback for generated writes.

## Acceptance Checklist

- [ ] `gentle-ai version` is the only Gentle AI probe; exit 0 and `gentle-ai <version>` are required for `verified`.
- [ ] Detection has exactly `not_found`, `found_on_path`, and `verified`.
- [ ] Task 2 covers execution, missing executable, timeout, nonzero exit, malformed output, Engram, and combined classification with deterministic bounded tests.
- [ ] Integrated selection and apply validate both capabilities at installation time.
- [ ] Post-installation loss degrades only its component unless repository policy makes it mandatory.
- [ ] RH-W-0008's completed replacement executor handles existing generated owners.
- [ ] Repository precedence, selective promotion, human approval, and all four runtime combinations are tested without a third mode.
- [ ] English and Spanish plans remain complete semantic mirrors.
- [ ] RH-W-0022 remains related and independent.
