# RH-W-0050 Bounded Reconciliation Design

> **Status: DRAFT. Pending external review and owner approval.**
>
> This review proposal is not an approved design, implementation authorization, lifecycle advancement, or delivery. RH-W-0050 remains `proposed`.

## Review Contract And State Labels

Review this draft against the canonical [RH-W-0050 roadmap record](roadmap.md#rh-w-0050), [standing decisions](decisions.md), checked-in [memory rules](../../.repo-harness/rules/memory.md), and [bilingual maintenance contract](i18n-maintenance.md). Repository evidence is canonical. The [Spanish peer](../es/repo-harness/rh-w-0050-design.md) must remain complete and semantically equivalent.

- **Approved constraint:** already authorized by the roadmap, decisions, rulebook, or current generated contract.
- **Verified current behavior:** directly observed in current repository evidence and not proposed by this draft.
- **Recommended decision:** proposed here and pending owner approval.
- **Remaining implementation detail:** intentionally deferred until an approved implementation task.

## Problem, Goals, And Non-Goals

Session start and session stop reconcile evidence but do not define a deterministic activation contract for complete finding reconciliation. Undefined baselines and bounds can cause either unbounded review or false next-ready routing.

This draft recommends a hybrid bounded scan plus evidence-triggered full reconciliation. It preserves repository authority, explicit human approval, bilingual parity, independent operation outcomes, Git and non-Git operation, and ephemeral observability without adding a backlog, cache, trigger state, receipt, conflict store, lifecycle store, ledger, daemon, scheduler, or framework.

This draft does not implement generated behavior, code, tests, storage, migration, automatic promotion, lifecycle mutation, conflict judgment, or deletion. It does not advance RH-W-0050 or implement any other RH-W unit.

## Authority And Verified Current Behavior

- Repository files, applicable local Git evidence, source, tests, checker output, and explicit approvals are canonical. Engram provides semantic continuity only.
- The roadmap owns lifecycle; current state is its concise projection; decisions own approved rationale; changelogs own completed chronology; localized peers present equivalent human content.
- Every harness Engram operation uses explicit repository project scope and reports its own outcome. `active`, `needs_review`, and `judgment_required` are advisory metadata, not truth states.
- Successful Engram reads or writes remain successful when another Engram operation fails. Capture, prompt association, conflict discovery, lifecycle review, and sync require their own evidence.
- The checked-in self-hosted `.repo-harness/rules/memory.md` currently contains richer repository-specific operation-level rules.
- Verified source inspection shows that `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` does not yet generate that complete richer contract for consumers. The richer checked-in file is current self-hosted policy, not proof of generated product behavior.
- A future implementation must decide and implement the canonical generated memory contract, then verify integrated and standalone renders separately. No wording below may imply that this generation already exists.

## Recommended Architecture

**Recommended decision, not approved:** every start and qualifying stop runs a deterministic bounded scan. Mandatory evidence or an explicit full request runs full reconciliation. Advisory signals only narrow inspection. A bounded request stays bounded unless it independently verifies a mandatory trigger.

| Owner | Proposed responsibility |
|---|---|
| Session start | Build an ephemeral initial snapshot, run the applicable bounded route, and suppress authoritative next-ready routing when the baseline is unknown or a mandatory finding remains. No mutation by default. |
| Session stop | Compare against the verified initial snapshot, run mandatory post-delivery reconciliation, and apply only already-authorized owner updates. |
| Documentation skill | Select the smallest canonical owner, classify owner promotion, maintain maps and localized parity, and reject duplicate stores. |
| Memory rules | Define explicit project scope and operation-level evidence after the product generator decision is implemented. |
| Roadmap and current state | Own lifecycle and its direct projection. |
| Repository owner | Approve final design, implementation, discretionary promotion, lifecycle changes, and destructive action. |

## Glossary

| Term | Normative definition |
|---|---|
| Delivery | Verified completion of an approved work result through the repository's applicable delivery evidence. A commit message, ahead-only state, memory claim, or user wording alone is not delivery. |
| Lifecycle delivery | Completion of a canonical lifecycle correction or next-ready correction. It mandates post-delivery reconciliation. Wording-only parity repair is not lifecycle delivery unless it exposes or changes a semantic conflict. |
| Meaningful stop | Session stop after any repository mutation, verified delivery claim, lifecycle or next-ready correction, retained finding, or continuity write attempt. A read-only no-change stop is not meaningful. |
| Direct projection | A document field whose value is derived from one canonical owner claim, such as current-state status from a roadmap record or a localized peer of that claim. |
| Relevant finding | Evidence within applicable owner scope that can change classification, lifecycle, delivery, parity, continuity, or next-ready routing. |
| Retained finding | A relevant finding not proven duplicate, irrelevant, unsupported, or already represented canonically. |
| Baseline | An ephemeral verified repository snapshot used for comparison during one invocation or session. It is never persisted as trigger state. |
| Bounded scan | Inspection constrained by the exact scope and numeric limits below. |
| Full reconciliation | Classification and disposition of every retained finding inside applicable owner scope, not the whole portfolio. |
| Mandatory trigger | Verified evidence that requires full reconciliation before authoritative routing. |
| Advisory signal | Evidence that may narrow inspection or appear in output but cannot trigger full reconciliation alone. |
| Authoritative next-ready recommendation | A recommendation represented as derived from reconciled canonical lifecycle and delivery evidence. Discussion and tentative options are not authoritative recommendations. |

## Activation Matrices

### Start

| Mode | Inputs | Route and outputs | Engram operations | Mutation permission |
|---|---|---|---|---|
| Integrated, explicit RH-W objective | Exact owner scope, local Git, checker, scoped Engram subject evidence | Bounded scan; full only on mandatory trigger; report result state and next-ready outcome | Resolve explicit project, then subject query; other operations only when independently applicable | None by default |
| Integrated, no explicit RH-W objective | Canonical active projection, applicable local Git, scoped Engram subject evidence | Derive bounded baseline; if unreliable, `baseline: unknown` and suppress authoritative next-ready | Resolve explicit project before any read; zero operations under project ambiguity | None by default |
| Standalone, explicit RH-W objective | Same repository owner scope without Engram | Same routing with memory dimension `not_applicable` | None | None by default |
| Standalone, no explicit RH-W objective | Canonical active projection and applicable local Git | Same baseline-known or baseline-unknown behavior | None | None by default |

### Stop

| Mode | Inputs | Route and outputs | Engram operations | Mutation permission |
|---|---|---|---|---|
| Integrated, meaningful stop | Verified initial snapshot, changed evidence, delivery and continuity outcomes | Compare to initial snapshot; full reconciliation for lifecycle delivery or another mandatory trigger | Independently report each applicable operation | Only approved same-work-unit owner and continuity updates |
| Integrated, non-meaningful stop | Verified initial snapshot and unchanged evidence | `not_triggered`; no owner or continuity restatement | None unless a separately requested continuity action applies | None |
| Standalone, meaningful stop | Verified initial snapshot and changed repository evidence | Same comparison and trigger behavior without memory | None | Only approved repository owner updates |
| Standalone, non-meaningful stop | Verified initial snapshot and unchanged evidence | `not_triggered`; no receipt or no-op write | None | None |

## Deterministic Baseline And Bounds

### Exact Applicable Owner Scope

At session start with an explicit RH-W objective, applicable owner scope is exactly:

1. That one roadmap record.
2. Its current-state direct projection, if present.
3. Its declared localized peer sections.
4. Decisions and changelog entries that name that RH-W ID or an exact claim cited by the record.
5. Current local branch evidence affecting those owner paths or an explicit delivery claim.
6. Engram results from the exact topic `work/RH-W-####` and one subject query containing the exact RH-W ID, only after explicit project resolution.

At stop, the baseline is the session's verified initial snapshot of those items. Changed scope is calculated by comparing current bytes, parsed owner fields, and local Git identity to that snapshot. No prior session, cache, or inferred timestamp substitutes for it.

At start without session-local state, derive an ephemeral baseline in this order:

1. Parse the applicable roadmap record and its direct current-state and localized projections.
2. If Git is available, inspect `HEAD`, status for owner paths, and commits touching owner paths from merge-base with the configured upstream through `HEAD`. If no usable upstream exists, inspect only `HEAD` and owner-path status and do not infer remote delivery.
3. If all applicable canonical claims have an owner and no comparison failure or unexplained owner-path change remains, baseline is `known`.
4. Otherwise baseline is `unknown`; report missing evidence and suppress authoritative next-ready routing. Never guess.

### Proportional Bounds

- **Commit inspection:** at most 20 commits touching applicable owner paths, newest first. Stop earlier once every changed owner field and explicit delivery claim has direct evidence. More than 20 relevant commits yields `bound_exhausted` and suppresses authoritative routing.
- **Owner paths:** the six scope classes above, with at most one roadmap record, one current-state projection, one localized peer per declared locale, all exact-ID decisions/changelog matches, and at most 12 distinct local evidence paths directly named by those claims. A thirteenth path yields `bound_exhausted`.
- **Engram queries:** at most two reads, the exact topic and one exact-ID subject query, each capped at 10 observations. Empty is valid only after successful same-surface operation evidence or a validated same-surface positive control. Adapter mechanics are an implementation detail.
- **Dependency traversal:** inspect the changed RH-W record and direct projections. Inspect only direct dependents whose declared routing field references a changed status, dependency, priority, or advancement condition, capped at 8 records. Never traverse transitive or all dependents automatically. A ninth direct-impact dependent yields `bound_exhausted`.
- **Full reconciliation:** expands finding classification within this bounded applicable scope. It does not remove these bounds or scan the complete backlog, all commits, all documentation, or all memory.

Repeated scans over unchanged evidence must produce the same scope, classifications, result state, and mutation set. The second scan after a successful repair must converge to no further mutations.

## Requests And Trigger Classification

| Class | Signal | Required treatment |
|---|---|---|
| Mandatory | Explicit full reconciliation request | Run full reconciliation within applicable owner scope. |
| Requested bounded route | Explicit bounded inspection request | Run only the bounded route unless independent evidence verifies another mandatory trigger. |
| Mandatory | Product delivery or lifecycle delivery | Run full reconciliation before authoritative next-ready routing. |
| Mandatory | Canonical lifecycle inconsistency or false routing risk | Reconcile the changed record and direct projections. Add bounded direct dependents only for changed routing fields. |
| Mandatory | Incomplete delivery evidence | Retain the gap and suppress authoritative next-ready routing. |
| Mandatory | Verified repository-memory contradiction | Repository evidence wins; classify disposition and preserve operation evidence. |
| Mandatory | Unclassified retained finding | Run full reconciliation and block only the affected branch if it remains unclassified. |
| Advisory | `judgment_required`, `needs_review`, stale summary, uninspected commit hint | Verify against the owner; never trigger alone. |
| Advisory | Unrelated dirty work, ahead-only Git, wording-only parity difference, non-lifecycle checker drift | Report or route narrowly; never infer delivery or lifecycle drift. |
| Rejected | Dirtiness, age, invocation count, broad result volume, commit keyword, network absence, non-Git mode | No trigger without separate verified mandatory evidence. |
| Rejected | Unvalidated empty search or unverified capture, prompt, plugin, compaction, or sync claim | Do not claim success or absence. |

## Full Reconciliation And Finding Dispositions

1. Inventory retained findings only inside applicable owner scope.
2. Resolve canonical owner and evidence class for each finding.
3. Compare canonical, direct projection, localized, delivery, local Git, and applicable memory evidence.
4. Assign exactly one disposition.
5. Apply only non-discretionary repairs already authorized by the task and policy.
6. Pause only for a human-owned choice; block on missing required evidence; fail on an attempted operation error.
7. Re-run the bounded scan and require convergence before authoritative routing.

| Disposition | Legal condition | Mutation authority |
|---|---|---|
| `integrate` | Valid retained finding belongs in an existing owner and is not duplicated. | Requires existing task authority; discretionary content requires owner approval. |
| `close` | Current owner and delivery evidence prove satisfaction or scoped supersession. | Never from memory alone; lifecycle mutation requires approval. |
| `defer` | Valid finding has a named blocker, prerequisite, or future condition. | Use existing roadmap owner or an approved update. |
| `reject` | Approved goals conflict with it or the owner explicitly declines it. | Preserve durable rejection only in an existing owner. |
| `noise` | Duplicate, irrelevant, unsupported, merely advisory, or already canonical. | No record or mutation. |

Classification never authorizes implementation, deletion, or lifecycle advancement.

## Result States And Independent Outcomes

| State | Legal condition |
|---|---|
| `completed` | Applicable scan or reconciliation finished within bounds, every retained finding is classified, required authorized updates succeeded, parity is verified, and no affected branch remains unresolved. |
| `paused` | A specific human-owned decision is required; verified independent outcomes remain valid. |
| `blocked` | Required evidence, owner resolution, reliable baseline, or bound capacity is unavailable without an operation failure. |
| `failed` | An attempted required operation failed. Name that operation; do not collapse unrelated outcomes. |
| `not_triggered` | Bounded scan completed and no mandatory trigger was verified. |

Keep repository resolution, baseline, bounded scan, trigger, finding classification, canonical updates, localized parity, delivery evidence, next-ready routing, Git, and every Engram operation separate. Do not fabricate an aggregate Engram status.

## Engram Evidence And Degradation

| Operation | Success evidence | Failure or absence treatment | Preserved outcome |
|---|---|---|---|
| Project resolution | Explicit repository project resolved by supported capability | Under ambiguity perform zero Engram operations; local work continues | Repository and Git evidence |
| Read | Scoped valid response, including validated empty | Mark read failed or unavailable; do not claim recovered continuity | Project resolution and repository scan |
| Write | Scoped confirmation | Mark write failed; do not claim saved continuity | Repository updates and reads |
| Prompt association | Direct same-operation evidence | Mark unknown or failed | Successful observation write |
| Conflict discovery | Valid candidate response | Mark failed or unavailable; do not invent no conflict | Read or write |
| Lifecycle review | Supported review-age response | Mark unavailable; verify owner claim directly | Other operations |
| Plugin capture | Session and project capture evidence | Mark unverified or failed | Manual supported operations |
| Sync | Scoped sync evidence | Mark unknown or failed | Local Engram and repository evidence |

Engram absence degrades continuity according to applicable policy but never proves repository drift or creates fallback storage.

## Mutation, Stops, And Delivery Routing

Human approval is required for final design, implementation, ambiguous repository selection, disputed owner semantics, discretionary promotion, lifecycle field changes, rejection of a valid product direction, destructive cleanup, and semantic localization exceptions.

Stop only the affected branch for ambiguity, an unresolved retained finding, incomplete delivery evidence, unknown baseline, bound exhaustion, owner-update failure, localized-update failure, or scope beyond approval. Preserve unrelated dirty work and successful independent outcomes.

After product or lifecycle delivery: verify delivery, run full reconciliation, update approved canonical owners and localized peers in one work unit, independently report continuity updates, verify the refreshed scan converges, and only then permit authoritative next-ready routing. Wording-only parity repair does not count as lifecycle delivery unless it exposes or changes a semantic conflict.

## Ephemeral Output Shape

```text
repository: <outcome and evidence class>
baseline: <known | unknown> (<evidence>)
bounded_scan: <completed | blocked | failed> (<scope and bounds>)
reconciliation: <completed | paused | blocked | failed | not_triggered>
trigger: <mandatory signals or none>
findings: <IDs and dispositions>
canonical_updates: <none | paths | paused | failed>
localized_parity: <not_applicable | verified | drift | paused | failed>
delivery_evidence: <not_applicable | complete | incomplete | unknown>
next_ready: <recommended unit | suppressed | not_requested>
git: <not_git | local outcome | optional branch outcome>
engram.<operation>: <independent outcome>
next_action: <one bounded action or human decision>
```

Only applicable fields render. This output is not a persisted receipt.

## Formal RED/GREEN Acceptance Matrix

| Scenario and observable input | RED defect | GREEN outcome | Mutation expectation | Preserved independent outcomes |
|---|---|---|---|---|
| Baseline known: consistent owners and usable local Git | Baseline is inferred vaguely | `baseline: known`; deterministic scope reported | None | Git and owner evidence |
| Baseline unknown: missing owner or failed comparison | Routing guesses readiness | `blocked`; `next_ready: suppressed` | None | Verified evidence remains valid |
| Bound exhaustion: 21 commits, 13 paths, 11 query results, or 9 direct dependents | Scan silently expands or truncates | Exact exhausted bound reported and routing suppressed | None | Completed bounded dimensions |
| Repeat unchanged scan | Results or writes vary | Identical scope, state, findings, and zero mutations | None | All prior verified outcomes |
| Successful repair followed by scan | Repair loops or restates | Second scan converges with zero further mutations | Only first authorized repair | Delivery and parity evidence |
| Explicit full request | Route remains partial | Full reconciliation runs within applicable scope | Only separately authorized writes | Git and Engram outcomes |
| Explicit bounded request, no other trigger | Route expands to full | Only bounded inspection runs | None | Normal routing evidence |
| Explicit bounded request plus verified delivery | Request suppresses mandatory trigger | Full reconciliation runs because delivery independently triggers | Authorized delivery reconciliation only | Request evidence |
| Incomplete delivery evidence | Next-ready is recommended | `next_ready: suppressed`; gap retained | None without authority | Repository and Git inspection |
| Unclassified retained finding | Finding is dropped or routing continues | Full route runs; affected branch blocks if still unclassified | None without authority | Classified findings |
| Unrelated dirty work | Dirtiness triggers or is modified | Reported as advisory and preserved | Never touch unrelated paths | Reconciliation result |
| Ahead-only Git without delivery proof | Delivery is inferred | Ahead commits reported; delivery remains unknown | None | Local Git evidence |
| Validated empty memory search | Empty is rejected or treated as global absence | Empty accepted after same-surface evidence or positive control | None | Project resolution |
| Unverified capture | Installation implies capture success | Capture is `unverified` | None | Manual read or write |
| Project ambiguity | Engram is queried under guessed project | Reconciliation pauses with zero Engram operations | None | Local repository evidence |
| Canonical owner update partially fails | All updates are called failed or complete | Failed path named; successful paths preserved; parity unresolved | No retry beyond approved bound | Successful owner writes |
| Localized update fails | Canonical success implies parity | Canonical result preserved; localized parity `failed`; routing suppressed if semantic | No unrelated rollback | Canonical update |
| Multiple simultaneous triggers | One trigger hides the others | One full cycle reports every verified mandatory trigger | One authorized mutation set | Every trigger's evidence |
| `integrate` finding | Finding creates a new store | Existing owner selected with approval boundary | Existing owner only | Other dispositions |
| `close` finding | Memory alone closes it | Owner and delivery evidence prove closure | Approved lifecycle owner only | Memory result |
| `defer` finding | Finding disappears | Named condition retained in existing roadmap owner | Approved roadmap update only | Other classifications |
| `reject` finding | Rejection is silent or destructive | Reason and owner authority retained | Existing owner only | Historical evidence |
| `noise` finding | Noise creates a record | No durable write | None | Relevant findings |
| Standalone mode | Engram is required or emulated | Repository route completes with memory `not_applicable` | Repository permissions only | Git and owner evidence |
| Normal no-trigger start | Invocation forces full review | `not_triggered`; normal routing continues | None | Bounded evidence |
| Non-meaningful stop | Stop creates chronology or memory noise | `not_triggered`; no no-op write | None | Initial snapshot |
| Lifecycle delivery | Correction bypasses post-delivery cycle | Full reconciliation precedes routing | Approved same-work-unit updates | Delivery evidence |
| Wording-only parity repair | Every wording edit becomes delivery | No lifecycle-delivery trigger unless semantic conflict changes | Localized repair only | Lifecycle state |
| Advisory Engram metadata | Metadata proves conflict | Remains advisory until owner verification | None | Engram operation result |
| Partial Engram failure | One failure collapses all memory status | Each operation reports independently | Successful writes remain | Repository result |
| No duplicate store | Implementation adds cache, receipt, or ledger | Existing owners and ephemeral output only | No new store | Existing continuity |

## Evidence Matrix

| Claim | Evidence class | Canonical owner or symbol | Preserved or changed |
|---|---|---|---|
| RH-W-0050 remains `proposed` | Verified current behavior | `roadmap.md#rh-w-0050` | Preserved |
| Repository evidence outranks memory | Approved constraint | `REPO-HARNESS.md`, memory rules | Preserved |
| Self-hosted memory rules are operation-rich | Verified current behavior | `.repo-harness/rules/memory.md` | Preserved and clarified |
| Product generator lacks the complete richer memory contract | Verified current behavior | `harness_tool.py::rule_outputs` | Corrected claim |
| Hybrid bounded activation should be adopted | Recommended decision | This draft | Changed and pending approval |
| Empty search validation uses same-surface evidence | Approved existing discipline | Roadmap RH-W-0040 evidence | Resolved recommendation |
| Direct dependents are inspected only on changed routing fields | Recommended decision | This draft | Resolved recommendation |
| Lifecycle delivery mandates post-delivery reconciliation | Recommended decision | This draft | Resolved recommendation |

## Implementation Impact Map

This map identifies future work only. It authorizes no edit.

| Stable symbol or path | Required future decision or change |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill` | Generate baseline, bounded start activation, request split, result states, and routing suppression. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill` | Generate initial-snapshot comparison, meaningful-stop, lifecycle-delivery, and convergence contracts. |
| `skill/repo-harness-builder/scripts/harness_tool.py::documentation_skill` | Generate classification-driven owner promotion, direct-projection handling, and localized parity obligations. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` | Decide and generate the complete richer operation-level memory contract. Verify integrated and standalone outputs separately. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render` and `render_with_skill_mapping` | Preserve existing modes and paths; add no persisted trigger input. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Add every formal RED/GREEN row to the existing suite, including integrated and standalone renders. |
| `skill/repo-harness-builder/references/architecture.md` | Document approved owner and activation boundaries. |
| `skill/repo-harness-builder/references/validation.md` | Document deterministic bounds and convergence verification. |
| `.repo-harness/skills/repo-harness-session-start/SKILL.md` and session stop peer | Future generated self-hosted consumer contracts after approval and implementation. |
| `.repo-harness/rules/memory.md` | Current richer self-hosted policy; future generated output must be reconciled without assuming current generator parity. |
| `.opencode/skills/repo-harness-builder/**` and `.claude/skills/repo-harness-builder/**` | Exact package projections only after canonical implementation. |

## Rollback, Remaining Details, And Review Obligations

The proposal adds no persisted schema, cache, or ledger, so a future rollback would restore prior generated wording and focused tests together and resynchronize existing projections. Existing Engram observations remain untouched.

Remaining implementation details include exact parser structures, report formatting, adapter-specific positive-control mechanics, test fixture construction, and renderer placement after owner approval. They may not weaken the baselines, bounds, states, independent outcomes, or integrated and standalone obligations in this draft.

External review must verify:

- [ ] Draft and `proposed` status are unmistakable; no implementation or approval is implied.
- [ ] Every baseline, bound, trigger, stop, state, and mutation rule is deterministic.
- [ ] Full and bounded human requests remain distinct.
- [ ] Product generator and richer self-hosted memory policy are not conflated.
- [ ] All formal acceptance rows are observable and preserve independent outcomes.
- [ ] Integrated and standalone, start and stop behavior are complete.
- [ ] English and Spanish sections, normative tables, lists, and obligations are semantically equivalent.
- [ ] Impact symbols exist and no wording claims generated behavior already exists.
- [ ] Links resolve and no U+2013, U+2014, unresolved placeholder, contradiction, or approval drift remains.
