# RH-W-0025 Capability-Aware Integration Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: use `test-driven-development` for each behavior slice and `verification-before-completion` before delivery. Track every step with its checkbox. Work only on the approved RH-W-0025 work unit.

**Goal:** Generate a generic repository harness that coordinates repository authority, Engram continuity, and optional Gentle AI capabilities without copying owner state or creating another lifecycle.

**Architecture:** Keep the repository as durable truth. Existing generation functions emit bounded integrated and standalone contracts; enabled owners issue their own statuses, approvals, receipts, conflicts, and transitions. The implementation adds no service, dependency, persisted coordinator, or third state machine. `integrated` means Gentle AI plus Engram, `standalone` means neither, and partial availability is reported as component-scoped degradation under repository policy.

**Tech Stack:** Python 3.10+ standard library, `unittest`, generated Markdown contracts, JSON manifest, self-hosted `.opencode/` and `.claude/` projections.

**Canonical inputs:** [integration research](integration-research.md), [RH-W-0025](roadmap.md#rh-w-0025), and [standing decisions](decisions.md). The [Spanish plan](../es/repo-harness/rh-w-0025-implementation-plan.md) is a human mirror, not a second owner.

**Execution prerequisite:** satisfied. RH-W-0008 delivered the approved content-bound replacement and integration executor, and it was applied and verified on 2026-08-15. RH-W-0025 is `ready`.

**Detection contract:** detect Gentle AI through `gentle-ai version`, an advertised read-only surface that returns `gentle-ai <version>` with exit 0 and resolves through PATH, so it is independent of installation layout. Detection is three-state: `not_found`, `found_on_path`, and `verified`. Only `verified`, a validated exit code plus output shape, authorizes using the capability. `found_on_path` authorizes nothing, because executable presence is not proof of an authentic working CLI and `integration-research.md` forbids it as a detection basis.

**Blocker evidence:** Current `plan_changes()` classifies non-identical existing outputs as `existing`, and `write_outputs()` writes only create/append actions plus the narrow RH-W-0024 legacy-name migration. That migration cannot regenerate arbitrary existing consumer owners. Widening RH-W-0025 would duplicate RH-W-0008 and create a third replacement mechanism.

## Scope Guards

- [ ] Keep generated consumer wording generic. Never mention improving `repo-harness-builder` in generated output.
- [ ] Preserve repository ownership of durable truth, source, tests, policy, backlog, and approvals.
- [ ] Use Gentle AI and Engram together only in `integrated` mode. Use `standalone` only when neither exists.
- [ ] Treat one missing or failed integration as component-scoped degradation, not a new mode or aggregate authority verdict.
- [ ] Promote reusable operational knowledge only after repository verification and human approval.
- [ ] Do not add a third state machine, auto-enable review, fabricate approval, synthesize SDD, copy receipt/conflict/attempt stores, or retry until clean.
- [ ] Keep RH-W-0022 related and independent. Do not add a dependency unless implementation evidence proves its declared relationship map is required.
- [x] RH-W-0008 hard prerequisite satisfied: the approved content-bound replacement and integration executor was delivered, applied, and verified.
- [ ] Add no runtime dependency, daemon, scheduler, network client, provider mutation, installation probe, private-layout probe, or executable-presence probe. Invoking `gentle-ai version` is none of these: it is the tool's own advertised read-only surface, and a `found_on_path` result never authorizes a capability on its own.
- [ ] Preserve human-owned files and unrelated uncommitted work. Stage only paths named by the current bounded commit.

## Planned File Map

| Path | Responsibility |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Canonical RED/GREEN contract tests for mode, authority, promotion, degradation, output, and forbidden duplication |
| `skill/repo-harness-builder/scripts/harness_tool.py` | Canonical generator functions and existing mode selection |
| `skill/repo-harness-builder/SKILL.md` | Builder interview and capability-gate contract |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Read-only capability characterization and explicit unresolved-probe gate |
| `skill/repo-harness-builder/references/architecture.md` | Owner boundaries and mode architecture |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Approval, promotion, retry, and non-copying constraints |
| `skill/repo-harness-builder/references/validation.md` | Generated-harness validation and expected status behavior |
| `command/repo-harness-builder.md` | Canonical command projection if builder-facing routing text changes |
| `.opencode/skills/repo-harness-builder/**` | Self-hosted OpenCode projection of the complete canonical skill package |
| `.claude/skills/repo-harness-builder/**` | Self-hosted Claude projection of the complete canonical skill package |
| `.repo-harness/**`, generated runtime session projections, `.repo-harness/manifest.json`, `.repo-harness/check.py`, `REPO-HARNESS.md`, and generated durable docs | Excluded from direct RH-W-0025 edits; regenerate only in the explicit post-RH-W-0008 follow-up through its approved content-bound executor |
| `docs/repo-harness/{roadmap,current-state}.md`, `docs/es/repo-harness/{roadmap,current-state}.md`, `CHANGELOG.md` | Lifecycle reconciliation only after implementation and verification succeed |

The exact generated consumer file set must come from `TOOL.render(...)`. Do not hand-maintain a second list, delete existing owners to force creation, or edit generated owners independently. The RH-W-0008 executor prerequisite was completed, applied, and verified on 2026-08-15, so RH-W-0025 is `ready`.

## Task 1: Characterize Current Behavior With Failing Contract Tests

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py`
- Inspect only: `skill/repo-harness-builder/scripts/harness_tool.py`, especially `render()`, `render_with_skill_mapping()`, `session_start_skill()`, `session_stop_skill()`, `documentation_skill()`, `rule_outputs()`, and `docs_outputs()`

- [ ] **Step 1: Add one focused test class and helpers**

Add `CapabilityAwareIntegrationContractTests(unittest.TestCase)` beside the existing generated-contract tests. Define `combined(self, standalone: bool = False) -> str` by joining `TOOL.render("Consumer", [], standalone=standalone).values()`. Do not add production symbols in this step.

- [ ] **Step 2: Add the initial RED characterization matrix**

Add tests with these exact names and assertions:

```python
def test_integrated_contract_names_repository_engram_and_gentle_ai_owners(self):
    text = self.combined()
    for phrase in ("repository evidence", "Engram", "Gentle AI", "component-scoped"):
        self.assertIn(phrase, text)
    self.assertNotIn("improve repo-harness-builder", text)

def test_standalone_contract_uses_repository_evidence_without_integration_claims(self):
    text = self.combined(standalone=True)
    self.assertIn("repository evidence", text)
    self.assertIn("standalone", text.lower())
    self.assertNotIn("Engram persistence succeeded", text)
    self.assertNotIn("Gentle AI approved", text)

def test_generated_contract_forbids_copied_authority_and_unbounded_retry(self):
    text = self.combined()
    for phrase in ("third state machine", "fabricate approval", "synthetic SDD", "retry until clean"):
        self.assertIn(phrase, text)
```

- [ ] **Step 3: Run the focused tests and capture the expected RED evidence**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v
```

Expected: non-zero exit with assertion failures showing the current generated contracts do not yet contain the RH-W-0025 owner, standalone, degradation, and anti-duplication language. Import or syntax errors are not an acceptable RED result.

- [ ] **Step 4: Record the baseline without changing behavior**

In the implementation notes for the work unit, record which rendered paths caused each failure and whether `integrationMode` already reports `integrated` and `standalone`. Do not change lifecycle files or Engram status yet.

## Task 2: Lock the Pinned Gentle AI Evidence and Implement the Unresolved Probe Gate

**Files:**
- Modify: `skill/repo-harness-builder/references/audit-and-interview.md`
- Modify only if the verified gate affects builder routing: `skill/repo-harness-builder/SKILL.md`
- Test: `skill/repo-harness-builder/tests/test_harness_tool.py`

- [ ] **Step 1: Write the failing unresolved-gate test**

Add `test_unresolved_gentle_ai_probe_requires_explicit_human_confirmation` to `CapabilityAwareIntegrationContractTests`. Read `references/audit-and-interview.md` and assert all of these exact facts: pinned Gentle AI revision `4004a0825d03a3d123a3c4f67bf375cb16fd6d8c`, `explicit human confirmation`, `advertised read-only capability`, and `never infer from executable presence`. Also assert that the contract does not name an unverified command, flag, executable path, or private installation layout.

- [ ] **Step 2: Run the test and verify RED**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests.test_unresolved_gentle_ai_probe_requires_explicit_human_confirmation -v
```

Expected: FAIL because `audit-and-interview.md` does not yet define the pinned unresolved human-confirmation gate. Import, syntax, or fixture errors are not acceptable RED evidence.

- [ ] **Step 3: Add the minimal unresolved-gate contract**

Update `skill/repo-harness-builder/references/audit-and-interview.md` to identify `4004a0825d03a3d123a3c4f67bf375cb16fd6d8c` as the authoritative Gentle AI revision for RH-W-0025. State that this pinned evidence defines no stable generic read-only capability probe, so integrated generation requires explicit human confirmation and must never infer availability from executable presence, process state, or private layout. Do not add a guessed interface.

- [ ] **Step 4: Run the same test and verify GREEN**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests.test_unresolved_gentle_ai_probe_requires_explicit_human_confirmation -v
```

Expected: PASS. The contract names the pinned revision and the human gate without a private path, guessed CLI flag, synthetic status, or installation-layout assumption.

- [ ] **Step 5: Preserve the replacement gate for later evidence**

A later verified probe may replace human confirmation only through an explicit research update that deliberately adopts and pins a newer Gentle AI revision, replaces the prior pinned evidence, and adds a new failing test before changing the contract. Run that new test RED, implement only the verified advertised read-only surface, then run it GREEN. Ordinary implementation work must not silently reinterpret `current`, `installed`, or `available` as a probe.

## Task 3: Add Minimal Generic Generated Rules and Contracts

**Files:**
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py`
- Modify: `skill/repo-harness-builder/references/architecture.md`
- Modify: `skill/repo-harness-builder/references/preservation-and-safety.md`
- Test: `skill/repo-harness-builder/tests/test_harness_tool.py`

- [ ] **Step 1: Extend RED tests for the minimal contract set**

Add `test_generated_contract_assigns_each_fact_to_one_owner` and `test_generated_contract_stays_consumer_generic`. Assert repository, Engram, Gentle AI, and harness composition responsibilities from the authority matrix. Assert generated output excludes `repo-harness-builder` except generated provenance and fixed skill identifiers already owned by the product contract.

- [ ] **Step 2: Run the new tests to verify RED**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: FAIL only for missing contract language.

- [ ] **Step 3: Make the smallest generator change**

Update existing bodies of `rule_outputs()`, `docs_outputs()`, `session_start_skill()`, `session_stop_skill()`, and `documentation_skill()` only where each concern naturally belongs. Reuse the existing `standalone: bool`; do not introduce a third mode enum or persisted coordinator. Put detailed rationale in reference documents, not in always-loaded root pointers.

- [ ] **Step 4: Run the complete capability class**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: the owner and generic-language tests PASS; mode-branch tests may remain RED until Task 4.

## Task 4: Implement Integrated, Partial-Degradation, and Standalone Branches

**Files:**
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py`
- Modify: `skill/repo-harness-builder/SKILL.md`
- Modify: `skill/repo-harness-builder/references/audit-and-interview.md`
- Test: `skill/repo-harness-builder/tests/test_harness_tool.py`

- [ ] **Step 1: Add static rendered-contract tests for both generated modes**

Add `test_integrated_render_instructs_all_four_runtime_availability_outcomes` and render once with `TOOL.render("Consumer", [], standalone=False)`. Assert the integrated contract instructs runtime handling for these four outcomes: Gentle AI and Engram available uses both owners; Gentle AI available and Engram unavailable degrades continuity only; Gentle AI unavailable and Engram available leaves Gentle AI review and delivery unmanaged without inferred approval; neither available reports both integration components unavailable and follows repository policy. These are static instructions inside one integrated render, not four generator inputs.

Add `test_standalone_render_is_the_neither_available_generated_mode` and render once with `TOOL.render("Consumer", [], standalone=True)`. Assert manifest `integrationMode == "standalone"`, repository-only continuity, and no promise that Gentle AI or Engram ran. Assert the integrated render manifest remains `integrationMode == "integrated"`.

- [ ] **Step 2: Add authority-precedence tests**

Add `import inspect` with the standard-library imports in `skill/repo-harness-builder/tests/test_harness_tool.py`. Add `test_repository_evidence_wins_over_conflicting_or_stale_memory` and `test_disabled_gentle_ai_is_neither_approval_nor_denial`. Assert stale Engram claims require repository verification and disabled or absent Gentle AI never yields an approval, receipt, denial, or delivery decision. Add `test_render_accepts_no_runtime_capability_detection_state`: inspect `inspect.signature(TOOL.render)` and `inspect.signature(TOOL.render_with_skill_mapping)` and assert this work adds no Gentle AI availability, Engram availability, detection result, or third-mode parameter.

- [ ] **Step 3: Run the branch tests to verify RED**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: FAIL for missing generated branch instructions, not for fixture setup.

- [ ] **Step 4: Implement branch text in existing contracts**

In `session_start_skill()` and `session_stop_skill()`, describe the four runtime capability outcomes as bounded instructions emitted by integrated mode. In `SKILL.md` and `audit-and-interview.md`, make the pinned human-confirmation gate explicit. Keep `standalone: bool` as the only generator mode input and manifest `integrationMode` values exactly `integrated` and `standalone`. Do not add a third mode, runtime capability input, detection cache, manifest field, or persisted detection state. Partial availability exists only in component outcomes while an integrated generated harness runs.

- [ ] **Step 5: Run focused GREEN verification**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: all Task 1 through Task 4 tests PASS.

## Task 5: Implement Selective Knowledge Promotion

**Files:**
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py`
- Modify: `skill/repo-harness-builder/references/preservation-and-safety.md`
- Test: `skill/repo-harness-builder/tests/test_harness_tool.py`

- [ ] **Step 1: Add RED promotion tests**

Add these methods:

```python
def test_promotion_requires_verification_relevance_owner_and_human_approval(self):
    text = self.combined()
    for phrase in ("verified", "still relevant", "canonical owner", "human approval"):
        self.assertIn(phrase, text)

def test_incidental_and_changing_external_facts_are_not_durable_truth(self):
    text = self.combined()
    self.assertIn("incidental", text)
    self.assertIn("revalidated", text)
    self.assertIn("current as of", text)
```

- [ ] **Step 2: Run and verify RED**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: FAIL for absent promotion rules.

- [ ] **Step 3: Add the minimal generated promotion contract**

Update `documentation_skill()` and the generated memory/documentation rules emitted by `rule_outputs()` or `docs_outputs()`. Require the sequence: retrieve continuity input, label stale or changing facts, verify against repository evidence, identify the canonical owner, ask for human approval, then update that owner in the same work unit. Explicitly discard incidental noise. Do not create a promotion queue or memory-owned backlog.

- [ ] **Step 4: Run focused GREEN verification**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: all promotion tests PASS and no previous mode test regresses.

## Task 6: Add Component Degradation and Status Output

**Files:**
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py`
- Modify: `skill/repo-harness-builder/references/validation.md`
- Test: `skill/repo-harness-builder/tests/test_harness_tool.py`

- [ ] **Step 1: Add RED status-contract tests**

Add `test_component_outcomes_remain_distinct_and_offer_one_next_action`, `test_component_failure_never_claims_false_success`, and `test_malformed_owner_status_stops_only_that_optional_branch`. Assert generated guidance reports repository outcome, Gentle AI outcome, Engram outcome, degradation reason, and exactly one next action. Assert malformed owner output is relayed as exact evidence and never reconstructed.

- [ ] **Step 2: Add anti-store and retry assertions**

Add `test_generated_output_has_no_copied_owner_stores_or_endless_retries`. Reject generated instructions that create a receipt cache, conflict store, attempt ledger, combined approval state, or loop-until-clean behavior. Positive wording must require bounded attempts and stopping after repeated failure.

- [ ] **Step 3: Run and verify RED**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: FAIL for missing status and bounded-failure language.

- [ ] **Step 4: Implement status guidance without a public aggregate verdict**

Update existing generated session contracts and `validation.md`. Do not add a coordinator result schema unless a current owner contract requires one. Use prose fields in the generated handoff/status contract: repository outcome, component outcomes, degradation reasons, one next action. Preserve existing scoped `ready`, `paused`, `blocked`, `failed`, `completed`, and `degraded` terms.

- [ ] **Step 5: Run focused GREEN verification**

Run `python -B skill/repo-harness-builder/tests/test_harness_tool.py CapabilityAwareIntegrationContractTests -v`. Expected: all tests PASS.

## Task 7: Prove Contract Stability and Minimal Manifest Behavior

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py`
- Modify only if tests prove necessary: `skill/repo-harness-builder/scripts/harness_tool.py`

- [ ] **Step 1: Add render and manifest regression tests**

Add `test_manifest_keeps_only_integrated_and_standalone_modes`, `test_existing_runtime_locale_and_pointer_outputs_remain_stable`, and `test_render_is_deterministic_for_each_mode`. Assert no third `integrationMode`, unchanged selected runtimes and language fields, unchanged root-pointer ownership, no forbidden Unicode, and byte-identical repeated renders.

- [ ] **Step 2: Add generated-content invariants**

Assert integrated output does not copy owner receipts, conflict decisions, or attempt history; standalone output does not promise Engram or Gentle AI effects; both modes preserve generic repository-first language.

- [ ] **Step 3: Run the entire canonical suite**

Run:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Expected: exit 0, all tests pass, and only repository-supported skips remain. If unrelated existing failures appear, stop and report them without changing unrelated behavior.

## Task 8: Synchronize Checked-In Builder Package Projections by Exact Copy

**Files:**
- Copy from canonical: `skill/repo-harness-builder/SKILL.md`, `scripts/harness_tool.py`, `tests/test_harness_tool.py`, and the four changed files under `references/`
- Copy to: `.opencode/skills/repo-harness-builder/` at the same relative paths
- Copy to: `.claude/skills/repo-harness-builder/` at the same relative paths
- Do not use for package projection: `skill/repo-harness-builder/scripts/harness_tool.py render`, `.repo-harness/check.py`, or consumer `.repo-harness/` generation

- [ ] **Step 1: Prove package projection drift before copying**

Run the SHA-256 verification command from Step 3 before copying. Expected: it throws on at least one RH-W-0025 package path because canonical changed first. A zero exit means there is no package projection work to perform.

- [ ] **Step 2: Copy the exact canonical package files**

Run unchanged from the repository root:

```powershell
$paths = @("SKILL.md", "scripts/harness_tool.py", "tests/test_harness_tool.py", "references/architecture.md", "references/audit-and-interview.md", "references/preservation-and-safety.md", "references/validation.md"); foreach ($path in $paths) { Copy-Item -LiteralPath "skill/repo-harness-builder/$path" -Destination ".opencode/skills/repo-harness-builder/$path" -Force; Copy-Item -LiteralPath "skill/repo-harness-builder/$path" -Destination ".claude/skills/repo-harness-builder/$path" -Force }
```

Expected: exit 0. The command copies only the seven checked-in builder package files. It does not generate or update consumer `.repo-harness/` contracts, manifests, runtime session skills, commands, or documentation.

- [ ] **Step 3: Verify byte equivalence for every package projection**

Run:

```powershell
$paths = @("SKILL.md", "scripts/harness_tool.py", "tests/test_harness_tool.py", "references/architecture.md", "references/audit-and-interview.md", "references/preservation-and-safety.md", "references/validation.md"); foreach ($path in $paths) { if ((Get-FileHash -Algorithm SHA256 -LiteralPath "skill/repo-harness-builder/$path").Hash -ne (Get-FileHash -Algorithm SHA256 -LiteralPath ".opencode/skills/repo-harness-builder/$path").Hash) { throw "OpenCode projection mismatch: $path" }; if ((Get-FileHash -Algorithm SHA256 -LiteralPath "skill/repo-harness-builder/$path").Hash -ne (Get-FileHash -Algorithm SHA256 -LiteralPath ".claude/skills/repo-harness-builder/$path").Hash) { throw "Claude projection mismatch: $path" } }
```

Expected: exit 0 with no output. Never repair a mismatch by editing a projection; correct canonical source and repeat the exact copy.

- [ ] **Step 4: Verify all three package suites**

Run:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py"
```

Expected: each command exits 0 with matching test totals and supported skips.

- [ ] **Step 5: Record the dependency-bound consumer follow-up**

The checked-in builder package projection ends here. Do not regenerate `.repo-harness/**`, runtime session projections, manifest, checker, rulebook, or generated durable docs in RH-W-0025 with the current additive writer. After RH-W-0008 completes, open an explicit bounded follow-up that renders the exact consumer output, presents the RH-W-0008 content-bound replacement/integration preview, obtains its required human approval and digest token, applies that unchanged plan, and verifies the resulting generated owners. The follow-up must use the exact interface delivered by RH-W-0008 rather than pre-writing speculative commands here. Never substitute package `Copy-Item`, file deletion, or hand edits for that gate.

## Task 9: Reconcile Bilingual Documentation and Lifecycle After Implementation

**Files:**
- Modify only after the RH-W-0008-enabled regeneration follow-up: `docs/repo-harness/usage.md`, `docs/es/repo-harness/usage.md`, `docs/repo-harness/decisions.md`, and `docs/es/repo-harness/decisions.md`
- Modify: `docs/repo-harness/roadmap.md`
- Modify: `docs/es/repo-harness/roadmap.md`
- Modify: `docs/repo-harness/current-state.md`
- Modify: `docs/es/repo-harness/current-state.md`
- Modify: `CHANGELOG.md`
- Preserve: `docs/repo-harness/integration-research.md`, `docs/es/repo-harness/integration-research.md`

- [ ] **Step 1: Defer generated durable documentation to the approved follow-up**

Do not hand-edit generated durable owners to simulate regeneration. After RH-W-0008 supplies the approved executor, the explicit follow-up must preview and apply verified capability selection, integrated and standalone behavior, partial degradation, promotion approval, and one-next-action status output. Keep English as canonical owner and Spanish as a complete natural human mirror.

- [ ] **Step 2: Reconcile lifecycle owners in one unit**

Only after RH-W-0025 implementation passes and the dependency-bound regeneration follow-up verifies every generated owner, change RH-W-0025 from `ready` to `completed`, set remaining work to `none`, cite exact tests/checks, update current-state projections, and add one concise dated changelog entry. Do not change RH-W-0022 status or dependency.

- [ ] **Step 3: Reconcile Engram without transferring authority**

Update topic `work/RH-W-0025` with the verified implementation and regeneration outcome. Do not mark completion while RH-W-0008 is incomplete or before the follow-up passes. Engram mirrors the outcome; roadmap remains lifecycle owner.

- [ ] **Step 4: Run documentation and lifecycle tests**

Run the canonical full suite and `python .repo-harness/check.py`. Expected: tests pass; checker reports `current`, exit 0, and `writes: 0`.

## Task 10: Full Verification, Rollback, and Delivery Boundaries

**Files:**
- Verify: every path changed by Tasks 1 through 9
- Do not modify: GitHub, external systems, unrelated roadmap records, or unapproved consumer repositories

- [ ] **Step 1: Run full verification**

Run:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py"
python .repo-harness/check.py
git diff --check
```

Expected: three passing suites with matching totals, checker `current` with exit 0 and 0 writes, and empty `git diff --check` output.

- [ ] **Step 2: Verify links, parity, Unicode, and scope**

Run the repository's roadmap/document tests in the canonical suite. Then inspect every relative link in the changed English and Spanish files, compare heading structure and semantic requirements, and scan changed text for U+2013 and U+2014. Expected: no broken links, complete English/Spanish semantic parity, and zero forbidden characters.

- [ ] **Step 3: Inspect the final diff and changed-path boundary**

Run `git status --short` and `git diff --stat`, then inspect `git diff --` with the exact RH-W-0025 path set. Expected: no dependency files, secrets, `.codegraph/`, provider files, RH-W-0022 behavior, copied owner stores, or unrelated user changes.

- [ ] **Step 4: Apply rollback rules if verification fails**

Before any commit, restore only RH-W-0025 edits from the implementer's own patch or worktree snapshot. Never use `git reset --hard`, `git checkout --`, or any command that can erase pre-existing uncommitted work. If a generated apply partially fails, use its bounded rollback result and stop after the repeated failure with exact evidence.

- [ ] **Step 5: Prepare bounded work-unit commits, but do not split unsafe projections**

Preferred commit boundary after all checks pass:

```text
feat: add capability-aware integration contracts
```

This bounded implementation commit includes canonical generator behavior, canonical tests, and required `.opencode/` and `.claude/` builder-package projections. Existing self-hosted consumer contracts and generated durable docs are excluded until the separate RH-W-0008-enabled regeneration follow-up. That follow-up keeps its generated owners, verification, bilingual lifecycle reconciliation, and changelog together. Do not commit unless explicitly requested.

## Forecast

- Expected changed files for RH-W-0025 implementation before regeneration: 15 to 21 canonical, test, reference, and builder-package projection files.
- Expected changed lines before regeneration: 350 to 650. The post-RH-W-0008 follow-up must forecast its generated owner set from that executor's content-bound preview.
- Primary risks: no stable Gentle AI read-only probe, generated contract growth, accidental owner-state duplication, projection drift, and premature lifecycle completion.
- Rollback unit: the complete RH-W-0025 patch, with generated apply rollback used for any partial generated write.

## Plan Acceptance Checklist

- [ ] Task 1 proves current behavior with legitimate failing tests before production edits.
- [ ] The Gentle AI probe is a verified planning gate, never an invented interface.
- [ ] All four capability combinations are covered without a third mode.
- [ ] Repository precedence, selective promotion, human approval, and component degradation are tested.
- [ ] No fabricated approval, synthetic SDD, copied stores, aggregate owner verdict, or endless retry is introduced.
- [ ] Canonical source, tests, and seven-file builder-package projections form the RH-W-0025 implementation unit; generated consumer owners and lifecycle completion remain together in the separate RH-W-0008-enabled follow-up.
- [ ] RH-W-0022 remains related and independent.
