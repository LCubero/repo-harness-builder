# RH-W-0069 Local Memory Backup And Cross-Source Continuity Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Review Contract

Review this design against the canonical [RH-W-0069 roadmap record](roadmap.md#rh-w-0069), the [RH-W-0051 taxonomy](rh-w-0051-design.md), completed [RH-W-0050 reconciliation semantics](rh-w-0050-design.md), current [memory rules](../../.repo-harness/rules/memory.md), and the [Spanish peer](../es/repo-harness/rh-w-0069-design.md). Repository evidence remains authoritative.

## Decision

Every deliberate session pause or close writes one local structured Markdown backup under `.repo-harness/memory-bk/`. When Engram is available, session-stop also writes Engram. The two writes are independent and visible. Partial success is `degraded`; no operation fabricates success for the other.

Session-start reads current repository evidence first, then Engram when available, then the newest local backup by default. It cross-checks Engram and local backup claims against each other and against current repository owners. Conflicts are reported, never silently merged. Older backups are read only when the current objective, a conflict, or missing continuity makes them relevant.

Gentle AI availability is independent from memory availability and stays outside backup mechanics. Built-in Claude, OpenCode, or Pi memory is neither authority nor a continuity dependency.

## Smallest Backup Schema

Filename:

```text
YYYYMMDDTHHMMSSZ-<source>-<short-session-id>.md
```

The timestamp is UTC. `source` is `pause` or `close`. The short session ID contains only lowercase ASCII letters, digits, and hyphens. Creation uses exclusive write semantics; a collision fails visibly and never overwrites an existing backup.

Required document shape:

```markdown
---
project: <stable repository project id>
timestamp_utc: <RFC 3339 UTC timestamp>
source: pause | close
session_id: <bounded non-secret id>
---

# Session Continuity

## Goal

## Instructions

## Discoveries

## Accomplished

## Next Steps

## Relevant Files
```

Each claim that is not self-evidently a human instruction or next-step intent carries one inline classification: `verified fact`, `supported inference`, `unknown or unverifiable`, or `human decision required`. A supported inference names its evidence and what would raise it to verified. Relevant files are repository-relative paths only. Empty sections use `None.` rather than placeholders.

The backup contains concise continuity, not copied source, logs, full diffs, test output, receipts, lifecycle tables, approval records, conflict state, or duplicate documentation.

## Authority Order

Every recalled claim is verified against the current applicable owner before use:

1. Source and tests own behavior.
2. `roadmap.md` owns lifecycle and execution ordering.
3. `current-state.md` owns the concise projection.
4. `decisions.md` owns standing rationale.
5. `CHANGELOG.md` owns completed chronology.
6. Current Git and checker evidence own delivery and congruence.
7. Engram and local backups are non-authoritative continuity evidence.
8. Built-in runtime memory is ignored as authority and is never required.

When owners differ, the most specific currently applicable owner wins. If repository evidence cannot resolve a material decision, session-start asks the human one concrete question.

## Session-Start Read Order

1. Resolve and validate the selected repository root.
2. Read `REPO-HARNESS.md`, applicable canonical documentation, current state, the exact roadmap records in scope, decisions when relevant, and latest relevant changelog entries.
3. Inspect current source, tests, Git, and checker evidence required by the objective.
4. Resolve explicit Engram project scope and read relevant recent context when Engram is available. Record each operation outcome independently.
5. Locate the newest valid regular Markdown backup inside `.repo-harness/memory-bk/` and read it. Ignore filename order unless metadata parses and agrees with the filename.
6. Read an older backup only when the newest file is invalid, the objective names it, a conflict needs chronology, or the newest file explicitly lacks relevant continuity.
7. Cross-check every Engram and backup claim against current repository evidence, then cross-check Engram and backup with each other.
8. Report accepted, stale, conflicting, unavailable, and unverifiable continuity separately. Never mutate during startup or continue an unfinished operation automatically.

An absent backup directory or no backup files is `not_found`, not failure. An unreadable, malformed, unsafe, or contradictory newest backup is reported and skipped; it is never repaired during startup.

## Session-Stop Write Order

1. Refresh repository evidence and build the concise structured summary.
2. Apply the bounded content filter before either write.
3. Write the local backup first with exclusive creation and verify it by reopening, parsing metadata, and checking required headings.
4. If Engram is available, write the equivalent scoped session summary to Engram and preserve all operation-level outcomes, including conflict candidates.
5. Cross-check the verified local backup and returned Engram observation for material semantic conflict when both writes succeeded.
6. Report `local_backup`, `engram_write`, and `cross_check` independently.

Local backup failure makes required local continuity `failed`. If local backup succeeds and Engram fails or is unavailable, continuity is `degraded`, not failed or completed without qualification. If local backup fails and Engram succeeds, preserve the Engram success but the required close continuity remains `failed`. If both succeed but conflict, continuity is `degraded` and the conflict is reported. A conflict unresolved by repository evidence becomes `human decision required`.

## Privacy And Content Filter

Before writing either destination, exclude:

- credentials, tokens, passwords, private keys, cookies, connection strings, and authentication material;
- raw environment names paired with values, environment dumps, command output that may contain secrets, and provider payloads;
- absolute paths, replacing a useful in-repository path with its repository-relative form and otherwise omitting it;
- unnecessary source excerpts, diffs, logs, stack traces, generated contents, or test output;
- sensitive personal data and unrelated personal preferences;
- external private repository names or locations unless an approved stable alias already exists in canonical documentation.

The filter is a bounded content-selection rule, not secret detection proof, encryption, redaction infrastructure, or a hostile-filesystem boundary. If safe summarization is impossible, omit the claim and report that continuity was reduced for privacy.

## Path Safety And Git Ignore

Only direct regular `.md` files inside the exact repository-relative `.repo-harness/memory-bk/` directory are eligible. Reject absolute inputs, `..`, separators inside the generated filename fields, indirect paths, symlinks, junction escapes, alternate streams, and cross-root resolution. Never follow links from backup content.

RH-W-0051 owns the required exact Git-ignore directive and clean-worktree exclusion. RH-W-0069 creates the directory lazily on the first authorized pause or close write. It never stages or tracks the directory or its files.

## Conflict Outcomes

| Evidence | Outcome |
|---|---|
| Memory agrees with current owner | Accept as continuity after owner verification |
| Memory is older than current owner | Mark stale and use current owner |
| Engram and backup differ, repository resolves | Report conflict and use repository evidence |
| Engram and backup differ, repository cannot resolve a material decision | Ask the human; do not merge or choose silently |
| One source is missing | Continue with the available source and report the missing operation |
| Source is malformed or unsafe | Reject that source, preserve other outcomes, and report exact bounded reason |

No conflict creates a new conflict store. RH-W-0050 classifications and Engram advisory relations are reused without reopening RH-W-0050.

## Compatibility And Migration

- Existing repositories without the directory start with no local backup history; this is valid.
- No automatic import from built-in runtime memory, old handoffs, or locations outside the exact repository backup directory.
- Repo-harness owns this native continuity contract. The owner alone controls continuity policy and authorizes each operation that changes retained continuity.
- Existing Engram observations remain untouched and are checked only when relevant.
- Plain Markdown needs no database, encryption, daemon, background service, or generic memory API.

## Manifest And Checker Impact

RH-W-0051 owns the single `localState` manifest taxonomy and exact ignore checking. RH-W-0069 adds no manifest field. The generated checker remains read-only and does not inspect backup contents or require the directory to exist. Focused implementation tests validate generated session contracts and helpers; the checker only verifies the owning local-state declaration and ignore contract.

## Focused RED Tests

Before implementation, focused tests must fail for:

1. Session-stop always attempting the local backup on deliberate pause and close.
2. Exact required metadata and seven sections, repository-relative relevant files, and evidence classifications.
3. Exclusive collision-safe creation, lazy directory creation, structural readback, and no overwrite.
4. Local write before Engram write and independent visible outcomes for every success/failure combination.
5. Engram/local cross-check with stale, resolved conflict, unresolved material conflict, malformed source, and missing-source cases.
6. Session-start repository-first, Engram-second, newest-backup-third ordering and bounded older-backup reads.
7. Verification of every recalled claim against its current applicable owner.
8. Rejection of credentials, raw environment values, absolute paths, unnecessary source, and sensitive personal data.
9. Path containment, regular-file-only reads, no indirect paths, and no staging or tracking.
10. Gentle AI independence, built-in-memory exclusion, and exclusive use of the native session continuity contract.
11. No checkpoint, approval, lifecycle, conflict, receipt, or delivery semantics in backups.
12. Integrated and standalone session renders, English and Spanish documentation parity, links, placeholders, and forbidden Unicode.

## Exclusions

- No implementation in this design work and no lifecycle advancement beyond `proposed`.
- No product code, generated skill, test, manifest, checker, directory, migration, staging, commit, push, publication, or network action.
- No automatic deletion or retention cleanup initially.
- No generic memory API, database, encryption subsystem, daemon, scheduler, background service, synchronization engine, or complex configuration.
- No automatic continuation, approval reuse, executable checkpoint, documentation duplication, lifecycle copy, receipt, delivery record, or conflict store.
- No Gentle AI coupling and no built-in runtime memory dependency.

## Rollback

Design rollback removes this document and its lifecycle references. A future implementation rollback removes the session contract behavior and focused tests together while leaving existing local backups and Engram observations untouched. Backup deletion always remains a separate explicit action.

## Done Conditions

- The bilingual designs and lifecycle records are semantically equivalent and linked.
- RH-W-0051 reserves `.repo-harness/memory-bk/` without owning session behavior.
- RH-W-0069 owns the exact schema, authority, read/write order, outcomes, privacy, conflicts, compatibility, tests, and exclusions.
- The proposed execution order places RH-W-0069 immediately after RH-W-0051 implementation and before RH-W-0053, because it consumes the taxonomy and establishes ordinary session continuity without absorbing typed checkpoint recovery.
- Implementation remains separately unauthorized.

## Remaining Human Decisions

None for this design. Implementation authorization and any future migration or deletion policy require separate explicit owner decisions.
