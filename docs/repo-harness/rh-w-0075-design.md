# RH-W-0075 External Necessity And Justification Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Decision

External access includes paths outside the selected repository, sibling repositories, network access, and provider tooling. Before asking for any of them, the harness must prove that the bounded objective cannot be completed inside the selected repository.

If strict necessity is proven, one request states the exact target, why it is necessary, its intended use, and the consequence of decline. If the objective is solvable inside the repository, the harness does not ask.

## Boundaries

- Existing approval gates remain unchanged. Necessity and justification do not grant access.
- RH-W-0074 governs what happens after the consumer answers and is not merged into this lifecycle.
- Batch mode under RH-W-0080 cannot bypass this owner.
- No new permission store, broker, telemetry, remembered authority, or automatic denial is introduced.

## Later Verification Targets

The proposed [implementation plan](rh-w-0075-implementation-plan.md) covers every external class, strict necessity evidence, the four required request fields, no request for repository-local solutions, decline behavior, and unchanged approval gates. It awaits owner approval and authorizes no implementation.
