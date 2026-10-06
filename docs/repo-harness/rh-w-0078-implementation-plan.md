# RH-W-0078 Owner-Approved Implementation Plan

**Status: implementation authorized; `active`, `P2`.** The plan was owner-approved on 2026-10-05 for documentation recording only. On 2026-10-04 the owner separately authorized preparing this bounded bilingual proposal ("si dale") after approving the [design](rh-w-0078-design.md). **Verified authority:** in the current authorization, the owner explicitly authorized RH-W-0078 implementation and issue #266 was confirmed. The [roadmap](roadmap.md#rh-w-0078) owns lifecycle; the approved design owns exact gates, measurements and acceptance. Historical plan approval remains historical and is not runtime evidence.

## Authority and Measurement Gates

- The owner explicitly authorized RH-W-0078 implementation; the former current-runtime baseline/feasibility-before-start gate is removed. An earlier owner authorization also covered scoped local staging and commit; no such action occurred. That authority is retained as historical intent, not renewed by or executable under the current documentation-only continuation, which excludes Git. External tool changes, runtime experiments, push, PR, merge, release, publication, and automatic stop are not authorized.
- Preserve [Plan 27](roadmap.md#plan-27-rh-w-0078-generated-skill-scope): measure effective loaded context and reference reachability across Claude Code, OpenCode and Pi as an effectiveness/acceptance gate, not as a prerequisite to starting implementation. Static counts do not establish runtime savings.
- Any runtime observation/experiment remains subject to separate authority. If unavailable, report `unmeasured`; never treat static counts as runtime evidence or claim savings without comparable measured results.
- Candidate comparison occurs **after candidate code exists** and before effectiveness/savings acceptance; future-candidate proof is not required before a candidate can exist. Preserve the design's actual-runtime reduction gate for every claimed common path/runtime/configuration.
- The 2026-10-06 decision initially approved the explicit inspect/check branch as design only. The user's subsequent “si dale” authorized I1 implementation: read-only active-builder dispatch in the canonical builder skill, focused canonical contract tests, and minimum bilingual authority/status documentation. Use the existing installed checker plus authenticated declared configuration only; infer no runtime/profiles/language/capabilities; stop on missing or untrusted evidence. Do not change CLI/runtime or installation/update/correction flows, synchronize projections, repair/apply, use Git/network, run runtime experiments, commit, or deliver. Later exact-reference ownership/projection work remains a separate stage. The current documentation continuation authorizes no implementation or projection synchronization.
- **Unknown:** numerical runtime baselines, candidate results and effective savings. The earlier focused failure, 623-test T1–T5 receipt, and older 300-second timeout are historical and do not describe the latest correction. See [Latest verification evidence](#latest-verification-evidence) for current technical receipts, unresolved native approval, and historical evidence.

## Latest Verification Evidence

Latest independent technical verification observed:

- Latest independent mux verification: 625 tests in 334.094 seconds, `OK (skipped=11)`: 614 non-skipped, zero failures/errors. Skip identities and child counts were not available.
- Checker remained `current` with 0 writes; global diff check clean. Six runtime tool/test pairs and four startup-own-render/UTF-8-LF checks passed. Five known anchors and aggregate file/index/status hash were unchanged during verification.
- The ten-path maintenance receipt returned `VALIDATED`. A later three-test-copy receipt also returned `VALIDATED`, but its returned digest extraction and pre-apply hash were not retained; complete pre-apply preservation cannot be claimed or reconstructed.
- The safety correction adds backup guards before read and reuses the existing checkpoint oracle scoped to `NativeContinuityRecovery` in `.index`; controls were not weakened. The earlier 623- and 624-test suite receipts remain historical.
- Native approval remains unresolved. RH-W-0078 remains `active`, `P2`; no feature closure or delivery is claimed. Runtime effectiveness and savings remain unknown.

## Future Owners and Ordering

Locators below describe proposed future surfaces, not present write authority. `H` = `skill/repo-harness-builder/scripts/harness_tool.py`; `T` = `skill/repo-harness-builder/tests/test_harness_tool.py`. Line locators refer to baseline `d30f8b442288eeb180257a46ccc4bb1b64688f5c`, with this documentation delta separate. Existing test classes/methods named below were inspected locally; negative cases are **proposed tests**, not existing methods or receipts.

### 1. Pin Semantics and Baseline Procedure

Owners: existing `H` counting/containment helpers and `T` `GeneratedCoreTests`; use the approved design's stdlib/in-memory procedure, no new measurement script or store.

- Pin runtime, locale, integration, capability outcomes, repository shape, scenario clauses, fixed non-sensitive fixture bytes and SHA256. Source hashes identify inputs, never authority. Unavailable configurations remain `unmeasured`.
- Distinguish body-only frontmatter exclusion from whole-file closure; safe unique reachability inventory from ordered gated read events; unique volume from repeated reads. Unconditional relocation earns no savings.
- Keep unresolved edges, ambiguous gates/order and unbounded cycles `unmeasured`; do not silently omit, break cycles or read private state. Use existing containment guards, retaining cooperative race limits.
- Proposed RED: malformed frontmatter, escaping/unresolved paths, repeated events and unconditional relocation falsely counted as savings; GREEN only after minimum applicable changes, then alternate cases/refactor.

### 2. Evolve Exact Generated Reference Ownership Atomically

Owners: `H` schemas (435, 2760, 2927), renderer (3861), checker (2975), `generated_skill_paths()` (4575), installed ownership (4018), replacement (4274), legacy/migration (4594–4770), apply (4871), LF derivation (1197–1212).

- Derive the finite exact reference files/projections enumerated by the approved design from validated selected profiles and canonical names. Manifest declaration, checker schema/exact equality/provenance/byte coverage, replacement/upgrade, `.repo-harness/manifest.json`, `.repo-harness/check.py`, and `.gitattributes` must evolve coherently. Never trust a directory, link or prefix; legacy manifests confer no new ownership by proximity.
- Within this unit inspect the migration regex at 4723–4724 (currently excludes Pi and references) and `canonicalProjections` replacement shortcut at 4293–4374 (projection membership bypasses provenance). Reconcile them with authenticated exact ownership and human collision protection, not unrelated cleanup.
- Existing tests: `GeneratedCheckScriptTests`, `ApprovedGeneratedOutputReplacementTests`, `GeneratedOutputMigrationTests`, `CrossVersionUpdateTests`, `RepositoryLineEndingContractTests`.
- Proposed RED: missing/extra/duplicate/escaping declarations, wrong profiles/provenance, stale digest, human reference collision and rollback restoration. Preserve manifest/checker atomicity and byte-bound consent through GREEN and triangulation.

### 3. Thin Start/Stop Entries Behind Required Gates

Depends on unit 2. Owners: `H` `session_start_skill()` (1446), `session_stop_skill()` (1654), generated canonical `.repo-harness/skills/repo-harness-session-{start,stop}/SKILL.md` and only selected runtime peers/references.

- Use exactly start `reconciliation.md`, `checkpoint.md`, `synchronization.md`; stop `checkpoint.md`, `git-commit.md`, `git-transport.md`, under each skill's `references/`. Runtime mapping and predicates remain the design's exact table, not new branches.
- Keep activation, safety, gate predicates/order, mandatory-before-action loads and missing/unsafe/unowned/unreadable-reference stops in each entry. Keep the four deeper-owner triggers; unconditional exact-path checkpoint discovery after native recovery, absence `not_found`, no fifth trigger or reused consent.
- Keep stop verification → applicable documentation → native continuity → eligibility unconditional, optional Engram outcome independent, and commit-only distinct from push authority.
- `GeneratedCoreTests` proposed RED: gate/order, missing references and every preservation obligation. Retarget inline assertions to entry plus required gated closure; do not delete protections to make compression pass. Generated documentation remains an unchanged control.

### 4. Compress Builder Body and Clarify Guide Ceiling

Owners: `skill/repo-harness-builder/SKILL.md`, its existing `references/`, proposed `references/authority-correction.md`, bundled `references/gentle-ai-style-guide.md` (line 5), `T` budget assertions (5428–5494), and exact selected builder package projections.

- Keep body proxy ≤1000 for builder/every generated skill and rulebook assertion `<700` unchanged; never restore runtime render/check budget refusal or raise ceilings. Reuse development assertions.
- Preserve `GeneratedCoreTests.test_documented_render_command_parses_and_keeps_every_interview_answer`, `ApprovalUxTests`, independent interview flags, strict builder versus conversational maintenance consent, and stateless target/plan/generation authority bindings.
- I1 implements only the separately authorized explicit inspect/check branch. Preserve the existing install/update/correction gates; confer no apply/repair/synchronization/Git authority; and do not run runtime experiments. The [approved design](rh-w-0078-design.md) defines evidence limits and fail-stop behavior. Runtime savings remain unknown.
- Proposed RED: body/guide scope and lost action-point/authority obligations; test entry plus mandatory closure. Safe source units can be sequenced without asserting runtime success; direct CLI check stays a distinct scenario.

### 5. Verify Preservation and Effectiveness Before Acceptance

Depends on units 1–4 and the separate authorities above. For applicable deterministic behavior: smallest focused RED → minimum GREEN → negative/alternate triangulation → green-only refactor. Stop on unsafe references, ownership/schema mismatch, human collision, changed digest, lost obligation, ambiguous scenario or candidate drift; preserve evidence and request the owning decision.

Implementation validation commands (run for local T1–T5; observed suite/checker results are recorded in [current state](current-state.md)). Candidate runtime-effectiveness comparison and separately authorized experiments remain pending:

```text
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
python .repo-harness/check.py
```

- Require checker `current`, exit 0, writes 0; selected-runtime canonical/projection byte parity and exact LF coverage; unchanged human bytes. Content-bound maintenance preview/apply requires future explicit authority; second preview must offer zero actions.
- Compare identical pinned static scenarios, entry/whole-file closure, ordered gates and repeated reads; require the approved common-path reductions, report alternate-path increases, never infer savings from body size.
- Under separately approved experiments, measure actual baseline/candidate consumption for resume-only start, local-handoff stop and resolved active builder inspection in every claimed runtime/configuration. Pin versions, fresh/cache state and prompt/tool traces; unavailable/incomparable observations are `unmeasured`, no decrease fails the claim. No reload, live-compliance or portability guarantee is invented.

## Review Load and Next Gate

**Supported inference:** expected cumulative documentation load is roughly 506–576 changed lines including the pre-existing 336-line design candidate; the final handoff reports actual counts including untracked files. Recommend two conceptual review slices: approved design/preparation (336 lines), then this bilingual proposal plus indispensable metadata. This is a recommendation only, not a selected Git/PR strategy or delivery authority. Keep unit 2's ownership transaction coherent even if its eventual change exceeds the advisory 400-line target; phase units by behavior and owner paths, not unsupported line forecasts.

**Verified fact:** the latest technical receipts are summarized in [Latest Verification Evidence](#latest-verification-evidence), and final independent bilingual reconciliation B4 (`mux4fck6-r-csef`) passed. RH-W-0078 remains `active`, `P2`; native approval remains unresolved. The latest native INSPECT returned `intended_untracked_selection_required` with zero lineages; no START or retry followed. The accepted 32-file selection and the historical 75-path lens-budget failure are historical, and no supported smaller candidate is established. Runtime effectiveness, cache results and registered names remain unknown; any runtime experiment requires separate authority. Earlier apply-3 digest-extraction/pre-apply-hash gaps remain unreconstructed. Earlier scoped staging/commit authority is historical, was not exercised, and is not renewed or executable under this documentation-only continuation. This update claims no staging, commit, push, PR, merge, release, publication, runtime experiment, savings result, native approval or formal delivery.
