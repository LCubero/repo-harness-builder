# RH-W-0050 Bounded Reconciliation Design

> **Status: DRAFT. Pending external review and owner approval.**
>
> This is a review proposal, not an owner-approved final design, implementation approval, lifecycle advancement, or delivery. RH-W-0050 remains `proposed`.

## Review Contract

Review this document against the canonical [RH-W-0050 roadmap record](roadmap.md#rh-w-0050), [standing decisions](decisions.md), [memory rules](../../.repo-harness/rules/memory.md), and [bilingual maintenance contract](i18n-maintenance.md). Repository evidence is canonical. The [Spanish peer](../es/repo-harness/rh-w-0050-design.md) is a complete human presentation and must remain semantically equivalent.

Statement labels used below:

- **Approved constraint:** already authorized in the roadmap or decisions and preserved here.
- **Recommended decision:** proposed by this draft and still requires owner approval.
- **Open question:** genuinely unresolved and assigned to external review or the owner.

## Problem And Failure Modes

Session start and session stop reconcile relevant evidence, but they do not define a bounded activation contract for deciding when a complete finding reconciliation is necessary. After delivery, unresolved findings, stale lifecycle projections, incomplete delivery evidence, duplicated localized continuity, or contradictory memory can make a later session recommend work from false state.

Failure modes are:

- Running a full portfolio and memory review at every start or stop, creating high cost and noise.
- Running a full cycle only when a human notices a problem, allowing false routing state to persist.
- Treating any dirty file, old summary, commit keyword, or Engram candidate as proof of drift.
- Treating Engram availability, `active`, `needs_review`, or `judgment_required` as an aggregate truth verdict.
- Collapsing independent Engram operation failures and discarding valid partial success.
- Recommending the next ready unit before post-delivery evidence and lifecycle owners agree.
- Creating another backlog, conflict store, receipt store, trigger cache, scan receipt, framework, ledger, or cumulative reconciliation document.

## Goals

- Define when start and stop perform a bounded scan and when evidence requires a full cycle.
- Classify findings as `integrate`, `close`, `defer`, `reject`, or `noise` using explicit evidence.
- Preserve repository authority, human approval, localized parity, and independent operation outcomes.
- Reconcile delivery before an authoritative next-ready recommendation.
- Work in Git and non-Git repositories without depending on Git or Engram storage internals.
- Produce observable, testable outcomes without a new durable store.

## Non-Goals

- Implement generated behavior, tests, code, storage, or migration.
- Advance RH-W-0050 beyond `proposed` or approve implementation.
- Replace roadmap, current state, decisions, changelogs, localized peers, or concise Engram mirrors.
- Add automatic promotion, lifecycle mutation, conflict judgment, or historical deletion.
- Require SDD, network access, SQLite, a daemon, a scheduler, or another framework.
- Implement RH-W-0011, RH-W-0048, RH-W-0051, RH-W-0052, RH-W-0053, or RH-W-0054.

## Authority And Approved Constraints

- Repository files, applicable Git evidence, source, tests, and explicit approvals are canonical. Engram provides semantic continuity only.
- Every harness Engram operation uses explicit repository project scope and reports its own outcome.
- The harness investigates verifiable facts and asks only for human-owned decisions.
- Existing canonical owners remain roadmap, current state, decisions, changelogs, localized peers, and concise Engram mirrors.
- No duplicate backlog, lifecycle store, conflict store, receipt store, cumulative reconciliation document, trigger cache, scan receipt, framework, or ledger is permitted.
- Post-delivery reconciliation occurs before an authoritative next-ready recommendation.
- Behavior stays lightweight, organic, and bounded to relevant context.
- SQLite local-first is a current Engram implementation detail, not a harness dependency.
- `active` and `needs_review` are review-age labels, not truth states.
- `judgment_required` is advisory candidate discovery, not a proven contradiction.
- Plugin capture, prompt association, passive capture, compaction handoff, and sync require operation evidence before a success claim.

## Evidence Basis

Verified repository contracts establish that:

- The roadmap owns lifecycle and RH-W-0050 remains `proposed` with no dependency.
- Session start reads applicable owners, recovers relevant Engram context when available, reconciles recalled claims with repository evidence, and treats non-Git operation as locally actionable.
- Session stop verifies current evidence, reconciles changed lifecycle owners in one work unit, saves continuity in integrated mode, and separates required continuity from optional Git outcomes.
- Memory rules require explicit project scope and independent outcomes for project resolution, local read, local write, prompt association, conflict discovery, lifecycle review, plugin capture, and sync.
- A successful observation write remains successful when best-effort prompt association or later conflict discovery fails.
- `judgment_required` only reports candidate relations. `active` and `needs_review` are computed from review age and confer no truth or authority.
- Current Engram uses a local-first SQLite implementation, but the harness contract does not depend on SQLite.

These are supported operation contracts, not assumptions about private Engram internals. This design avoids brittle implementation line references and does not claim unverified plugin, capture, compaction, or sync behavior.

## Approaches

### A. Full Reconciliation At Every Start And Stop

Every invocation inventories all lifecycle records, projections, delivery evidence, relevant Git history, localized peers, and memory observations.

Tradeoffs: strongest routine coverage and simplest trigger logic, but highest cost, largest context, repeated no-op output, and pressure to create caches or receipts. It conflicts with lightweight bounded operation.

### B. Trigger-Only Reconciliation

Run no bounded scan. Enter the full cycle only when the user or an already-visible failure explicitly requests it.

Tradeoffs: lowest normal cost, but it cannot reliably discover false routing, incomplete delivery evidence, or owner changes. It delegates detection to humans and stale state.

### C. Hybrid Bounded Scan Plus Evidence-Triggered Full Cycle

Every start and meaningful stop performs a small owner-aware scan. A mandatory trigger enters the full cycle. Advisory signals narrow inspection or appear in output but never trigger alone. A no-trigger route continues normal behavior.

Tradeoffs: adds a classification boundary and focused contract tests, but avoids routine portfolio scans while detecting routing-critical drift.

**Recommended decision, not approved:** choose C. It is the smallest approach that satisfies post-delivery safety, repository authority, non-Git operation, and bounded context without durable trigger state.

## Architecture And Contract Ownership

| Owner | Contract |
|---|---|
| Session start | Resolve the repository, inspect relevant owners, perform the bounded scan, reconcile when mandatory evidence exists, then route conversationally. No mutation by default. |
| Session stop | Verify work, perform no-op or mandatory reconciliation, complete approved same-work-unit owner updates, save concise continuity, then handle optional Git gates separately. |
| Documentation skill | Select the smallest canonical owner, maintain maps and localized parity, reject duplicate stores, and validate links. |
| Memory rules | Define explicit scope, operation-level outcomes, advisory metadata, repository precedence, and evidence requirements for capture or sync claims. |
| Rulebook | Retain only never-miss gates to session and memory owners. Do not absorb detailed procedure. |
| Roadmap | Own RH-W lifecycle, advancement conditions, dependencies, and durable finding dispositions. |
| Current state | Project only concise active status, blockers, recent completions, and next actionable condition. |
| Decisions | Own approved durable constraints and reasons, not draft recommendations. |
| Changelogs | Record completed chronological work only. A draft design is not product delivery. |
| Repository owner | Approve disputed semantics, promotions, lifecycle changes, destructive cleanup, final design, and implementation. |

## Bounded Activation Algorithm

### Inputs

- Explicit selected repository and requested session action.
- Rulebook and applicable documentation map.
- RH-W records relevant to the objective and any unit reported as delivered or next-ready.
- Current-state projection, decisions, localized peers, and applicable changelog evidence.
- Available local source, tests, checker results, and Git evidence.
- Relevant explicitly project-scoped Engram results, with each operation retained separately.
- Explicit human request or decision evidence.

### Project And Owner Resolution

1. Resolve one repository root and confirm its rulebook belongs to it.
2. Resolve the canonical owner for each claim before comparing content.
3. If project resolution is ambiguous, stop reconciliation and request the one human-owned repository choice. Do not search or write Engram under a guessed project.
4. If a claim has no declared owner, retain it as an unclassified finding, which is a mandatory trigger.

### Bounded Scan

Inspect only:

- The requested unit and directly referenced lifecycle records.
- The concise current-state projection and any next-ready claim.
- Applicable localized peers for changed or conflicting claims.
- Delivery evidence for work represented as delivered since the last verified repository state.
- Relevant changed owner paths and checker output.
- Relevant project-scoped memory observations returned by a narrow query.

Do not scan the whole backlog, all commits, all memory, or all documentation unless a mandatory trigger requires the full cycle. Do not persist the scan.

### Trigger Classification

Classify each signal against the trigger table. Mandatory signals activate the full cycle. Advisory signals may narrow inspection or appear in output but do not activate the full cycle alone. Rejected signals are ignored unless separately verified mandatory evidence exists.

### No-Trigger Route

Return `reconciliation: not_triggered`, name the bounded evidence inspected, preserve independent owner and Engram outcomes, and continue normal start or stop routing. A stop with no changed lifecycle or delivery claim is a no-op reconciliation and writes no owner merely to record that nothing changed.

### Full Reconciliation Route

1. Inventory retained findings only from applicable evidence.
2. Resolve the owner and claim class for each finding.
3. Compare canonical repository evidence, localized presentation, delivery evidence, and relevant memory.
4. Classify each finding using the finding contract.
5. Apply only non-discretionary repairs already authorized by the task and repository policy.
6. Stop for owner approval at the exact boundaries below.
7. If approved changes occur, update affected canonical owners and localized peers in the same work unit.
8. Update only the concise existing Engram mirror when integrated continuity is available and the repository change is significant.
9. Re-run the bounded scan against refreshed evidence.

### Independent Outcomes

Keep these dimensions separate when applicable:

- Repository resolution and evidence.
- Reconciliation activation.
- Finding classification.
- Canonical owner updates.
- Localized parity.
- Delivery evidence.
- Next-ready routing.
- Git inspection or optional synchronization.
- Each Engram operation in the degradation matrix.

One failure does not erase an unrelated success.

### Routing After Reconciliation

- If all mandatory findings are classified and required owner updates are verified, next-ready routing may resume from roadmap and current state.
- If delivery evidence is incomplete or a mandatory finding remains unresolved, suppress authoritative next-ready recommendation. Report the blocked dimension and one remediation.
- Discussion may continue, but it must not represent a unit as ready or authorize unrequested work.

## Trigger Table

| Class | Signal | Required treatment |
|---|---|---|
| Mandatory | Post-delivery state | Run the full cycle before authoritative next-ready routing. |
| Mandatory | Canonical lifecycle inconsistency | Reconcile roadmap against every conflicting projection. |
| Mandatory | Changed lifecycle owner | Reconcile the changed RH-W record, current state, localized peer, and concise memory mirror in one work unit. |
| Mandatory | Incomplete delivery evidence | Classify the gap and suppress authoritative next-ready routing. |
| Mandatory | False routing risk | Reconcile when evidence could select work from a false status, dependency, priority, or advancement condition. |
| Mandatory | Explicit human request | Run the requested bounded or full reconciliation without expanding scope. |
| Mandatory | Verified repository-memory contradiction | Repository evidence wins; classify memory disposition and preserve operation evidence. |
| Mandatory | Unclassified retained finding | Enter the full cycle because durable routing cannot rely on an unowned finding. |
| Advisory | `judgment_required` | Inspect candidate evidence only when relevant; it is not contradiction proof. |
| Advisory | `needs_review` | Treat as review age only; verify the claim from its owner before use. |
| Advisory | Unrelated dirty Git state | Report and preserve it; do not infer lifecycle drift. |
| Advisory | Uninspected commit hints | Inspect only if relevant to a delivery or owner claim. |
| Advisory | Wording differences without semantic conflict | Maintain natural localization; repair only actual semantic drift. |
| Advisory | Stale summaries | Verify relevant claims; do not trigger on age alone. |
| Advisory | Checker drift not affecting lifecycle | Route to the checker owner without forcing lifecycle reconciliation. |
| Advisory | Ahead-only Git without delivery evidence | Report exact local evidence; do not infer delivery. |
| Rejected or noisy | Any dirty file | Dirtiness alone is not a trigger. |
| Rejected or noisy | Elapsed time | Age alone proves no contradiction. |
| Rejected or noisy | Every start or stop | Invocation alone is not a mandatory trigger. |
| Rejected or noisy | Broad memory result volume | Quantity is not relevance or contradiction. |
| Rejected or noisy | Treating `active` or `needs_review` as truth | Review-age metadata has no authority. |
| Rejected or noisy | Engram absence as repository drift | Continuity can fail while repository state remains valid. |
| Rejected or noisy | Commit keywords alone | Keywords do not prove delivery or lifecycle. |
| Rejected or noisy | Unavailable network state | Local reconciliation remains possible. |
| Rejected or noisy | Non-Git mode | Absence of Git is supported local operation, not drift. |
| Rejected or noisy | Unvalidated null search | A negative result needs a positive control or equivalent validation. |
| Rejected or noisy | Unverified plugin, capture, or sync channels | Never claim operation success without direct evidence. |

## Finding Classification Contract

| Classification | Meaning | Required evidence |
|---|---|---|
| `integrate` | A valid retained finding belongs in an existing canonical owner and affects durable work. | Verified relevance, identified owner, non-duplication check, evidence class, and human approval when content or authority is discretionary. |
| `close` | The finding is satisfied, superseded in its scope, or no longer actionable. | Current owner evidence proving the condition, plus delivery evidence when closure depends on delivery. Never close from memory alone. |
| `defer` | The finding remains valid but should not advance now. | Named blocker, prerequisite, or human-owned future condition, with an existing roadmap owner or approved update. |
| `reject` | The finding is understood and deliberately excluded. | Reasoned conflict with approved goals or constraints, or explicit owner decision. Preserve durable rejection in an existing owner. |
| `noise` | The signal has no durable product, lifecycle, or continuity value. | Evidence that it is duplicate, irrelevant, unsupported, merely advisory, or already represented canonically. Noise creates no record. |

Every retained finding has exactly one current classification. Classification does not authorize mutation, lifecycle advancement, deletion, or implementation.

## Evidence Precedence And Claim Classes

Precedence is contextual:

1. Explicit current human decisions own human authority and acceptance criteria.
2. The declared canonical repository owner governs its fact domain.
3. Verified local source, tests, checker output, and applicable Git evidence corroborate implementation and delivery.
4. Localized peers present canonical facts but do not override English factual owners.
5. Engram contributes semantic continuity and candidate relations only.
6. Unsupported summaries, hints, keywords, and unavailable channels provide no authority.

Each claim is one of:

- `verified_fact`: applicable, trustworthy, proportionate evidence directly supports it.
- `supported_inference`: named evidence and reasoning support it, and the report states what would verify it.
- `unknown_or_unverifiable`: no valid current mechanism or evidence exists.
- `human_decision_required`: authority or the deciding criterion belongs to a human.

Conflicting evidence is not averaged. Resolve the owner, preserve the conflict, and stop only the affected branch when authority is human-owned.

## Git And Non-Git Behavior

In Git repositories, use read-only status, tracked paths, history, ancestry, and local delivery evidence only when relevant. Dirty state is advisory. Ahead-only commits do not prove delivery. Network state is optional and cannot block local reconciliation.

In non-Git repositories, continue with repository files, checker output, source, tests, explicit approvals, and available project-scoped continuity. Mark Git-derived delivery claims `unknown_or_unverifiable` unless another approved mechanism supplies evidence. Non-Git mode does not activate reconciliation or lower repository authority.

## Engram Operation-Level Degradation Matrix

| Operation | Success evidence | Failure or absence treatment | Preserved partial success |
|---|---|---|---|
| Project resolution | Supported capability resolves the explicit repository project. | Stop Engram reads and writes under ambiguity; local reconciliation continues. | Repository and Git outcomes remain valid. |
| Read | Scoped read returns a valid result, including a validated empty result. | Mark memory read failed or unavailable; do not claim recovered continuity. | Project resolution and repository reconciliation remain valid. |
| Write | Scoped write confirms the observation or summary. | Mark continuity write failed; do not claim successful memory continuity. | Repository updates and prior reads remain valid. |
| Prompt association | Direct operation evidence confirms association. | Mark unknown or failed; do not downgrade a successful observation write. | Observation write remains successful. |
| Conflict discovery | Valid candidate response is returned. | Mark failed or unavailable; do not invent no-conflict. | Read or write remains successful. |
| Lifecycle review | Supported review operation returns review-age metadata. | Mark unavailable; verify claims from repository owners. | Reads, writes, and repository evidence remain valid. |
| Plugin capture | Plugin evidence confirms capture for this session and project. | Mark unverified or failed; never infer from installation. | Manual supported operations remain valid. |
| Sync | Supported sync evidence confirms the scoped result. | Mark unknown or failed; do not infer cross-device continuity. | Local Engram and repository operations remain valid. |

Engram absence degrades continuity only unless repository policy explicitly makes continuity required for the active workflow. It never proves repository drift and never causes a fallback harness store.

## Human Approval Boundaries And Exact Stops

Human approval is required for:

- Final approval of this design and any implementation authorization.
- Ambiguous repository or project selection.
- Choosing between genuinely conflicting canonical owners or acceptance criteria.
- Promoting a discretionary finding into durable documentation or lifecycle.
- Changing RH-W status, priority, dependency, advancement condition, or approved scope.
- Rejecting a valid product direction, deleting historical evidence, or destructive cleanup.
- Accepting a semantic localized exception rather than repairing it.

Stop only the affected branch when:

- Project or canonical owner resolution is ambiguous.
- A mandatory finding cannot be classified from available evidence.
- Required delivery evidence is incomplete.
- A verified repository-memory contradiction needs owner authority for disposition.
- Localized peers conflict semantically and canonical wording does not resolve it.
- An intended mutation exceeds approval or would touch unrelated work.

Do not ask about facts that source, tests, docs, checker output, or local Git can verify.

## Canonical Writes And Same-Work-Unit Reconciliation

- Lifecycle classification and advancement stay in `docs/repo-harness/roadmap.md`.
- Concise active projection stays in `docs/repo-harness/current-state.md` only when it changes.
- Approved durable rationale stays in `docs/repo-harness/decisions.md`.
- Completed chronology stays in `CHANGELOG.md`.
- Spanish peers update in the same work unit with equivalent semantics.
- Significant continuity gets one concise update to the existing project-scoped Engram topic or session summary when available.
- Source contracts remain in existing generator and reference owners named in the impact map.

Never create a reconciliation log, trigger cache, scan receipt, backlog copy, conflict store, delivery store, lifecycle store, or second design accumulation. This draft is the single design artifact for RH-W-0050 and should be updated in place through review.

## Post-Delivery Sequence

1. Verify what was delivered using applicable repository and local delivery evidence.
2. Run the full reconciliation cycle regardless of any other trigger.
3. Classify implementation findings and incomplete evidence independently.
4. Reconcile roadmap, current state, decisions or changelog when applicable, localized peers, and concise memory mirror in one work unit.
5. Verify links, parity, tests, checker status, and changed-file scope.
6. Re-run the bounded scan against refreshed owners.
7. Only when delivery evidence is complete and no mandatory finding remains unresolved may routing make an authoritative next-ready recommendation.

If evidence is incomplete, output `next_ready: suppressed`, identify the missing evidence, and continue only with discussion or remediation. Do not infer delivery from commit messages, ahead-only state, memory, issue text, or user wording alone.

## Observability And Output Shape

The report should be concise and organic while exposing independent outcomes:

```text
repository: <outcome and evidence class>
bounded_scan: <completed | failed> (<scope>)
reconciliation: <not_triggered | completed | paused | blocked | failed>
trigger: <mandatory signals or none>
findings: <counts and unresolved IDs by classification>
canonical_updates: <none | paths | paused | failed>
localized_parity: <not_applicable | verified | drift | paused | failed>
delivery_evidence: <not_applicable | complete | incomplete | unknown>
next_ready: <recommended unit | suppressed | not_requested>
git: <not_git | local outcome | optional branch outcome>
engram.project_resolution: <outcome>
engram.read: <outcome>
engram.write: <outcome>
engram.prompt_association: <outcome>
engram.conflict_discovery: <outcome>
engram.lifecycle_review: <outcome>
engram.plugin_capture: <outcome>
engram.sync: <outcome>
next_action: <one bounded action or human decision>
```

Only applicable fields need rendering. No aggregate Engram status may replace operation fields. Output is ephemeral, not a persisted receipt.
Angle-bracket values in the example are schema metavariables for implementation review, not unresolved design placeholders.

## Focused RED/GREEN Acceptance Scenarios

These are proposed implementation criteria. RED fails only because the named obligation is absent; GREEN uses the smallest contract change.

| Scenario | RED expectation | GREEN expectation |
|---|---|---|
| Normal no-trigger start | Current contract cannot prove bounded no-trigger behavior. | Relevant owners are scanned, `reconciliation: not_triggered`, and normal routing continues without writes. |
| No-op stop | Stop lacks an explicit no-op result. | No changed lifecycle or delivery claim creates no canonical write, changelog entry, memory restatement, or receipt. |
| Mandatory post-delivery cycle | Delivery can route directly to next-ready. | Full reconciliation always precedes authoritative next-ready routing. |
| Owner conflict | Conflicting canonical claims can be silently selected. | The affected branch stops with evidence and one human-owned decision request. |
| Localized conflict | Semantic Spanish drift can be treated as wording. | English owner wins; peer is repaired in the same work unit or routing pauses for a genuine exception. |
| Non-Git operation | Missing Git blocks or triggers reconciliation. | Local reconciliation continues, Git delivery is unknown when unavailable, and non-Git is not a trigger. |
| Advisory Engram signals | `judgment_required` or `needs_review` activates or proves conflict. | Signals remain advisory until repository verification establishes a mandatory trigger. |
| Verified repository-memory conflict | Memory can override or be ignored silently. | Repository wins, finding is classified, memory disposition is reported, and approval is requested only if required. |
| Partial Engram failures | One failed operation collapses all outcomes. | Successful write or read remains successful while other operations fail independently. |
| Engram absence | Missing Engram creates drift or fallback storage. | Continuity degrades according to policy, local reconciliation continues, and no duplicate store appears. |
| No duplicate store | Implementation can add a trigger cache, receipt, or ledger. | Contract tests reject every new durable reconciliation, trigger, conflict, receipt, or lifecycle store. |

Additional focused cases should cover incomplete delivery suppression, unclassified findings, explicit human request, unrelated dirty work, ahead-only Git, validated empty memory search, and unverified capture claims. They belong in the existing canonical test module, not a new framework.

## Rollback And Compatibility

The proposal is additive at the generated contract level. Existing start and stop outcomes, Git safety gates, localization, standalone mode, integrated mode, and canonical owners remain valid.

Implementation rollback would restore prior generated wording and focused tests together, then resynchronize existing runtime projections through the repository-owned renderer. Because no persisted schema or store is added, rollback needs no data migration. Existing Engram observations remain untouched. Older generated harnesses continue operating without RH-W-0050 guarantees until explicitly regenerated and approved through existing replacement semantics.

## Implementation Impact Map

This map names current stable symbols and generated contracts. It does not approve editing them.

| Stable symbol or path | Proposed impact |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill` | Generate bounded scan, trigger classification, independent outcomes, and post-reconciliation routing contracts. |
| `skill/repo-harness-builder/scripts/harness_tool.py::session_stop_skill` | Generate no-op stop, post-delivery cycle, same-work-unit reconciliation, and route suppression contracts. |
| `skill/repo-harness-builder/scripts/harness_tool.py::rule_outputs` | Extend existing memory or working-agreement detail only if needed. No new rule file by default. |
| `skill/repo-harness-builder/scripts/harness_tool.py::render` and `render_with_skill_mapping` | Preserve existing render ownership and output paths; add no mode or persisted input. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Add focused generated-contract RED/GREEN scenarios in the existing suite. |
| `skill/repo-harness-builder/references/architecture.md` | Document owner boundaries and activation rationale if implementation is approved. |
| `skill/repo-harness-builder/references/validation.md` | Add focused verification expectations if implementation is approved. |
| `.repo-harness/skills/repo-harness-session-start/SKILL.md` | Generated canonical consumer start contract. |
| `.repo-harness/skills/repo-harness-session-stop/SKILL.md` | Generated canonical consumer stop contract. |
| `.repo-harness/rules/memory.md` | Generated operation-level memory contract, changed only if clarification is approved. |
| `.repo-harness/rules/working-agreements.md` | Generated bounded-work contract, changed only if ownership analysis places detail there. |
| `.opencode/skills/repo-harness-builder/**` and `.claude/skills/repo-harness-builder/**` | Checked-in exact package projections of changed canonical builder files. |
| Runtime projections returned by `render(...)` | Existing generated destinations only; no new path or store. |
| Roadmap, current state, decisions, changelogs, localized peers, concise Engram mirror | Existing lifecycle and continuity owners, changed only when evidence requires. |

## Recommended Decisions Pending Approval

- Adopt approach C, hybrid bounded scan plus evidence-triggered full cycle.
- Use the mandatory, advisory, and rejected signal classification in this document.
- Make post-delivery an unconditional full-cycle trigger before authoritative next-ready routing.
- Keep observability ephemeral and operation-level, with no aggregate Engram verdict or persisted receipt.
- Implement through existing session, rules, references, render, and test owners only.

None of these recommendations is approved by this draft.

## Open Questions For External Review

1. Should a changed lifecycle owner always trigger the full cycle only for that RH-W record and direct projections, or should shared dependency changes expand it to direct dependents? Recommendation: include direct dependents only when the changed field can alter their routing, with a deterministic bound identified in review.
2. What exact evidence validates an empty Engram search across supported capability surfaces without coupling the harness to one implementation? Recommendation: require a supported operation receipt or a same-surface positive control, leaving adapter-specific proof to implementation review.
3. Should an approved documentation-only lifecycle correction count as post-delivery? Recommendation: yes when it changes a canonical lifecycle or next-ready claim; no for wording-only parity repair.

These questions do not permit lifecycle advancement. External review should resolve or narrow them before owner approval.

## External Review Checklist

- [ ] Status is unmistakably draft, pending external review and owner approval.
- [ ] RH-W-0050 remains `proposed`; no implementation or delivery is implied.
- [ ] Repository authority and Engram continuity boundaries are consistent.
- [ ] Approach C is recommended but not represented as approved.
- [ ] The bounded scan has deterministic scope and no persisted cache or receipt.
- [ ] Every mandatory trigger is necessary; advisory and rejected signals cannot trigger alone.
- [ ] Finding classifications are understandable and evidence-backed.
- [ ] Post-delivery suppression prevents false next-ready claims.
- [ ] Git and non-Git behavior are operable.
- [ ] Engram degradation preserves partial success and explicit project scope.
- [ ] Human stop conditions ask only human-owned decisions.
- [ ] Canonical destinations and localized parity avoid duplicate truth.
- [ ] Acceptance scenarios are focused, testable, and compatible with strict RED/GREEN work.
- [ ] Impact map names current symbols and paths without brittle line references.
- [ ] Rollback needs no new schema, migration, or historical deletion.
- [ ] Open questions are genuine and do not hide unresolved approved constraints.
- [ ] English and Spanish peers are semantically equivalent.
- [ ] No U+2013 or U+2014 characters, placeholders, contradictory approval wording, or hidden implementation authorization remain.
