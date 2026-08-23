# Repo Bootstrap Handoff (Archived)

Archived on 2026-08-23. This is a historical record of the 2026-08-12 split that created this repository, kept for provenance only. It is not policy, not a plan, and not a source of pending work. Current policy lives in `REPO-HARNESS.md` and [decisions.md](../decisions.md); current work lives in [roadmap.md](../roadmap.md).

Everything this document once instructed a reader to fetch from outside this repository was removed when it was archived, because nothing here depends on another workspace. The removed text remains recoverable from this file's Git history. Its four uncarried design decisions were promoted into [decisions.md](../decisions.md) on the same day, and its two surviving pending items became `RH-W-0062` and `RH-W-0063`.

## Decision Behind The Split

`repo-harness-builder` moves to its own Git repository, which becomes the single canonical source. `skills-pack-ld9` becomes a consumer only and stops owning the source.

Reasons:

- `skills-pack-ld9` is not a Git repository. The only versioned product in that workspace has no history and no revert path.
- The builder has its own release lifecycle. It is versioned, tested, and has been used against an external repository.
- `0_mis_skills/` mixes canonical authoring material with imported reference packs. A versioned product should not share a root with material that is never published.

The origin workspace is a skill creator, not a distribution target. A finished skill there takes one of two exits: its own repository, or the publishable pack in `0_github/`. This builder takes the first exit because it is a versioned product with its own release lifecycle, not a pack entry.

Accepted tradeoff: synchronization cost. `skills-pack-ld9` will consume an installed copy instead of owning the source, and needs an explicit way to detect that its copy is stale.

## Product State As Of 2026-08-12

Verified in `skills-pack-ld9` at the time of writing:

- Skill package version `3.1`, declared in `SKILL.md` frontmatter.
- Generator version `3.0`. Rendered output did not change when the package went to `3.1`, so the two version numbers are intentionally different.
- Test suite: 116 tests passing with five expected skips. One skip is a Windows symlink privilege check, four are optional external wrapper checks.
- Both integrated and standalone self-tests report `current`.
- Dogfooded end to end against a separate repository, producing 26 files and a generated checker reporting `current`.
- Author: Luis Diego Cubero Chavarria. License: CC BY-NC 4.0.

## Package Structure

```
SKILL.md
assets/audit-report.template.md
assets/validation-report.template.md
references/architecture.md
references/audit-and-interview.md
references/gentle-ai-style-guide.md
references/preservation-and-safety.md
references/validation.md
scripts/harness_tool.py
tests/test_harness_tool.py
```

Implementation constraint: Python 3.10 or later, standard library only.

## Design Decisions To Carry Over

These were meant to be seeded into `docs/repo-harness/decisions.md`. Six of them were. On 2026-08-23, while archiving this document, four were found never to have been carried over and were promoted then: version control as the safety net, runtime selection never inferred, the tool-owned path boundary, and generated session-start never delegating to a subagent. The list below is kept as the historical record of the transfer, not as a second owner.

- **Historical preservation policy, superseded.** The original additive-create-only rule was replaced by the repository-sensitive policy in `docs/repo-harness/decisions.md`.
- **Version control is the safety net.** The tool is not presented as a boundary against a hostile filesystem.
- **Wrappers load by registered name, never by path.** This applies to generated wrappers and to the external command wrapper.
- **Tool-owned path boundary.** The builder claims no path under `.atl`, `.codegraph`, or `.engram`; its sole ignore-file behavior is the approved exact root `.codegraph/` directive.
- **Runtime selection is never inferred.** A runtime set is never derived from surfaces found on disk. The CLI takes a repeatable `--runtime` form, and naming a runtime set is itself the permission to plan.
- **Root pointer attribution.** `CLAUDE.md` counts with `claude-code`. `AGENTS.md` counts with `opencode` or `pi`. Only `REPO-HARNESS.md` and `CHANGELOG.md` are common to every runtime.
- **Integrated preflight is local.** Defined as `gentle-ai version` plus one successful local Engram MCP call. It does not require Engram Cloud health or sync.
- **Language split.** User-facing reports are localized. Generated artifacts stay in English.
- **One blocking decision per turn.** The builder's content-bound apply plan uses case-sensitive uppercase `APROBAR` or `DENEGAR`, and a digest is approved once and never reused. Other choices are conversational.
- **Generated session-start must not delegate to a subagent.** The purpose of session-start is to load evidence into the main thread. A subagent compresses and returns a summary, which discards the exact evidence the parent needs. Delegation exists to keep noise out of the parent, and here the output is the point.

## Builder History To Seed Into The New Changelog

These entries currently sit in the `skills-pack-ld9` changelog but describe the builder product, not that workspace. Dated 2026-08-11.

- Added generated `docs/repo-harness/usage.md` and linked it from the generated rulebook and documentation map.
- Added selected-runtime read-only `repo-harness-help` wrappers for Claude Code, OpenCode, and Pi.
- Defined integrated preflight as `gentle-ai version` plus a successful local Engram MCP call, without requiring Engram Cloud health or sync.
- Narrowed the runtime set warning to the direction that needs it: only a runtime the manifest never recorded reports `unrecorded` and `staleManifest`.
- Added `runtimeSet` detection so a runtime set that differs from the installed manifest is surfaced before approval, and `check` defaults to the recorded set when no runtime flag is supplied.
- Split validation commands by audience, so the packaged test suite is no longer run when building a harness in a repository.
- Made a supplied language argument binding as a bare token, in the skill contract and in the external command wrapper.
- Corrected root pointer attribution in both the audit and the plan summary.
- Replaced the compound runtime question with a single blocking decision.
- Added a runtime selection contract, plus the repeatable `--runtime` form in the CLI help and the validation reference.
- Locked the tool-owned path boundary; roadmap unit 2 later added the bounded root `.codegraph/` ignore exception without writing inside the tool directory.
- Decoupled the packaged test suite from a host-specific command path, so it passes wherever the skill is installed.
- Bumped the skill package to version 3.1 while the generator version stayed 3.0.
- Closed eight defects and locked each with tests.

Entries that stay with `skills-pack-ld9` and must not be copied: creating that workspace's own harness, and applying the usage guide and help wrappers to that workspace's active harness. Those describe a consumer, not the product.

## Pending Work In The New Repository (Resolved)

Items 1 through 6 were completed while this repository was built: the package became its single canonical source, the harness was bootstrapped against the repository itself, `CHANGELOG.md` and `docs/repo-harness/decisions.md` were seeded, the product guide was written, and historical findings were reconciled against repository evidence.

The two items that were still open when this document was archived were moved to the canonical lifecycle owner, because a pending item living in an archive has no owner:

- Item 7, how a repository that installed the builder detects that its copy is stale against canonical, is now `RH-W-0062`.
- Item 8, the deferred and optional presentation pass over generated user messages, is now `RH-W-0063`, registered `deferred` to preserve its original disposition.
