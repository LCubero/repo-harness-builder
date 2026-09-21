# RH-W-0095 Contextual Acknowledgement Design

> **Status: IMPLEMENTED, INDEPENDENTLY VERIFIED, AND `completed`. Git delivery remains separate, unauthorized, and unclaimed.**

## Review Contract

The English design is canonical; the [Spanish peer](../es/repo-harness/rh-w-0095-design.md) must remain semantically equivalent. Review the implemented outcome against the [roadmap record](roadmap.md#rh-w-0095), RH-W-0067 response ownership, RH-W-0083 composition-time loading, RH-W-0049 organic continuation, the charter and working agreements, and the existing pending-gate rule.

## Decision And Sole Owner

After a brief acknowledgement that contains no explicit action and target, preserve the immediately active conversational subject and state one sensible next discussion or action direction. The acknowledgement is neither approval nor authority to plan, delegate, mutate, deliver, or continue automatically.

RH-W-0067's generated `.repo-harness/rules/response-contract.md` remains the sole response-contract owner. RH-W-0095 adds only this bounded semantic clause there; it creates no second owner.

## Precedence And Authority

| Situation | Required result |
| --- | --- |
| Current turn explicitly names an action and target | Existing authorization rules apply; `ok` adds no authority. |
| A human gate is pending | The gate remains pending under its existing contract; acknowledgement does not answer, approve, dismiss, repeat, or displace it. |
| Acknowledgement only, active subject recoverable | Preserve that subject and suggest one sensible direction; execute nothing. |
| Comparison or discussion is active | Continue that discussion without selecting an option for the human. |
| No subject is recoverable | Invent nothing; ask at most one minimal clarification only if continuation requires it. |
| Later turns | Infer no preference, remembered instruction, reusable consent, lifecycle decision, or authorization. |

Existing authority remains unchanged: `REPO-HARNESS.md` owns authority and no-unrequested-work; `working-agreements.md` owns stopping and proportionality; RH-W-0049 owns organic discussion; RH-W-0083 owns immediate-before-composing loading; the pending-gate rule owns pending questions.

## Behavior Examples

| Context and input | Expected boundary |
| --- | --- |
| Active design discussion; `ok` | Briefly acknowledge, retain the design subject, and suggest one useful next discussion; do not plan or edit. |
| A decision gate is pending; `ok` | Leave the gate pending; do not treat `ok` as its answer or start other work. |
| Comparing two approaches; `ok` | Preserve the comparison and suggest one material dimension to examine next; choose neither. |
| No recoverable conversational context; `ok` | Do not invent a topic; if continuation is necessary, ask one minimal clarification. |
| `ok, implement RH-W-0095` | Route the explicit action and target through existing authorization rules; `ok` itself contributes nothing. |
| Nothing is pending after a completed explanation; `ok` | Use plain status plus one useful direction tied to the active subject, not an empty block, fixed menu, or backlog dump. |

## Implemented Surfaces

- **Canonical seed and generated owner:** one concise response-contract bullet in `skill/repo-harness-builder/scripts/harness_tool.py` and `.repo-harness/rules/response-contract.md`.
- **Canonical tests:** focused assertions in the existing `RuleFileTests` and `QuestionVisibilityTests` in `skill/repo-harness-builder/tests/test_harness_tool.py`.
- **Declared package projections:** canonical script and test projections in `.claude/skills/repo-harness-builder/`, `.opencode/skills/repo-harness-builder/`, and `.pi/skills/repo-harness-builder/`.
- **Unchanged ownership:** no runtime-specific response-contract copy or daily-skill edit was added.

## Verification Evidence

- Strict RED: `RuleFileTests.test_contextual_acknowledgement_has_one_generated_response_contract_owner` failed before source changes with `AssertionError: 0 != 1`; an independent focused rerun then passed all 7 exact tests with 0 skipped in 0.003s.
- Maintenance: the unapplied `english` preview exposed 13 replacements and digest `d8ed32e...`; the manifest-aligned `espanol` preview contained exactly six Claude/OpenCode/Pi script/test projections, and digest `8d51b4782cf0eb2ba5723ed7239948426e34c9303c5bdb081f82c5314dcc6d0f` applied `VALIDATED`.
- The checker reported `current`, exit 0, and 0 writes. Exact canonical/Claude/OpenCode/Pi byte parity passed for both script and test files.
- The initial non-isolated full run collected 559 tests and failed five checks: one stale documented 558 count and four direct/nested Git-fixture failures caused by unrelated ancestor repository `C:/Users/LD9/.git`. After updating both collection counts and applying process-local `GIT_CEILING_DIRECTORIES=C:/Users/LD9/AppData/Local/Temp`, the three direct Git selectors passed and the full suite passed 559 tests with 10 skipped, exit 0, in 205.400s. No persistent environment change was made.
- The rendered response-rule token proxy is 698 against budget 700. `git diff --check` passed, and pre/post status was identical.

These tests verify the generated instruction contract; they do not guarantee compliance by an arbitrary model. Native final review remains parent-owned and is not a durable lifecycle or delivery claim.

## Rollback

Remove the bounded clause and its focused assertions from canonical source/tests, regenerate only declared projections and the generated owner, then reconcile lifecycle documentation. No state cleanup or migration applies.

## Exclusions And Decisions

No classifier, parser, detector, phrase allowlist, state machine, formatter, middleware, runtime refusal, preference axis, RH-W-0018 reopening, persistence, telemetry, analytics, compliance log, live observer, schema, manifest, config, migration, dependency, external component, runtime-specific response-contract copy, daily-skill duplication, fixed menu, backlog dump, automatic execution, or second response-contract owner.

Remaining design decisions: **none**. Implementation and independent verification are complete; issue, branch, commit, push, pull request, merge, release, and any live-model guarantee remain unauthorized and unclaimed.
