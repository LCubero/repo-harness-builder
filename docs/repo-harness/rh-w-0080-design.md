# RH-W-0080 Ephemeral Batch Interaction Design

> **Status: OWNER-APPROVED DESIGN. A proposed implementation plan exists; implementation remains separately unauthorized.**

## Decision

An explicit user request may activate one ephemeral batch-interaction mode for one bounded objective in the selected repository. After inspect-first analysis, the harness may group only non-authorizing requirement clarifications that become discoverable from that analysis. It then executes only the work already authorized for that objective without file-by-file interruptions. Batch mode expires with the objective and never persists, broadens, groups, normalizes, supplies, infers, or replaces authority.

## Activation And Intake

Ownership is fixed by the current renderer architecture. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` owns explicit activation, inspect-first intake, grouped non-authorizing clarifications, and objective-bounded continuation. `::session_stop_skill()` owns expiry at pause, close, completion, abandonment, or objective replacement. `::rule_outputs()` owns the shared authority exclusions in working agreements so neither session skill duplicates or weakens them.

1. The user explicitly names batch mode and one bounded objective.
2. Inspect current repository evidence before asking questions.
3. Group only non-authorizing requirement clarifications discoverable from that inspection.
4. After the answers, execute the authorized objective without per-file confirmation.
5. Ordinary evidence gaps that can be resolved inside the repository are resolved without asking.

Approval and authority gates are NEVER grouped. External access, Git delivery, publication, destructive actions, secrets, builder preview or apply, exact digest or token approval, and newly discovered material product decisions remain separate one-question resumable gates.

Batch activation and grouped answers do not authorize previewed bytes or apply. They cannot supply or replace `APPROVE`, `DENY`, `APROBAR`, `DENEGAR`, a plan digest, a generation digest, refreshed evidence, or the exact active approval domain. The builder's digest-bound authority remains unchanged.

## Boundaries

- RH-W-0075 remains the sole owner of proving strict external necessity and justifying an external request.
- RH-W-0074 remains the sole owner of warn-once compliance after the user has answered.
- RH-W-0069 remains the sole owner of local and Engram continuity.
- RH-W-0068 remains the owner of changelog disposition during documentation closure.
- RH-W-0081 solely owns host-local human-facing changelog date derivation. Its chronology cannot be inferred from or control RH-W-0069 UTC backup identity.

No implementation, test, generated projection, changelog correction, Git action, network access, provider action, or publication is authorized by this design or the proposed plan.

## Verification Targets In The Proposed Plan

The proposed [implementation plan](rh-w-0080-implementation-plan.md) covers explicit activation, non-persistence, the narrow inspect-first clarification exception, uninterrupted in-scope execution, separate resumable authority gates, unchanged builder digest-bound authority, and every stop condition. It awaits owner approval.

## Done Conditions

- The bilingual design and lifecycle projections agree.
- RH-W-0080 remains `proposed` with an owner-approved design.
- No product behavior changes.
