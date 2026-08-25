# RH-W-0013 Minimal Context Budget Design

## Review Contract

Review this design against the canonical [RH-W-0013 roadmap record](roadmap.md#rh-w-0013), [Plan 16](roadmap.md#plan-16-rh-w-0013-measured-context-budgets), the immutable vendored [Gentle AI style guide](../../skill/repo-harness-builder/references/gentle-ai-style-guide.md), and current generator and checker behavior. The [Spanish peer](../es/repo-harness/rh-w-0013-design.md) must remain semantically equivalent.

Status is `ready`. This document approves no implementation, further lifecycle transition, commit, publication, or external access.

## Architecture Finding

Only `REPO-HARNESS.md` is always loaded. Files under `.repo-harness/rules/` and the three daily skills are conditional routes. RH-W-0013 must not impose runtime budget reporting on those conditional surfaces or refactor them as if they were always loaded.

The existing `token_proxy()` and `RULEBOOK_TOKEN_BUDGET = 700` remain the complete measurement mechanism. Existing template tests continue to keep the generated rulebook and rule files below 700 during development.

## Decision

The owner-approved bands remain documentation guidance:

| Estimated tokens | Guidance |
|---:|---|
| 180 through 450 | Target |
| 451 through 700 | Acceptable |
| 701 through 1000 | Warning |
| 1001 through 1200 | Priority attention |
| Above 1200 | Immediate review advice |

No band blocks rendering, changes checker status, changes file state, or creates runtime metadata. RH-W-0067 owns response-rule placement, its gate row, and rulebook headroom.

## Implementation Boundary

Implementation changes exactly two effects in `skill/repo-harness-builder/scripts/harness_tool.py`:

1. Remove the budget-specific `require()` loop from `render_with_skill_mapping()` so an output at exactly 700 or above can render.
2. Remove the `over-budget` branch from `check()` so a structurally valid human-owned file remains `human-evolved` when its bytes differ, or `current` when they match.

Remove `budgeted()` only if the implementation proves no caller remains after both changes. Keep `token_proxy()`, `RULEBOOK_TOKEN_BUDGET`, and the development-time rulebook and rule-file budget tests.

## Explicit Exclusions

- No runtime report or schema.
- No `contextBudget` or threshold field.
- No Markdown parser, section ranking, or recommendation identifiers.
- No preview or apply propagation.
- No generated references or daily-skill refactor.
- No manifest, generated checker, command, prompt, or wrapper change.
- No change to the vendored style guide.
- No RH-W-0067 response-rule placement or headroom work.

## Required Evidence

RED must prove both current defects:

- A rendered rulebook whose `token_proxy()` is exactly 700 fails in `render_with_skill_mapping()` with the current budget `ContractError`.
- A human-evolved budgeted file at or above 700 becomes `over-budget` and makes `check()` report `drift`, instead of remaining `human-evolved` with overall status `current`.

GREEN must prove only the intended changes:

- Exact-700 and exact-701 fixtures render without a budget refusal.
- The same human-evolved fixture remains `human-evolved`, overall `current`, with `writes: 0`.
- Real section, locale, generated-byte, attribute, missing-file, and indirect-path failures retain existing behavior.
- Existing template budget tests for the rulebook and rule files still pass.
- The canonical script and tests are byte-identical to their OpenCode and Claude projections.

## Remaining Human Decisions

No product decision remains. Implementation and lifecycle advancement still require separate authorization and evidence.
