# RH-W-0068 Required Documentation Closure Gate Design

> **Status: IMPLEMENTED AND VERIFIED. RH-W-0068 remains proposed for its separate unresolved defects.**

The [owner-approved RH-W-0074 design](rh-w-0074-design.md) explicitly targets the forced `paused`/`blocked` classification for accepted omissions, not completion eligibility. Implementation remains separately unauthorized. This implemented contract is unchanged and remains binding until that implementation is authorized and completed.

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

- Declined, pending, failed, or incomplete required reconciliation makes the close `paused` when human approval or input can resolve it, otherwise `blocked`. It is never `completed`.
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
- This documentation-closure slice is implemented; RH-W-0068 remains `proposed` until its other owned defects are resolved.
- No commit, push, network action, RDD flow, store, schema, or checker behavior is part of this slice.
- Existing verified work is never rolled back because documentation is declined or blocked.

## Lifecycle Disposition

The documentation-closure defect is resolved. RH-W-0068 is not complete because the changelog-disposition, Git-policy, and unrelated-skill activation defects still require separate design and implementation authority.
