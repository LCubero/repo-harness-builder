# RH-W-0054 Material Correction Invalidation Design

> **Status: OWNER-APPROVED DESIGN. Implementation is pending.**
>
> The owner approved the behavior decisions represented here on 2026-08-21. Design approval advances RH-W-0054 to `ready`; it does not authorize or claim implementation or delivery.

## Review Contract And State Labels

Review this design against the canonical [RH-W-0054 roadmap record](roadmap.md#rh-w-0054), the existing digest-bound preview and apply safety model, and the completed [RH-W-0050 bounded reconciliation baseline](rh-w-0050-design.md). Repository evidence is canonical. The [Spanish peer](../es/repo-harness/rh-w-0054-design.md) must remain complete and semantically equivalent.

- **Approved constraint:** already authorized by repository policy or existing executable safety behavior.
- **Verified current behavior:** directly observed in current repository evidence and not introduced by this design.
- **Approved design decision:** owner-approved for future implementation by RH-W-0054.
- **Remaining implementation detail:** intentionally deferred until an approved implementation task.

## Problem, Goals, And Non-Goals

A preview can derive claims, selected arguments, outputs, a plan, a digest, and an approval boundary from interview answers, human decisions, assistant interpretations, and repository evidence. When a material premise is later corrected, preserving any authority derived from the old premise can execute work the human did not freshly review.

The approved design makes the first repository write the correction boundary. Before that write, every material correction invalidates the affected preview generation's complete plan, digest, and approval, even when recalculation would produce byte-identical files and actions. The flow preserves old evidence only as history, re-establishes required factual or human authority, recalculates from current inputs, and presents a fully fresh preview for fresh approval.

After any apply write, authority invalidation cannot undo the mutation. A material correction discovered then fails RH-W-0054 closed, reports that the current repository state requires fresh verification, and routes to a separate remediation or recovery workflow. It never silently rolls back, auto-corrects, reuses approval, or claims that invalidation recovered repository state. No existing record explicitly owns this post-apply workflow, so assigning its ownership is a separate future design decision. RH-W-0053 remains limited to interrupted-session recovery.

This design does not implement behavior, tests, storage, migration, session interruption recovery, or cross-session correction state. It adds no lifecycle store, receipt, ledger, cache, duplicate authority, or automatic repository mutation. Persistent interruption recovery belongs to RH-W-0053. Harness-owned storage semantics belong to RH-W-0051.

## Authority Model

| Authority | Role |
|---|---|
| Repository evidence | Canonical source for current repository facts and executable inputs. |
| Human decision | Current authority for human-owned choices after an explicit decision or correction. |
| Preview generation | Ephemeral authority boundary that binds the complete derived preview to the inputs and evidence used to create it. |
| Plan and digest | Executable guards inside one valid preview generation. They do not survive material correction. |
| Approval | Human authority for exactly one current preview generation and its bound plan and digest. |
| Prior evidence and previews | Historical context only after invalidation; never current authority. |

Existing plan, digest, and byte checks remain the final executable guards. RH-W-0054 adds an earlier authority rule: an invalidated generation cannot reach those guards as an approved current generation, even if its bytes, actions, plan, or digest happen to equal a newly calculated result.

## Definitions

| Term | Normative definition |
|---|---|
| Material correction | A correction that changes or challenges an input, claim, decision, interpretation, or repository fact from which any preview result or execution authority was derived. Materiality is about derivation and authority, not whether final bytes differ. |
| Irrelevant correction | A correction proven to have no dependency path to any answer, claim, selected argument, derived output, plan element, digest input, or approval in the current preview generation. |
| Preview generation | One ephemeral derivation cycle from classified current inputs and repository evidence through a complete preview, plan, digest, and approval boundary. |
| Affected set | The corrected input plus every transitively dependent answer, claim, selected argument, derived output, and plan element in the current generation. |
| Invalidation | Before the first repository write, removal of current authority from the affected generation's complete plan, digest, and approval. It does not delete history or reverse a mutation. |
| Fresh preview | A complete preview recalculated in a new generation after required facts and decisions are current. It is not a replay or re-labeling of the old preview. |

## Correction Classification

Every correction is classified before recalculation:

| Input class | Required authority recovery |
|---|---|
| Factual claim | Re-verify using applicable current evidence. If verification is unavailable or contradictory, block the affected flow and name the missing evidence. |
| Human decision | Obtain a new explicit human decision. The corrected statement alone is not reusable approval for the resulting preview. |
| Assistant interpretation | Return to the underlying verified facts and human decisions, replace the interpretation, and recalculate every dependent result. |
| Repository evidence change | Re-read the applicable current repository evidence and recalculate from it. A prior snapshot remains history only. |

Classification does not itself authorize recalculation or execution. Recalculation begins only after every affected factual claim is re-verified and every affected human-owned choice has a current explicit decision.

## Dependency-Aware Invalidation

Before the first repository write, the correction flow uses scoped, ephemeral dependency awareness inside the current session and preview generation:

1. Identify the corrected input and classify it.
2. Trace its dependencies through affected answers, claims, selected arguments, derived outputs, and plan elements.
3. If no dependency path exists, classify the correction as irrelevant to the plan and preserve the current generation.
4. If any dependency path exists, classify the correction as material.
5. Invalidate the complete plan, digest, and approval for that affected preview generation.
6. Preserve the old inputs, evidence, preview, plan, digest, and approval only as historical context.
7. Re-verify affected facts or obtain new affected human decisions.
8. Recalculate all dependent answers, claims, selected arguments, outputs, and the complete plan in a new preview generation.
9. Produce a fresh digest and a fully fresh preview.
10. Require fresh approval before apply.

Invalidation is scoped by dependency when deciding whether a correction is material. Once a correction is material to a preview generation, plan, digest, and approval invalidation is generation-wide. Partial plan approval, digest carry-forward, and approval salvage are forbidden because they would create two authorities inside one executable preview.

## State Transition Contract

| Current state | Event | Required next state |
|---|---|---|
| Interview or derivation before preview | Material correction | Mark affected derived state non-authoritative, recover required authority, and continue only through a new generation. |
| Preview produced, not approved | Material correction | Invalidate the generation's complete plan and digest; recover authority and produce a fresh preview. |
| Preview approved, not applied | Material correction | Invalidate plan, digest, and approval immediately; recover authority and require fresh preview plus fresh approval. |
| Preview or approval exists | Proven irrelevant correction | Preserve the current generation and record no new executable authority. |
| Repository evidence changes after preview | Applicable change | Treat as a material repository evidence correction and invalidate the generation before apply. |
| Old plan, digest, or approval is presented after invalidation | Reuse attempt | Reject it as stale regardless of byte or action equality. |
| Fresh preview receives fresh approval | Apply requested | Run the existing final plan, digest, byte, and material-change guards. |
| Any apply write has occurred | Material correction | Fail RH-W-0054 closed, report that current repository state requires fresh verification, and route to a separately owned remediation or recovery workflow without claiming rollback or recovery. |

No correction state survives the session. If the session ends before a fresh preview is approved, RH-W-0054 provides no resumable checkpoint. Any future persistent interruption workflow must be designed by RH-W-0053 using RH-W-0051 storage semantics.

## Result States And Output

The correction flow reports only ephemeral current-session outcomes:

| Result | Meaning |
|---|---|
| `irrelevant` | No dependency path to the current preview generation was found; current plan authority is unchanged. |
| `invalidated` | A material correction removed authority from the complete plan, digest, and approval for the affected generation. |
| `blocked` | Required factual evidence or a required human decision is unavailable. |
| `fresh_preview_required` | Authority recovery is complete and recalculation must produce a new complete preview. |
| `fresh_approval_required` | A fresh preview exists but has no approval for its generation. |
| `ready_for_existing_guards` | Fresh approval exists and apply may proceed through all existing executable guards. |
| `post_apply_remediation_required` | A material correction was discovered after an apply write; RH-W-0054 cannot recover the mutated repository state and a separate workflow must freshly verify it. |

An implementation may choose exact field names, but its output must identify the correction class, affected set, invalidated generation, authority recovery outcome, fresh generation, and next required boundary. This output is not persisted as a receipt or ledger.

## Failure And Safety Boundaries

- Missing or contradictory factual evidence blocks only the affected correction flow and never converts an old preview into authority.
- A missing human decision pauses before recalculation of dependent outputs.
- Failure to compute the affected set fails closed by treating the correction as material to the current preview generation.
- A repository evidence change discovered by existing pre-apply checks invalidates the generation before any write.
- After any apply write, a material correction fails RH-W-0054 closed. It reports the need for fresh repository verification and separate remediation or recovery without rollback, automatic correction, reused approval, or a false recovery claim.
- Byte-identical or action-identical recalculation never restores old approval. Equality can confirm output stability, not continuity of human authority.
- Historical evidence is preserved without becoming an alternate current input store.
- No automatic correction, silent re-preview, automatic approval, or repository mutation is permitted.

## Required RED/GREEN Test Contract

| Scenario | RED defect | GREEN outcome |
|---|---|---|
| Correction changes output bytes or actions | Old preview or approval remains usable | Old generation is invalidated; changed result appears only in a fresh preview requiring fresh approval. |
| Correction yields identical bytes and actions | Equality preserves old approval | Old generation is invalidated; identical result still requires a fresh preview and fresh approval. |
| Correction is irrelevant to plan | Whole generation is invalidated without a dependency | Irrelevance is proven from the ephemeral dependency set and current authority remains unchanged. |
| Correction changes a human decision | Corrected text is treated as both decision and approval | A new explicit decision precedes recalculation, and fresh preview approval remains separate. |
| Repository evidence changes after preview | Apply relies only on the old snapshot | The generation is invalidated, current evidence is re-read, and a fresh preview is required. |
| Old digest or approval is reused | Stale authority reaches apply because bytes match | Reuse is rejected before execution regardless of equality. |
| Corrected flow succeeds | Apply resumes from partial old authority | Success occurs only through complete fresh preview, fresh approval, and existing final guards. |
| Material correction is discovered after an apply write | RH-W-0054 silently rolls back, auto-corrects, or reuses prior authority | RH-W-0054 fails closed, reports that current repository state requires fresh verification, and routes to a separate remediation or recovery workflow. |
| Post-apply routing is reported | Invalidation is described as undoing the write or recovering repository state | Output explicitly states that no rollback or recovery occurred and that ownership of the separate workflow remains a future design decision unless another canonical owner is approved. |

Focused coverage must also prove history preservation, blocked authority recovery, failure-closed dependency analysis, session-local ephemerality, post-apply correction routing, absence of a false rollback or recovery claim, and absence of any new store, receipt, ledger, cache, or duplicate authority.

## Implementation Impact Map

This map identifies future work only. It authorizes no edit.

| Stable symbol or path | Required future change |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` preview and apply flow | Introduce preview-generation identity, correction classification, ephemeral dependency tracking, pre-write generation-wide invalidation, fresh-preview routing, stale-generation rejection, and failure-closed post-apply routing without persistence or a recovery claim. |
| Existing plan and digest validation | Preserve as final executable guards and require that their generation still has current fresh approval. |
| Repository evidence checks before apply | Route applicable post-preview evidence changes through material correction invalidation before writes. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Add the complete RED/GREEN contract, including identical-result invalidation, irrelevant-correction preservation, post-apply routing, and proof that no rollback or recovery is falsely claimed. |
| Product behavior documentation | Document correction intake and fresh approval boundaries without claiming cross-session recovery. |
| Runtime projections | Synchronize only after canonical implementation through existing ownership and digest-bound maintenance behavior. |

## Remaining Implementation Details

Exact internal identifiers, dependency representation, correction command or conversational surface, output field names, and fixture construction remain implementation details. They may not weaken pre-write generation-wide plan, digest, and approval invalidation after a material correction, separate decision and approval authority, fresh-preview requirements, existing final guards, failure-closed post-apply routing, or the prohibition on persisted RH-W-0054 state and false rollback or recovery claims.

## Evidence Matrix

| Claim | Evidence class | Owner | Treatment |
|---|---|---|---|
| RH-W-0054 is `ready` and implementation is pending | Owner-approved lifecycle decision | `roadmap.md#rh-w-0054` | Advanced by this approved design only. |
| Material correction invalidates approval even for identical results | Owner-approved design decision | This design | Required. |
| Dependency awareness is scoped and ephemeral | Owner-approved design decision | This design | Required. |
| Post-apply invalidation cannot undo repository mutation | Owner-approved design decision | This design | Fail closed and route to separate remediation or recovery. |
| Ownership of post-apply remediation is not assigned | Human decision required | Future design | RH-W-0053 is not assigned without explicit repository evidence. |
| Existing digest, plan, and byte checks remain final guards | Approved constraint and verified current behavior | Existing preview and apply model | Preserved. |
| Cross-session interruption recovery belongs to RH-W-0053 | Approved scope boundary | `roadmap.md#rh-w-0053` | Preserved. |
| Storage semantics belong to RH-W-0051 | Approved scope boundary | `roadmap.md#rh-w-0051` | Preserved. |
| No new authority or persistence store is introduced | Owner-approved design decision | This design | Required. |
