# RH-W-0075 External Necessity And Justification Design

> **Status: owner-approved design, implemented in its minimal form on 2026-10-01 (see below) and delivered through PR #257.**

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

## Minimal Implementation (2026-10-01)

On 2026-10-01 an agent wrote to the system temp by habit, which on Windows is outside the repository, and listed an outside path without saying why, although `.repo-harness/tmp/` was available. The owner delegated the scope. Two generated lines answer the original direction ("stay in the repository; when asking, say why and what for"):

1. The rulebook hard rule becomes: "Ask for explicit approval before reading or modifying paths outside it. Say why and what for."
2. Generated working agreements add: "Put temporary files in `.repo-harness/tmp/`; use the system temp only when that fails or a task needs a location outside Git."

The four-field request contract, necessity proofs, and batch-mode coupling in the proposed implementation plan are superseded: they add process without a present failure they would catch. RH-W-0074 stays separate.
