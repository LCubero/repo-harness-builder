# RH-W-0051 Local-State Taxonomy Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Review Contract

Review this design against the canonical [RH-W-0051 roadmap record](roadmap.md#rh-w-0051), [standing decisions](decisions.md), and the [Spanish peer](../es/repo-harness/rh-w-0051-design.md). Repository evidence owns current behavior. This design defines storage taxonomy only and does not implement session behavior.

## Decision

Harness-owned local state uses four fixed repository-relative namespaces:

| Path | Class | Retention | Authority |
|---|---|---|---|
| `.repo-harness/tmp/` | Disposable working state | May be removed by an authorized harness operation | Non-authoritative |
| `.repo-harness/archive/` | Recoverable archived state | Never automatically deleted | Non-authoritative |
| `.repo-harness/state/` | Small current operational state reserved for declared consumers | Consumer-defined | Non-authoritative |
| `.repo-harness/memory-bk/` | Local session continuity backups reserved for RH-W-0069 | Never automatically deleted initially | Non-authoritative |

The directory name is `memory-bk`, not `memory`, to keep the boundary explicit: it is a fallback continuity backup, not a generic memory API or a second documentation store.

## Ownership Boundaries

- RH-W-0051 owns fixed paths, containment, Git-ignore behavior, clean-worktree semantics, checker classification, and manifest representation for the taxonomy.
- RH-W-0069 exclusively owns backup file schema, read and write order, privacy filtering, Engram cross-checking, and session integration.
- RH-W-0053 retains typed interrupted checkpoints. Its reserved location is `.repo-harness/state/checkpoints/`; a backup never becomes an executable checkpoint.
- RH-W-0046 and deferred RH-W-0018 retain consent or preference semantics. Their reserved location is `.repo-harness/state/preferences/`.
- RH-W-0040 retains violation-record semantics. Its reserved location is `.repo-harness/state/violations/`.
- RH-W-0052 retains archive, uninstall, reinstall, and deletion approval behavior.
- Personal storage, `.ld9/`, `.atl/`, `.codegraph/`, and `.engram/` remain outside harness ownership.

No namespace owns documentation, approvals, lifecycle, conflicts, receipts, delivery, or canonical project truth.

## Path And Git Safety

Every local-state path is resolved from the selected repository root, normalized, and rejected if it escapes that root, traverses an indirect path, or targets anything other than the exact declared namespace. Multi-root workspaces resolve each selected repository independently. A non-Git folder uses the same containment and ownership rules without pretending Git evidence exists.

All four namespaces are Git-ignored by default and excluded from clean-worktree concerns. Implementation adds exact root ignore directives for the directories, while preserving human-owned `.gitignore` bytes through the existing append and approval model. A repository override may change ignore or retention policy only through a future explicit, repository-scoped design; this design defines no override syntax.

Plain `python .repo-harness/check.py` remains read-only. It reports taxonomy support or drift without creating directories, backups, or ignore entries. A missing local-state directory is not drift by itself because directories are created only when a consumer first writes approved state. Missing required ignore directives or an inconsistent declared taxonomy is drift.

## Manifest And Checker Impact

Implementation should add one manifest field, `localState`, rather than one field per consumer. Its exact value is the fixed ordered mapping of namespace names to repository-relative paths and classes. The authenticated manifest and generated checker must change atomically because the checker enforces exact manifest fields.

The checker validates exact declared paths, classes, root containment, and ignore directives. It does not enumerate or parse local files, judge backup content, enforce retention, or treat ignored local state as generated output. RH-W-0069 may add focused behavior tests without adding another manifest field.

## Compatibility And Migration

- Existing installations without `localState` continue through the established one-pass authenticated manifest/checker replacement path.
- Migration adds no directories and moves no existing files.
- Personal `.ld9/` content is never discovered, copied, migrated, renamed, or deleted automatically.
- Existing `.repo-harness/` generated and human-owned content remains untouched except for the separately authorized manifest/checker pair and bounded `.gitignore` integration.
- A future explicit migration from personal or legacy storage requires its own approved design.

## Focused RED Tests

Before implementation, focused tests must fail for:

1. The render declaring exactly `tmp`, `archive`, `state`, and `memory-bk` under `.repo-harness/`.
2. Exact root containment and rejection of traversal, absolute, indirect, and cross-root paths.
3. Exact Git-ignore planning that preserves existing bytes and accepts only equivalent root directives.
4. A read-only check producing zero writes when namespaces are absent.
5. Drift when required ignore or manifest taxonomy differs.
6. Atomic replacement of a changed authenticated manifest/checker pair.
7. No automatic directory creation or migration during render preview, check, or upgrade.
8. No read, write, move, or delete under `.ld9/` or other tool-owned paths.
9. Non-Git and multi-root behavior remaining repository-scoped.
10. English and Spanish design parity, valid links, and forbidden Unicode rejection.

## Exclusions

- No session-start or session-stop behavior.
- No backup schema, filename, content filter, Engram call, conflict resolution, or retention cleanup.
- No implementation of checkpoints, preferences, consent, violations, archive deletion, or migration.
- No database, encryption subsystem, daemon, scheduler, background service, generic state API, or complex configuration.
- No generated skill, product source, test, manifest, checker, directory, staging, commit, push, or network change in this design work.

## Rollback

Design rollback removes this document and its lifecycle references only. A future implementation rollback restores the prior manifest/checker pair and ignore behavior through the normal atomic generated-output path. It never deletes local-state contents automatically.

## Done Conditions

- Both design peers and documentation maps agree on the four namespaces and ownership boundaries.
- RH-W-0051 remains `proposed`; implementation has separate authorization.
- RH-W-0069 depends on this taxonomy but does not enlarge RH-W-0051 into session behavior.
- Roadmap order places RH-W-0069 after RH-W-0051 and before consumers that need its session continuity behavior.

## Remaining Human Decisions

None for this design. Any repository override syntax, retention cleanup, migration, implementation, or lifecycle advancement requires separate explicit authorization.
