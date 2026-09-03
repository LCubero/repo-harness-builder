# RH-W-0083 Proposed Scope

**Status: proposed; bilingual scope owner-approved as written.** This document defines the approved correction boundary. Scope approval authorizes no product implementation, test change, generated projection change, lifecycle advancement, or delivery; implementation and focused test changes are deferred to another session and require separate explicit authorization.

## Immediate Obligation

Require each generated daily skill, in its Output Contract, to load and apply `.repo-harness/rules/response-contract.md` immediately before composing any reply or reported output that carries a notice, pending action, question, approval, visible next action, or contextual explanation.

The wording must reinforce the existing owner at the action point. It must not copy either 32-byte form, the setext warning, or any other response syntax into a daily skill. `RH-W-0067` remains the sole complete response-contract owner.

## Acceptance Tests

Future implementation is limited to focused development-time assertions that:

1. fail before the correction because every generated daily Output Contract lacks the immediate-before-composing obligation;
2. pass only when each generated daily Output Contract carries that obligation exactly once and still names the canonical rule path;
3. keep `test_daily_contracts_do_not_duplicate_response_syntax` green, preserving its focused check that prohibited response-syntax phrases do not appear in the three generated daily skills; and
4. keep `test_response_syntax_has_one_generated_owner` green, preserving its focused check that `Decision required` appears in only the generated response-contract owner.

No live-model evaluation or runtime reply observer is required. The tests prove the generated instruction contract, not that an arbitrary agent will always obey prose.

## Boundaries

- No change to `REPO-HARNESS.md` or `.repo-harness/rules/response-contract.md`.
- No reply classifier, scorer, formatter, runtime refusal, telemetry, or persistent compliance state.
- No duplication into `RH-W-0076`, whose three-defect scope remains frozen.
- No product source, product test, generated skill, manifest, checker, runtime projection, or delivery change under scope approval alone.

## Evidence And Lifecycle

Verified repository fact: the canonical roadmap records the response-contract reference after Execution Steps, inside each daily Output Contract, and records that existing assertions inspect generated text and ownership rather than replies. It also retains the later approval-contract mixing occurrence as evidence in the same root cluster without creating a new work item. Supported inference: reinforcing the same owner immediately before the action point is the smallest correction because the trigger is known only while composing the reply; implementation evidence would raise this to a verified product result. Verified owner decision: this bilingual scope is approved as written, while implementation and focused test changes are deferred to another session and remain separately unauthorized.

`RH-W-0083` remains `proposed`. Scope approval does not make it `ready`, start implementation, or claim completion or delivery.
