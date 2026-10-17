# RH-W-0080 Ephemeral Batch Interaction Design

> **Design status: completed by conditional owner lifecycle acceptance on 2026-10-07 after all stated criteria passed; see the canonical [roadmap](roadmap.md#rh-w-0080). Historical design/plan approval did not authorize implementation; separate implementation authority was granted later. Product delivery through PR #269 (4dc5cf3) is complete.**

## Decision

An explicit user request may activate one ephemeral batch-interaction mode for one bounded objective in the selected repository. After inspect-first analysis, the harness may group only non-authorizing requirement clarifications that become discoverable from that analysis. It then executes only the work already authorized for that objective without file-by-file interruptions. Batch mode expires with the objective and never persists, broadens, groups, normalizes, supplies, infers, or replaces authority.

## Activation And Intake

Ownership is fixed by the current renderer architecture. `skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` owns explicit activation, inspect-first intake, grouped non-authorizing clarifications, and objective-bounded continuation. `::session_stop_skill()` owns expiry at pause, close, completion, abandonment, or objective replacement. `::rule_outputs()` owns the shared authority exclusions in `.repo-harness/rules/batch-interaction.md` so neither session skill duplicates or weakens them.

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

Historical authority boundary: this design and the proposed plan did not authorize implementation when approved. Separate implementation authority was granted later; no commit, publication, or lifecycle completion is implied.

## Verification Targets In The Proposed Plan

The approved [implementation plan](rh-w-0080-implementation-plan.md) records the bounded contract and execution history. Detailed implementation and verification evidence is in the local task record `odd/tasks/rh-w-0080-bounded-intake.md` (not tracked in Git); it does not claim live-agent compliance or measured savings.

## Historical Design-Time Done Conditions (superseded by lifecycle acceptance)

- The bilingual design and lifecycle projections agree.
- RH-W-0080 remains `proposed` with an owner-approved design.
- No product behavior changes.
