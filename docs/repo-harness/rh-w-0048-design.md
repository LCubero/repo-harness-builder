# RH-W-0048 Runtime Projection Recovery Design

> **Status: OWNER-APPROVED DESIGN. RH-W-0048 remains proposed; implementation planning and implementation are separately unauthorized.**

## Decision

An installed harness may recover only missing, local runtime projections through the future explicit command `python .repo-harness/check.py repair`. With no repair argument, `python .repo-harness/check.py` remains a non-mutating check and reports `writes: 0`.

`repair` is preview-only by default. It lists the exact files eligible for creation and returns a deterministic `planSha256`. Mutation requires `repair --apply --approved-plan-sha256 <digest>`. Apply re-runs the preflight and refuses if the plan changed. This is explicit repair authority, not builder approval, prior session consent, or permission to update existing bytes.

Review this design against the canonical [roadmap record](roadmap.md#rh-w-0048), [standing decisions](decisions.md), [RH-W-0051 local-state taxonomy](rh-w-0051-design.md), current [architecture](../../skill/repo-harness-builder/references/architecture.md), current [preservation rules](../../skill/repo-harness-builder/references/preservation-and-safety.md), and the [Spanish peer](../es/repo-harness/rh-w-0048-design.md).

## Source Of Truth And Recoverable Set

The authenticated `.repo-harness/manifest.json` declares the selected runtimes and canonical-to-runtime relationships. The canonical files named by that authenticated declaration supply the expected bytes. Ordinary consumer recovery uses `canonicalSkills` and `canonicalCommands`, whose sources live under `.repo-harness/`. A reciprocal source repository may also evaluate `canonicalProjections` only when its declared canonical source is present, direct, repository-contained, and readable. An unavailable canonical source is reported and never reconstructed from a runtime copy.

No second inventory, template bundle, cache, ledger, database, or manifest mode is added. The existing authenticated manifest plus canonical files are sufficient. Runtime-family mappings remain the mappings already declared by the manifest: command peers for Claude Code and OpenCode, prompt peers for Pi, and skill projections for each selected runtime. The design does not assume a runtime reload API or registry command. Recovery proves files on disk; live runtime pickup remains runtime-specific and is reported as unverified unless independently observed.

The minimum recoverable set is exactly the selected runtime targets derivable from authenticated `canonicalSkills` and `canonicalCommands`. Optional `canonicalProjections` remain repository-specific declared targets, not a requirement added to ordinary consumers.

## Projection Classification

| Result | Meaning | Repair behavior |
|---|---|---|
| `current` | A direct regular target matches its canonical expected bytes under the existing comparison rules. | No action. |
| `missing-local` | The target is absent, its canonical source is valid, and Git evidence does not declare that exact target tracked; without usable Git, projections are local by default. | Eligible for additive creation. |
| `missing-tracked` | The target is absent but the exact path is tracked in the available Git index. | Real drift; report only. Never reclassify as local or repair silently. |
| `stale` | A direct regular target exists but differs from expected canonical bytes. | Preserve every byte; report that repair does not update it. |
| `damaged` | The manifest is unauthenticated or invalid, a source or target is indirect, unsafe, unreadable, the wrong file type, outside the root, or otherwise cannot support a safe exact comparison. | Refuse the affected repair branch before writes and report the bounded reason. |
| `canonical-missing` | A declared canonical source is absent. | Report honestly; no target can be created from missing authority. |

“Obsolete” in human explanation maps to existing `stale`: bytes exist but no longer match their canonical owner. It is not absence and is not eligible for missing-file repair. Package freshness remains separate from projection state.

## Preflight, Preview, And Authority

Preflight is read-only and bounded to the authenticated manifest, its declared canonical files and targets, path containment, target kind, and exact tracked-path evidence when Git is usable. It neither creates directories nor edits `.gitignore`, `.gitattributes`, manifest, checker, canonical sources, or runtime files.

Preview returns the classification for each declared target, the exact sorted `create` set, exclusions with reasons, `writes: 0`, and `planSha256`. The digest binds the repository-relative path and expected content digest of every creation plus the authenticated manifest digest. It contains no absolute paths or file content. An empty eligible set succeeds honestly with no apply work.

Apply accepts only the exact current digest, repeats the same preflight immediately before the first write, and refuses stale authority. It never stages, commits, pushes, initializes Git, follows links, or asks a runtime to reload.

## Repair Transaction And Preservation

Repair creates only `missing-local` targets with exclusive-create semantics. It may create required parent directories inside the selected root. Every pre-existing file, unknown child, human-evolved file, stale projection, tracked-missing path, and unrelated directory byte remains untouched.

All eligible creations form one transaction. After each write, apply verifies containment and the expected digest. On any failure it removes only files created by that invocation whose bytes still equal what the invocation wrote; it leaves changed or pre-existing bytes untouched. Empty parent cleanup is best effort and may remove only directories created by the same invocation. This reuses the existing create, preflight, digest, verification, and rollback model rather than introducing a journal or archive.

After apply, the checker runs the same projection comparison again. Success requires every attempted target to report `current`; the result lists created, current, skipped, refused, rollback, and post-check outcomes separately. A partial or failed rollback, unavailable live-runtime validation, remaining stale file, tracked-missing file, or canonical source failure is reported explicitly and never collapsed into `recovered`.

## Repair Versus Update

Repair restores absent local projections from already installed canonical material. It does not change canonical content, manifest schema, package version, selected runtimes, existing runtime bytes, or human documentation.

Update remains the builder renderer's separately approved operation. It may replace authenticated manifest-owned generated bytes under the existing content-bound plan, one-pass manifest/checker atomicity, and rollback rules. A user needing an obsolete projection changed receives an update recommendation; `repair` never broadens itself into update, reinstall, archive, uninstall, or deletion. RH-W-0052 retains those later operations.

For local-only projections, future rendering omits new `.gitattributes` rules because Git never checks those paths out. Existing human-owned attribute lines are preserved and never removed by repair. Exact tracked projections retain the existing LF contract. This changes no current `.gitattributes` bytes.

## Acceptance Criteria

- Plain check remains read-only and reports zero writes.
- Preview classifies current, missing-local, missing-tracked, stale, damaged, and canonical-missing cases without writes.
- Apply requires the exact preview digest and creates only missing-local targets.
- Fresh-clone and partial-tree cases recover the declared minimum for Claude Code, OpenCode, and Pi from available canonical files without an external builder installation.
- Existing, stale, unknown, human-evolved, indirect, unsafe, and tracked-missing targets retain every byte.
- Any failed multi-file creation rolls back only unchanged files and directories created by that invocation.
- Post-check reports each remaining limitation honestly; on-disk parity never implies a runtime reload.
- Repair and update remain separate authorities and RH-W-0052 remains separate.

## Non-Goals

- No implementation, implementation plan, migration, reinstall, archive, uninstall, deletion, staging, commit, push, PR, release, or runtime reload.
- No new service, daemon, scheduler, database, ledger, cache, global state, second manifest, copied template store, or network dependency.
- No overwrite or normalization of existing runtime files and no inference that ignored paths are disposable.
- No runtime-specific API beyond writing the already declared filesystem projections.

## Lifecycle And Human Decisions

RH-W-0048 remains `proposed`. The owner approved this design after delegated technical review and the parent's direct review. It resolves the command name, tracked signal, authority, source, recoverable set, classifications, preservation, transaction, update boundary, `.gitattributes` policy, runtime-parity limit, and result semantics. Approval does not authorize an implementation plan or implementation.
