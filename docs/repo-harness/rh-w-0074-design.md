# RH-W-0074 Accepted Omission And Completion Design

> **Status: owner-approved design, implemented in its minimal form on 2026-10-01 (see below) and delivered through PR #259.**

Canonical owner: this document. [Spanish peer](../es/repo-harness/rh-w-0074-design.md). Lifecycle: [RH-W-0074](roadmap.md#rh-w-0074).

## Accepted Direction

End a conversation without pretending every obligation was fulfilled. Warn once about the concrete consequence, respect the user's explicit decision within authorized scope, and preserve each operation's actual result. Do not ask again for an unchanged decision already supplied. No global exception-success state is needed.

`completed` still requires successful verification and every applicable required task, documentation reconciliation, and required continuity step to have succeeded. Declining a required task does not make it optional or `not_applicable`, complete the work unit, or advance its roadmap lifecycle.

## Proposed Reconciliation With RH-W-0068

**Verified fact:** the [implemented closure owner](rh-w-0068-documentation-closure-design.md#closure-outcomes) forbids `completed` for required documentation that is declined, pending, failed, or incomplete, and forces `paused` or `blocked`. The installed session-stop contract also requires a close outcome while defining `paused` and `blocked` by actual missing input or prerequisites. Those requirements cannot remain unchanged for an accepted omission with nothing awaiting a decision.

**Owner-approved amendment:** this is a narrow classification/output change, not an exception to completion eligibility:

- Keep RH-W-0068's ordered verification/documentation/continuity gate, applicable-surface obligations, and prohibition on fabricated completion.
- Replace only the forced `paused`/`blocked` classification for an accepted omission. Report required documentation as `declined`, preserve verified operation results, and state in ordinary prose that the conversation ends with the work unit not completed. Do not emit an aggregate success or invent a new aggregate status. Adjust the session-stop output requirement explicitly to permit this scoped report instead of a mandatory aggregate close label in that case.
- Use `paused` only for actual pending human input; use `blocked` only for an actual missing prerequisite. Failed verification or another attempted operation remains `failed`; ending the conversation does not erase that result.

Until this design is separately authorized for implementation and implemented, RH-W-0068 and the generated contract remain binding. Design approval records the amendment but does not activate or silently supersede it.

## Small Acceptance Cases

| Evidence and decision | Proposed report |
|---|---|
| Every applicable required obligation succeeds | `completed`, without an exception |
| Verified work; user explicitly declines required documentation after accepting the stated consequence; no other input is pending | Preserve verified work; documentation `declined`; conversation ends, work unit not completed; no repeated consent request or aggregate success label |
| Verification fails, even if the user asks to stop | Verification `failed`; no completion or invented successful proof |
| Approval or a prerequisite is genuinely missing | Scope `paused` or `blocked` to that actual missing input or prerequisite; do not infer it from a decline alone |

These are design criteria, not executed tests or proof of live-model compliance.

## Smallest Future Change And Boundaries

If separately authorized, amend only the existing working-agreements stopping guidance and session-stop closure/output clauses, their focused contract assertions, and directly affected generated projections. Reconcile the RH-W-0068 design and durable decision summary with the approved wording in that same implementation unit. No new rulebook gate or rulebook bytes are needed.

No new schema, ledger, runtime, parser, state store, automatic override, or global outcome is proposed. External access, destructive actions, provider changes, publication, and digest-bound builder authority retain their existing separate approval boundaries. RH-W-0075 retains pre-request necessity; RH-W-0068 retains closure ordering. This design neither changes the approved RH-W-0069 → RH-W-0053 → RH-W-0048 → RH-W-0052 implementation chain nor authorizes a plan, implementation, review lifecycle, or Git delivery.

## Minimal Implementation (2026-10-01)

The owner authorized implementation and delegated the scope under issue #258. Two generated surfaces change:

1. Working agreements, Stopping: "Once the human decides, warn once about the consequence, then comply within authorized scope; never re-ask."
2. Session-stop closure gate: a declined, pending, failed, or incomplete required reconciliation still never yields `completed`, but `paused` is used only for actually pending human input, `blocked` only for an actually missing prerequisite, and `failed` for a failed operation. An accepted decline is reported as `declined` after one warning, with the work unit stated as not completed and no aggregate close label; the Output Contract permits that scoped report.

To fit the new sentence, this repository's own working agreements shed the Project Agreements clause that `usage.md` already owns. No new status, schema, or store was added.
