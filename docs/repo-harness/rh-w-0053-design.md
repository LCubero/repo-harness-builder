# RH-W-0053 Interrupted-Session Checkpoint Design

> **Status: OWNER-APPROVED PROPOSED DESIGN. Implementation is separately unauthorized.**

## Decision

RH-W-0053 uses one ignored, non-authoritative structured Markdown file at `.repo-harness/state/checkpoints/active.md`. It records one incomplete operational checkpoint, not session history. Creation or replacement requires an explicit current human request; interruption alone grants no write authority.

Session-start may discover and read the file without mutation. It never continues the recorded work automatically. Resumption requires current repository verification and fresh authority for every action that still needs approval.

Review this design against the canonical [roadmap record](roadmap.md#rh-w-0053), [RH-W-0050 reconciliation](rh-w-0050-design.md), [RH-W-0051 local-state taxonomy](rh-w-0051-design.md), [RH-W-0069 continuity design](rh-w-0069-design.md), current [memory rules](../../.repo-harness/rules/memory.md), and the [Spanish peer](../es/repo-harness/rh-w-0053-design.md).

## Ownership And Boundaries

| Owner | Responsibility |
|---|---|
| RH-W-0051 | `.repo-harness/state/`, containment, ignore, manifest, and read-only checker behavior |
| RH-W-0053 | `checkpoints/active.md`, checkpoint schema, creation, discovery, expiry, and resumption rules |
| RH-W-0050 | Evidence classes and bounded reconciliation |
| RH-W-0069 | Pause or close summaries in `.repo-harness/memory-bk/` and Engram cross-checking |
| Repository owners | Current behavior, lifecycle, decisions, chronology, delivery, and approvals |

The checkpoint is neither canonical truth nor authority. It is not a backlog, plan, approval, receipt, delivery record, conflict store, memory summary, or backup. It stores only the minimum unfinished operational state needed to evaluate a possible resume.

## Persistence And Schema

The directory is created lazily only for an authorized write. There is at most one active checkpoint. A new authorized checkpoint replaces the existing regular file only after its current bytes are inspected and the replacement is written safely and read back structurally. No checkpoint history or second index is created.

Required shape:

```markdown
---
schema_version: 1
project: <stable repository project id>
checkpoint_id: <bounded lowercase ASCII id>
created_at_utc: <RFC 3339 UTC timestamp>
objective: <one-line non-sensitive objective>
step_state: not_started | started_unverified | failed
resume_authority: required
---

# Incomplete Operational Checkpoint

## Verified Completed Facts
## Interrupted Step
## Remaining Intent
## Supported Inferences
## Unknowns
## Human Decisions Required
## Relevant Files
```

`Verified Completed Facts` may include only outcomes supported by current evidence at write time. `Interrupted Step` contains one action description, its `step_state`, and bounded observed evidence; it never uses `completed`. `Remaining Intent` is intent, not a promise or plan authorization. Each inference names its evidence and what would make it verified. Relevant files are repository-relative regular paths. Empty sections contain `None.`

The checkpoint never persists an approval token, approval scope, permission claim, command credential, or assertion that prior authority remains valid. `resume_authority` is always `required`; no other value is valid.

## Creation And Expiry

Creation or replacement is allowed only when the human explicitly requests saving the current incomplete operation as a checkpoint. An already authorized implementation, pause, stop, or continuity write does not imply checkpoint authority. If an interruption occurs before such a write completes, no checkpoint success is claimed.

A checkpoint expires logically, without automatic deletion, when its objective completed, was abandoned, or was superseded; its project identity no longer matches; current evidence contradicts a material premise or shows the step settled; its lifecycle owner no longer permits the intent; or its path or structure is invalid.

Age alone is advisory and never proves expiry. No timer, scheduler, daemon, cleanup job, retention database, or automatic archival exists. An expired checkpoint remains untouched until an explicit authorized replacement or deletion action.

## Sensitive-Data Limits

Apply the RH-W-0069 bounded content-selection rule. Exclude secrets, credentials, tokens, cookies, private keys, connection strings, environment dumps or values, provider payloads, absolute paths, private external repository identities, personal data, full commands carrying sensitive arguments, source excerpts, diffs, logs, stack traces, and test output. Use repository-relative paths and concise outcomes. If an unfinished step cannot be represented safely, omit the sensitive detail and mark the necessary state `unknown or unverifiable`.

This is content minimization, not secret-detection proof, encryption, or protection from a hostile filesystem.

## Non-Mutating Session-Start Discovery

1. Resolve the selected repository and read current applicable owners first.
2. Check only the exact `active.md` path. Absence is `not_found`, not failure.
3. Reject non-regular, indirect, escaping, malformed, or project-mismatched input without following links.
4. Compare every material checkpoint claim with current repository, Git, checker, lifecycle, and decision evidence that applies.
5. Classify the checkpoint as `usable`, `stale`, `contradictory`, `expired`, or `invalid`, and report the bounded reason.
6. Do not rewrite, delete, archive, mark consumed, update timestamps, or continue work during discovery.

`usable` means only that the checkpoint can inform a resume proposal. `stale` means newer evidence exists and wins. `contradictory` means material claims disagree and current owners do not resolve the operational meaning. `expired` follows the semantic conditions above. `invalid` covers unsafe path or schema failure.

## Resumption Authority

Resumption is a new operation, never continuation by inheritance:

1. Reconstruct the smallest current objective from repository evidence and the usable checkpoint.
2. Re-verify completed facts and the interrupted step; do not rerun a step whose current completion is verified.
3. Reclassify remaining intent against current lifecycle, decisions, scope, dependencies, and repository state.
4. Discard stale checkpoint claims from the proposed route; preserve them only as reported non-authoritative evidence.
5. Ask one concrete question only when current human authority or a real product decision is required.
6. Execute only after fresh authority covers the exact current action. Prior approvals, consent, digests, tokens, and session intent are never reused.

A stale, contradictory, expired, or invalid checkpoint cannot authorize or trigger work. If current evidence resolves a contradiction, repository evidence wins and the discrepancy is reported. Otherwise only the affected resume branch pauses for the human.

## Relationship To Continuity

RH-W-0069 backup or Engram summary answers, "What happened and what matters next?" RH-W-0053 answers, "What exact operational step was left incomplete, and is it safe and authorized to resume now?"

Session-start may use both, but verifies both against current owners. A backup never becomes a checkpoint, a checkpoint never substitutes for the pause or close summary, and neither source copies the other. Conflict handling reuses RH-W-0050 classifications and creates no additional persistence.

## Acceptance Criteria

- The only checkpoint path is `.repo-harness/state/checkpoints/active.md`, with one structurally typed Markdown checkpoint and no history store.
- No checkpoint write occurs without an explicit current human request.
- An interrupted step can only be `not_started`, `started_unverified`, or `failed`; it is never represented as completed.
- Sensitive material and absolute paths are excluded; safe omission becomes unknown rather than fabricated detail.
- Session-start discovery is repository-first, bounded to the exact path, and performs zero writes.
- Missing, stale, contradictory, expired, invalid, and usable outcomes are distinguishable.
- Expiry is evidence-based and causes no automatic deletion or cleanup.
- Every resume re-verifies current evidence and obtains fresh authority; no prior approval is reusable.
- RH-W-0069 summaries and Engram remain distinct continuity sources, not executable checkpoints.
- Integrated and standalone operation require no daemon, database, scheduler, service, ledger, cache, generic parser service, or global state.

## Exclusions And Future Impact

This design changes no product source, generated skill, test, manifest, checker, local-state directory, runtime projection, or lifecycle status. A future separately authorized implementation may update the canonical session-start and session-stop contracts and focused tests. RH-W-0051 remains the namespace and ignore owner, so RH-W-0053 adds no manifest field and the checker must not inspect checkpoint contents.

The owner approved this proposed design on 2026-09-06. Implementation planning, implementation, migration, deletion, archival, and delivery remain separately unauthorized. No human product decision remains for this proposed design.
