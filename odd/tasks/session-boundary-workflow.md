# Session Boundary Workflow

## Objective
Record the repository-local agreement to handle one coherent feature, blocker, issue, or named objective per working session, then close continuity before starting the next unit in a fresh Pi session.

## Scope
- Add the agreement to the repository working agreements.
- Explain the start/work/stop loop in the English and Spanish usage guides.
- Preserve existing documentation, verification, continuity, authority, and Git boundaries.

## Non-Goals
- No forced Pi shutdown, process control, automatic resume, or active-session lock.
- No requirement that every objective have a remote provider issue.
- No new continuity store, backup format, checkpoint, Engram behavior, generated session contract, or product behavior.
- No automatic documentation, Git, delivery, design, or implementation authority.

## Tasks
- [x] **SBW-T1 — Record the local working agreement.** Add one bounded project agreement for one coherent objective per session and explicit session-stop before a fresh session-start.
- [x] **SBW-T2 — Update human workflow guidance.** Reconcile the English and Spanish daily workflow descriptions.
- [x] **SBW-T3 — Verify and prepare continuity close.** Six focused tests passed; the checker reported `current` with zero writes; `git diff --check` passed; the budget was 514.71 proxy tokens; and native review `review-b9a3686f86f821bf` was approved and acknowledged with one informational warning that opened no correction.

## Acceptance Criteria
- The policy is repository-local and does not claim generic consumer behavior.
- A session may contain the documentation, verification, review, and delivery required to finish its one objective.
- Adjacent work is deferred to a fresh session after session-stop.
- English and Spanish guidance are semantically aligned.
- The checker reports `current` with zero writes and `git diff --check` passes.

## Next Step
Run `repo-harness-session-stop`, close Pi, and resume the next named objective in a fresh session through `repo-harness-session-start`.
