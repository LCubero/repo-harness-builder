# RH-W-0068 Required Documentation Closure Gate Design

> **Status: EARLIER DOCUMENTATION-CLOSURE SLICE IMPLEMENTED AND VERIFIED. RH-W-0068 is active for its owner-delegated remainder; implementation, projection synchronization, scenario-contract coverage, and whole-suite verification are finished locally. Delivery remains pending.**

The [RH-W-0074 design](rh-w-0074-design.md), implemented on 2026-10-01, replaces only the forced `paused`/`blocked` classification for accepted omissions; completion eligibility below is unchanged. An accepted decline is reported as `declined` with the work unit not completed, and `paused` or `blocked` apply only to actually pending input or missing prerequisites.

## Decision

After successful implementation or delivery verification, a work unit may be `completed` only after every canonical documentation surface applicable to that work unit is reconciled from proven facts. The exact close order is:

1. Implementation.
2. Verification evidence.
3. Documentation reconciliation from that evidence.
4. Continuity handoff.
5. Completion eligibility.

This closes one verified native session-stop loophole inside the existing [RH-W-0068](roadmap.md#rh-w-0068) owner. It is separate from completed RH-W-0051 and from RH-W-0068's unresolved changelog-disposition, Git-policy, and unrelated-skill activation defects.

## Applicability And Surfaces

Applicability is decided per work unit after verification. Required surfaces are only those that own a fact changed by that unit:

| Surface | Required when |
|---|---|
| Canonical roadmap or lifecycle record | The unit has a tracked lifecycle record or changes lifecycle truth |
| Current-state projection | Its canonical lifecycle or visible current state changed |
| Localized peers | The applicable owner declares localized parity |
| Meaningful changelog | Completed behavior, delivery, or another meaningful completed change belongs in chronology |
| Documentation map | A document was created, moved, deleted, or changed map ownership |
| Other durable documentation | The work changed a durable fact owned there |

An applicable surface ends as `completed`. A genuinely irrelevant surface ends as `not_applicable` with a concise reason. Trivial, no-op, wording-only, or otherwise inapplicable work creates no mandatory churn.

## Closure Outcomes

- Declined, pending, failed, or incomplete required reconciliation is never `completed`. Since RH-W-0074, the close is `paused` only when human input is actually pending, `blocked` only when a prerequisite is actually missing, and `failed` for a failed operation; an accepted decline is reported as `declined` with the work unit not completed.
- The user may decline documentation. That declines completion, not preservation of verified implementation, delivery, commits, evidence, or other successful operation outcomes.
- Optional or `not_applicable` documentation never blocks completion.
- Documentation is written only after implementation or delivery verification supplies proven facts. No evidence, status, delivery, chronology, or parity claim is fabricated or documented prematurely.
- Continuity records the actual scoped outcomes and never substitutes for canonical documentation.

The roadmap remains lifecycle authority. Its current-state projection and every declared localized peer must move in the same work unit. A localized failure preserves successful implementation evidence but prevents authoritative lifecycle completion.

## Smallest Implementation Shape

The implementation changes only the canonical `session_stop_skill()` contract, its focused canonical test, and generated projections. It adds required documentation reconciliation to completion eligibility, preserves per-surface `completed` or `not_applicable`, and replaces the former rule that declined documentation could continue to a completed close. It adds no store, schema, manifest field, checker behavior, migration, or generalized workflow engine.

## Focused Acceptance Evidence

RED needs one focused canonical contract test proving the current loophole: a verified implementation or delivery cannot become `completed` when any applicable canonical documentation result is declined, pending, failed, or incomplete. The same test must assert the five-step order, `not_applicable` handling, preservation of completed work after user decline, and no premature or fabricated documentation.

RED failed on the absent ordered gate. GREEN passed the focused closure regression and the related session-stop contract set after the smallest source change. Maintenance plan `1db45a1a6d561fa44f34a5f4032874ea707b3acfda0851f14231f0d2b0630a1a` replaced only the three generated session-stop projections and returned `VALIDATED`.

## Boundaries

- No RH-W-0051 source, generated output, test, lifecycle evidence, or delivery claim is changed by this design.
- This earlier documentation-closure slice is implemented and verified; RH-W-0068 is `active` for its owner-delegated remainder. Current implementation and projection synchronization are finished, but independent whole-suite verification failed on the stale documented count. Scenario-contract coverage and whole-suite verification (616 tests, 11 skipped, 0 failures) are settled locally; delivery must precede whole-unit completion.
- No commit, push, network action, RDD flow, store, schema, or checker behavior is part of this slice.
- Existing verified work is never rolled back because documentation is declined or blocked.

## Lifecycle Disposition

The documentation-closure defect is resolved. On 2026-10-01 the owner delegated a minimal scope for the remaining defects under issue #260:

1. Changelog disposition: the session-stop Output Contract reports it as exactly one of `updated`, `not needed`, `declined`, or `pending approval`, with its reason; completed designs, plans, or lifecycle changes are meaningful work, never `not needed`.
2. Git route: completed required continuity can coexist with a pending optional Git question, such as the push offer for an ahead-only branch after a commit; the invocation ends on that question and stays resumable.
3. Scenario-contract coverage: one deterministic real-Git scenario commits on a clean upstream, confirms `inspect_git_state` reports `clean` with ahead 1 and behind 0, and requires the generated session-stop contract to offer the push and keep the close resumable for that state. It failed against the previous contract on the missing resumable clause. Changelog disposition and skill activation have no executable helper, so their contract-text tests are the terminal coverage disposition. Unit tests do not prove live-model compliance.
4. Unrelated skills: generated session-start and session-stop never activate unrelated personal or external skills by phrase matching.

The narrow aggregate-outcome clarification is covered by item 2 together with completed RH-W-0074.
