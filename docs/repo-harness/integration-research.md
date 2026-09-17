# Gentle AI and Engram Integration Research

## Question and Scope

How should a generated repository harness use Gentle AI and Engram organically without creating another authority or lifecycle? This document records durable research guidance only. It does not implement behavior, promote lifecycle work, or change product state.

Generated consumer harnesses remain generic and never mention improving `repo-harness-builder`. This self-hosted repository applies the same generic rule to its own product source.

## Pinned Revisions

| Source | Revision |
|---|---|
| `repo-harness-builder` | [`58f41a790556d9dbb2c9e3094459b85c206b8fbd`](https://github.com/LCubero/repo-harness-builder/tree/58f41a790556d9dbb2c9e3094459b85c206b8fbd) |
| Gentle AI | [`4004a0825d03a3d123a3c4f67bf375cb16fd6d8c`](https://github.com/Gentleman-Programming/gentle-ai/tree/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c) |
| Engram | [`1dafc0f63051b2214100f7bd801357e4aab61c26`](https://github.com/Gentleman-Programming/engram/tree/1dafc0f63051b2214100f7bd801357e4aab61c26) |

## Current Refresh (2026-09-17)

**Verified fact:** this refresh reviewed the selected local `gentle-shell`, Gentle AI, and Engram reference snapshots at these exact local Git commits: `gentle-shell` [`2b579c80824e83442b8ae9f7bfad629a52f9f711`](https://github.com/Gentleman-Programming/gentle-pi/tree/2b579c80824e83442b8ae9f7bfad629a52f9f711), Gentle AI [`eae8fadd5ece2622cb072c836fa49cef520afc8e`](https://github.com/Gentleman-Programming/gentle-ai/tree/eae8fadd5ece2622cb072c836fa49cef520afc8e), and Engram [`f6b22f89d024fb6aaa207606d893f47b3f4e23a9`](https://github.com/Gentleman-Programming/engram/tree/f6b22f89d024fb6aaa207606d893f47b3f4e23a9). These are current local refresh evidence, not replacement revision pins. The pinned material above remains separate historical evidence and was not rewritten as a new observation.

**Verified fact:** `gentle-shell` is the current Pi-facing product/repository name, while npm, runtime, package, and source-repository identities can still use `gentle-pi`. This is an in-progress mixed naming transition, not a completed package rename. Stable refresh sources are the [gentle-shell repository](https://github.com/Gentleman-Programming/gentle-pi), [Gentle AI repository](https://github.com/Gentleman-Programming/gentle-ai), and [Engram repository](https://github.com/Gentleman-Programming/engram); refresh when their documented product naming, ownership boundary, Pi setup, or workflow semantics change.

**Verified fact:** `gentle-shell` owns Pi-native orchestration, bounded subagents, ODD task artifacts and their project-scoped mirroring through Engram, optional SDD, review-transport/mirror contracts, and optional Engram composition. Its ODD flow closes each substantial task with a work-unit commit; native review candidates are work-unit commits or PR slices, never a checkbox or accumulated feature branch; and delivery strategy considers chained slices around 400 authored changed lines. When background-subagent policy is active and `subagent_run` is available, routine delegations default to background execution. The Engram mirror is specific to ODD coordination and does not limit Engram's broader continuity capability. Native review semantics remain Gentle AI/provider-owned. Gentle AI documents ODD as the default workflow, SDD as explicitly selected, and RDD as user-owned and opt-in; an SDD archive is neither review approval nor delivery. Engram `f6b22f89d024fb6aaa207606d893f47b3f4e23a9` makes direct backup format `0.2.0` preserve local pins and complete memory-relation metadata while accepting legacy `0.1.0` backups that omit those fields.

## Bounded Impact Assessment (2026-09-17)

**Verified fact:** the audit covered 11/12 repository evidence paths, 8/8 direct dependent records, and 0/20 commits; the bound was not exhausted. No ownerless affected surface was found. Every direct record is currently **no impact**:

### Reproducibility Boundary

`11/12` means the eleven exact repository paths below were inspected and one capacity slot remained unused; it does not mean a known path was omitted. The audit inspected no commits (`0/20`) and does not claim unbounded proof.

Repository evidence paths inspected:

- `REPO-HARNESS.md`
- `.pi/skills/repo-harness-documentation/SKILL.md`
- `odd/tasks/gentle-ecosystem-reconciliation.md`
- `docs/repo-harness/roadmap.md`
- `docs/repo-harness/current-state.md`
- `docs/repo-harness/decisions.md`
- `docs/repo-harness/integration-research.md`
- `docs/repo-harness/README.md`
- `.repo-harness/rules/response-contract.md`
- `skill/repo-harness-builder/scripts/harness_tool.py`
- `skill/repo-harness-builder/tests/test_harness_tool.py`

The following official-repository-relative source locators were inspected from owner-selected local snapshots. Their storage location is intentionally not part of canonical documentation; their exact local Git commit identities are recorded in the current refresh above:

- [gentle-shell](https://github.com/Gentleman-Programming/gentle-pi)
  - `assets/orchestrator-delegation.md`
  - `assets/orchestrator-memory.md`
  - `assets/sdd-orchestrator-workflow.md`
  - `contracts/review-provider-contract-mirror/v1.2.0/bundle/orchestration/pi.md`
- [Gentle AI](https://github.com/Gentleman-Programming/gentle-ai)
  - `docs/intended-usage.md`
  - `docs/trigger-rules.md`
  - `docs/review-integration.md`
- [Engram](https://github.com/Gentleman-Programming/engram)
  - `docs/AGENT-SETUP.md`
  - `docs/intended-usage.md`
  - `docs/codebase/memory-core.md`

`RH-W-0077` is not a direct Gentle ecosystem contract dependent: its personal-storage containment issue is unaffected because no canonical owner adopts or depends on the snapshot location. The direct set remains 8/8; audit inputs are promoted as source identities and conclusions, not as a repository dependency.

| Record | Classification and current basis | Reopen or advancement condition |
|---|---|---|
| `RH-W-0025` | **no impact** — current naming and capability evidence preserves its completed two-component, owner-scoped contract. | Reopen only if capability classification gains another state, integrated selection bypasses either verification, owner state is copied, post-installation loss affects the wrong component, promotion bypasses repository verification or human approval, or RH-W-0008 replacement safety regresses. |
| `RH-W-0048` | **no impact** — no current evidence contradicts the completed bounded recovery contract. | None. |
| `RH-W-0050` | **no impact** — current evidence does not alter its completed reconciliation or continuity boundaries. | Reopen only if bounded reconciliation, finding disposition, operation-level continuity outcomes, standalone degradation, projection parity, or post-delivery convergence regresses. |
| `RH-W-0053` | **no impact** — no contradiction affects the completed checkpoint contract. | None. |
| `RH-W-0069` | **no impact** — current evidence leaves the completed local-backup boundary intact. | Terminal; reopen only through a new immutable owner record. |
| `RH-W-0072` | **no impact** — no concrete missing route or affected operation was found. | A concrete missing route and affected operation, followed by owner-approved scope, are required for any further correction. |
| `RH-W-0078` | **no impact** — upstream evidence supports rather than changes its measured progressive-disclosure direction or scope gate. | Advance only after `RH-W-0076` lands the corrected guide sentence and the owner approves a bilingual scope that prices and verifies the recommended direction. Implementation remains separately unauthorized. |
| `RH-W-0079` | **no impact** — current optional-capability evidence does not contradict its completed release-boundary classification. | None. Any actual release remains separate. |

**Verified fact:** historical pins, versions, receipts, and old failures remain historical evidence; they must not be rewritten as current defects. Current upstream capability growth or naming changes alone do not reopen completed work. This audit requires no roadmap status, priority, dependency, scope, current-state, map, changelog, product, test, generated-contract, or runtime-projection change.

**Current evidence, not a permanent claim:** refresh this assessment if owner contracts change, a concrete missing route appears, capability behavior fails, or a listed reopen condition is violated.

## Primary Sources

- Harness: [rulebook](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/REPO-HARNESS.md), [memory rule](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/.repo-harness/rules/memory.md), and [RH-W-0022](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/docs/repo-harness/roadmap.md#rh-w-0022).
- Gentle AI: [trigger rules](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/trigger-rules.md), [organic RDD](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/architecture/organic-rdd.md), [status contract](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/internal/assets/skills/_shared/sdd-status-contract.md), and [organic runtime tests](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/e2e/organicruntime/organic_runtime_test.go).
- Engram: [intended usage](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/intended-usage.md), [architecture](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/ARCHITECTURE.md), [agent setup](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/AGENT-SETUP.md), [memory core](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/codebase/memory-core.md), and [store behavior](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/internal/store/store.go).

## Authority Matrix

| Concern | Authority | Harness responsibility |
|---|---|---|
| Durable policy, current truth, source, tests, and backlog | Repository | Reconcile evidence and update its canonical owners |
| Routing, exact-candidate review, receipts, and bounded review correction | Gentle AI when enabled | Invoke and relay owner-issued status without copying it |
| Semantic observations, relations, stale-state metadata, and session continuity | Engram | Retrieve or save context, then reconcile claims with repository evidence |
| Capability composition | Harness | Select a valid branch without persisting another lifecycle |

## Capability and Availability Matrix

| Gentle AI | Engram | Required behavior |
|---|---|---|
| Available | Available | Repository policy remains primary and owns commit, push, PR, and release; use Engram for continuity and Gentle AI for applicable owner-controlled routing, exact-candidate review, receipts, and bounded review correction |
| Available | Unavailable | Use repository evidence and Gentle AI contracts; report continuity degradation |
| Unavailable | Available | Use Engram continuity under repository policy; never infer Gentle AI approval |
| Unavailable | Unavailable | Work from repository evidence alone; report unavailable integration capabilities without blocking ordinary work unless local policy requires them |

## Recommended Generic Contract

1. Resolve repository root and repository policy first.
2. Detect Engram through tool availability and project detection. Detect Gentle AI through an advertised read-only capability or status surface, never private layout or executable presence alone.
3. Reconcile recalled claims and stale-state metadata with current repository evidence before acting.
4. Let each enabled owner issue its own statuses and transitions. Do not synthesize a combined authority verdict.
5. Return repository outcome, component outcomes, degradation reasons, and one next action.

## Organic and Proportional Behavior

Use direct work by default. Gentle AI uses ODD as the default workflow; enter SDD only when explicitly selected. Memory retrieval, checks, review, and retries must stay bounded by the task. Integration availability must not cause broad reads, synthetic artifacts, or ceremony that the repository does not need. Gentle AI disabled mode means ordinary repository policy, not denial or approval.

## Durable Knowledge Promotion Rule

Engram observations and session summaries are continuity inputs, not durable team truth. Promote only verified, still-relevant knowledge into the repository's proper canonical owner, in the same work unit when that truth must change. Keep version-bound tool inventories, flags, schemas, decay values, and installation details out of durable policy unless the product contract depends on them.

## Anti-Patterns and Explicit Non-Goals

- No third lifecycle, attempt ledger, conflict store, receipt cache, or copied authority state.
- No memory-owned backlog, current state, approval, or delivery decision.
- No fabricated approval, automatic review enablement, automatic stale-memory review, or sensitive conflict resolution.
- No custom memory protocol when Engram is available and no synthetic SDD artifacts for direct work.
- No code graph, dependency engine, daemon, scheduler, external service, or lifecycle promotion in this research artifact.
- No product-specific wording in generated consumer harnesses. Self-hosting applies generic behavior to this repository; it does not change generated language.

## Failure and Degradation Semantics

- Missing or contradictory repository authority blocks the affected work or requires a decision.
- Engram failure degrades continuity only; never claim recovery or persistence succeeded.
- Gentle AI absent or disabled defers to repository policy; only its routing, exact-candidate review, receipts, and bounded review correction become unavailable or unmanaged, while repository policy continues to own delivery.
- Malformed owner status stops that optional branch with exact evidence; the harness must not reconstruct authority.
- Stale memory must be labeled and verified. Marking it reviewed requires explicit confirmation.
- Pending memory conflict remains component-scoped and follows Engram's judgment gates.

## Relationship to RH-W-0022

This contract is related to, but separate from, `RH-W-0022`. That work unit owns declared documentation relationships. A future integration work unit would own runtime capability branches and authority boundaries. It should depend on `RH-W-0022` only if implementation requires its relationship map to enforce same-work-unit promotion. This research does not change either lifecycle status.

## Candidate Canonical Builder Surfaces

- `skill/repo-harness-builder/scripts/harness_tool.py`: generated branches and templates.
- `skill/repo-harness-builder/SKILL.md` and `command/repo-harness-builder.md`: builder interaction and command projection.
- Generated memory, session-start, session-stop, usage, and decision documents: authority, continuity, degradation, and human guidance.
- `docs/repo-harness/roadmap.md`: only a future approved work unit and its relationship to `RH-W-0022`.

## Required Tests

- Contract or golden coverage for all four availability combinations.
- Repository evidence wins over conflicting or stale memory.
- Component failure never produces a false success, approval, continuity claim, or delivery denial.
- No duplicate lifecycle, attempt, artifact, or receipt state is emitted.
- Sensitive conflicts and stale review use the correct human gate.
- Component outcomes remain distinct and expose one actionable next step.
- Existing generated output, locales, and runtime projections remain stable.

## Unresolved Questions and Confidence

- Should Engram be required only when a generated repository explicitly selects integrated continuity? Confidence: medium.
- Which read-only Gentle AI capability probe is stable and independent of installation layout? Resolved 2026-08-15: `gentle-ai version` returns `gentle-ai <version>` with exit 0, resolves through PATH rather than a known directory, and returns before system detection, configuration reads, or update checks. Treat a validated exit code and output shape as `verified`; presence on PATH alone stays `found_on_path` and never authorizes a capability, because this document forbids executable presence as a detection basis. Confidence: high.
- Should the public result expose an aggregate status at all? Recommendation: repository outcome plus component outcomes. Confidence: high.

Confidence is high for authority boundaries, availability behavior, promotion rules, anti-patterns, and degradation semantics. It is medium for future discovery interfaces because the pinned sources define owner contracts but not a generic harness probe.
