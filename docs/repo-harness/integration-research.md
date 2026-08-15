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

## Primary Sources

- Harness: [rulebook](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/REPO-HARNESS.md), [memory rule](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/.repo-harness/rules/memory.md), and [RH-W-0022](https://github.com/LCubero/repo-harness-builder/blob/58f41a790556d9dbb2c9e3094459b85c206b8fbd/docs/repo-harness/roadmap.md#rh-w-0022).
- Gentle AI: [trigger rules](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/trigger-rules.md), [organic RDD](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/docs/architecture/organic-rdd.md), [status contract](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/internal/assets/skills/_shared/sdd-status-contract.md), and [organic runtime tests](https://github.com/Gentleman-Programming/gentle-ai/blob/4004a0825d03a3d123a3c4f67bf375cb16fd6d8c/e2e/organicruntime/organic_runtime_test.go).
- Engram: [intended usage](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/intended-usage.md), [architecture](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/ARCHITECTURE.md), [agent setup](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/AGENT-SETUP.md), [memory core](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/docs/codebase/memory-core.md), and [store behavior](https://github.com/Gentleman-Programming/engram/blob/1dafc0f63051b2214100f7bd801357e4aab61c26/internal/store/store.go).

## Authority Matrix

| Concern | Authority | Harness responsibility |
|---|---|---|
| Durable policy, current truth, source, tests, and backlog | Repository | Reconcile evidence and update its canonical owners |
| Routing, exact-candidate review, receipts, and delivery gates | Gentle AI when enabled | Invoke and relay owner-issued status without copying it |
| Semantic observations, relations, stale-state metadata, and session continuity | Engram | Retrieve or save context, then reconcile claims with repository evidence |
| Capability composition | Harness | Select a valid branch without persisting another lifecycle |

## Capability and Availability Matrix

| Gentle AI | Engram | Required behavior |
|---|---|---|
| Available | Available | Repository policy remains primary; use Engram for continuity and Gentle AI for applicable owner-controlled routing, review, and delivery |
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

Use direct work by default. Delegate or use SDD only when requested or materially useful and accepted. Memory retrieval, checks, review, and retries must stay bounded by the task. Integration availability must not cause broad reads, synthetic artifacts, or ceremony that the repository does not need. Gentle AI disabled mode means ordinary repository policy, not denial or approval.

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
- Gentle AI absent or disabled defers to repository policy and leaves its review or delivery capability unmanaged.
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
