# RH-W-0091 Startup Priority and Resume-First Design

> **Status: COMPLETED HISTORICAL DESIGN. RH-W-0091 is `completed`. The corrected bilingual implementation plan was owner-approved, implemented, and independently verified under separate authority.**

Canonical lifecycle: [RH-W-0091](roadmap.md#rh-w-0091). Scope: [rh-w-0091-scope.md](rh-w-0091-scope.md). Spanish peer: [rh-w-0091-design.md](../es/repo-harness/rh-w-0091-design.md).

## Boundary and ownership

[decisions.md](decisions.md) remains the sole charter and LLM-first owner; this design only points to it. The charter is bounded input, not copied policy. The roadmap remains the lifecycle owner, `current-state.md` remains a concise projection, and `CHANGELOG.md` remains completed chronology. This design does not reorder those durable documents.

`RH-W-0069` retains the single native backup schema, safety, privacy, validation, and write semantics. `RH-W-0088` retains Engram recovery as availability evidence. `RH-W-0078` alone owns physical generated-skill splitting and progressive-disclosure mechanics. `RH-W-0031`, `RH-W-0071`, `RH-W-0082`, and `RH-W-0018` retain language provenance and preference/continuing-constraint semantics; verified conversation language plus active stable preferences or continuing constraints from those existing owners precede backup recovery. Gentle AI retains routing, review, receipts, and correction at an applicable enabled boundary.

Engram is scoped continuity and availability evidence, never repository authority. It requires no separate preflight. Startup does not probe Gentle AI separately; it reaches Gentle AI only at its applicable enabled boundary.

## Minimal resume-first read model

For a relevant session start, read only this bounded sequence before any named trigger:

1. the bounded charter pointer in `decisions.md`;
2. verified conversation language plus active stable preferences or continuing constraints from their existing owners;
3. the newest valid `RH-W-0069` backup's `Where We Left Off`; and
4. deeper owners only when the trigger matrix requires them.

Repository owners remain authoritative. Missing, invalid, stale, or contradictory evidence is reported and resolved against its current owner; nothing is inferred from ordering. Scoped Engram recovery supplies availability/continuity evidence when applicable without a separate probe, preflight, or repeated unchanged-success report.

### Deeper-read trigger matrix

| Trigger | Required deeper read |
| --- | --- |
| Explicit objective or `RH-W-####` | The named owner and directly relevant evidence |
| Contradiction | The conflicting current owners until repository evidence resolves it, or one concrete human decision when it cannot |
| Invalid or missing priority evidence | The owner that can validate or replace that evidence |
| Mandatory reconciliation | The applicable canonical lifecycle, projection, localization, and continuity owners |

No trigger authorizes an exhaustive read, automatic continuation, preference inference, or a new store.

## Single-backup resume shape and stop order

Within the existing one-file `RH-W-0069` backup schema, `Where We Left Off` is the first section after `# Session Continuity`. It contains exactly these headings, in order:

```markdown
## Where We Left Off

### Next Action

### Pending Work

### Current Blocker
```

An empty value is `None.` Existing inline evidence labels continue to apply. The existing detailed continuity sections follow this resume block unchanged in role. This is a read/write ordering amendment inside the same exclusive UTC-timestamped backup, not a second summary, lifecycle, approval, receipt, or conflict store.

On a deliberate pause or close, session-stop must: (1) refresh applicable evidence; (2) determine the three resume fields in the stated order; (3) write and validate that resume block before detailed continuity; then (4) preserve the existing RH-W-0069 content filtering, exclusive-write, local-before-Engram, validation, and independent-outcome rules. No backup behavior changes until separately implemented.

## Future focused RED categories

Before separately authorized implementation, focused RED coverage must show:

1. charter reference without charter copying;
2. verified conversation language plus active stable preferences or continuing constraints before backup recovery, with no inference from absent or invalid evidence;
3. newest-valid backup selection and `Where We Left Off` before later detail;
4. exactly ordered `Next Action`, `Pending Work`, and `Current Blocker`, with `None.` when empty;
5. every trigger-matrix path and no untriggered deep read;
6. scoped Engram evidence without a separate preflight, and Gentle AI only at an applicable enabled boundary;
7. preserved RH-W-0069 schema, privacy, exclusive creation, local-before-Engram outcomes, and no second summary store; and
8. preserved RH-W-0078, language, preference, lifecycle, chronology, routing, review, receipt, and correction ownership.

These are contract categories, not implementation detail or live-model proof.

## Compatibility, rollback, and exclusions

Existing valid backups without `Where We Left Off` remain readable through RH-W-0069 detailed continuity; they do not gain synthesized fields. A future implementation must write the new block only in newly created backups and preserve all existing backups. No migration, retention change, cleanup, parser/API, database, daemon, cache, preference write, generated-skill projection, test implementation, Git action, network action, or delivery is part of this design.

Design rollback removes this document and its documentation references only. A future implementation rollback removes the resume-first contract and focused tests while preserving existing backups and Engram observations. Neither rollback deletes continuity or alters durable owners.

## Done conditions

- English and Spanish designs are semantically equivalent and linked.
- `RH-W-0091` is `completed`; this design remains historical owner-approved evidence.
- The read model, exact trigger matrix, resume fields, compatibility, rollback, and exclusions preserve every named owner.
- Implementation planning and execution were separately authorized; final verification and lifecycle closure are complete, with delivery unclaimed.
