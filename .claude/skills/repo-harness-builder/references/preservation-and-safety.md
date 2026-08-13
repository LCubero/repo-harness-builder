# Preservation And Safety

## The Whole Model

The harness creates files that are absent. It never replaces or deletes pre-existing bytes. The sole append exception is a selected runtime's root `CLAUDE.md` or `AGENTS.md`, and only for its exact required one-line directive.

Everything follows from it. There is no drifted-file recovery, migration, alias, replacement mode, or generalized patch engine. Preview hashes relevant pointer bytes and the complete deterministic plan. Apply requires that digest and refuses before writing when the current plan differs.

## Default Evidence

Scope planning evidence to intended write paths, relevant existing ancestors, and canonical renderer inputs. A full-tree strict snapshot may be used when requested, but is not the default.

## Classification

| Target | Result |
|---|---|
| Absent | Create it |
| Present with exactly the rendered bytes | Already identical; nothing to do |
| Present with any other content | Left untouched and reported |
| Selected root pointer missing its exact directive | Append only the smallest newline-delimited directive |
| Selected root pointer already containing its exact directive | Leave every byte unchanged |
| Present as a directory or indirect target | Refuse apply before creating anything |

## Adding A Runtime Later

`.repo-harness/manifest.json` and `.repo-harness/check.py` are generated files like any other, so a second run classifies them as present and leaves them untouched. Widening the runtime set therefore creates the new wrappers and projections correctly while the manifest keeps recording the old set, and the generated checker goes blind to everything the manifest does not list. It reports `current` after inspecting only part of the harness, which is worse than reporting drift.

The plan and the check both carry `runtimeSet` with `recorded`, `requested`, and `matches`, plus `unrecorded` and `staleManifest` only when the requested set names a runtime the manifest never recorded. That is the one direction needing recovery. Requesting fewer runtimes than recorded creates nothing new and leaves the checker covering everything, so report the difference and say no recovery applies. Surface the recovery case before approval and name it: delete the two files in `staleManifest`, then run again with the complete set. The builder recreates them because they are then absent, and no existing byte is replaced. This keeps additive-create intact rather than teaching the renderer to overwrite.

`CHANGELOG.md` needs no special case. If it exists, it is left alone like anything else. If it is absent, the standard dated changelog is created.

## Human-Owned Versus Generated

After creation, two kinds of file behave differently under `check`:

- **Generated**: canonical skills, runtime projections, wrappers, and the manifest. Compared byte for byte. A difference is reported as `modified`.
- **Human-owned**: the rulebook, `CHANGELOG.md`, and everything under `docs/repo-harness/`. Seeded once, then edited freely. Compared only by the level-two sections they promise, ignoring dated entries. Rewriting the prose is expected and never reads as drift; deleting a promised section is reported as `sections-missing`.

This is the distinction that keeps the check useful. Comparing a rulebook by content would flag every real project as broken on day one.

## Approval

Show all four action sets and the current plan digest. When the create set is grouped or counted for the human, attribute each root pointer to the runtime that requires it, never with the common core. `CLAUDE.md` counts toward `claude-code`; `AGENTS.md` counts toward `opencode` or `pi`, and a combination selecting both counts the one shared file once. Only `REPO-HARNESS.md` and `CHANGELOG.md` are common. A summary that inflates the common count understates what each profile costs. Before asking, state `Answers are case-sensitive and must be written exactly in uppercase.` Then show exactly `APROBAR` and `DENEGAR`. Only exact `APROBAR` permits writing; exact `DENEGAR` stops. Reject every other answer with `Invalid answer. Write exactly APROBAR or DENEGAR in uppercase.` Never normalize an answer or reuse an earlier preview digest. Apply requires the approved current digest and fails closed when pointer bytes or another planned target changed. Approval of repository writes never approves Git, provider, backup, or publication actions.

## Containment And Its Honest Limit

Reject unsafe relative paths, absolute paths, parent traversal, invalid existing targets, and indirect existing ancestors before any write. Apply the same ancestor guard before every check read. After each create, confirm the resolved path is still inside the selected root and remove it if it is not.

This is proportional containment for a cooperative working copy. It is not a security boundary against an attacker who can replace directories underneath a running process. Version control is the real safety net, and the tool says so rather than implying a guarantee it does not provide.

## Failure Behavior

Refuse indirect targets and unsafe paths. Verify created bytes, pointer result digests, and links after writing. On failure, remove only files this run created and only when their bytes still match what this run wrote. Never replace or delete pre-existing content.

## Generated Checker Boundary

The generated checker trusts no mutable manifest field until the exact manifest bytes match its embedded SHA-256. It then requires the exact checker schema, an `integrationMode` of `integrated` or `standalone`, supported runtimes, safe canonical skill names, and repository-relative paths. It rejects absolute, drive-qualified, parent-traversing, symlinked, reparse-point, and outside-resolving paths before content reads. Its scope remains manifest integrity and canonical runtime skill projections only.
