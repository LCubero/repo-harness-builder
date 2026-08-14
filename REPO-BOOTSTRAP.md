# Repo Bootstrap Handoff

Read this before creating the standalone `repo-harness-builder` repository. It carries the knowledge that currently lives in `skills-pack-ld9` and would otherwise be lost, because the builder generates an empty harness shell and knows nothing about its own history.

This file is a completed historical handoff, not an operating document. Current policy lives in `REPO-HARNESS.md` and `docs/repo-harness/decisions.md`.

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
- Dogfooded end to end against `E:\code\02-Proyectos\bionic-reader`, producing 26 files and a generated checker reporting `current`.
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

Seed these into `docs/repo-harness/decisions.md` in the new repository.

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

## Existing Review Material

A findings-only review of the builder exists at `.ld9/skill-sanitize/skill_review/found/repo-harness-builder.md` in `skills-pack-ld9`, with the prior report preserved alongside it as `repo-harness-builder.superseded-2026-08-11T1422.md`. Carry the current report over and leave the superseded one behind.

## Pending Work In The New Repository

1. Create the repository and place this package as its single canonical source.
2. Bootstrap the repository harness by running the builder against the new repository itself.
3. Seed `CHANGELOG.md` from the history section above.
4. Seed `docs/repo-harness/decisions.md` from the decisions section above.
5. Write the product guide, which is distinct from the generated harness usage guide. It covers what the builder does, how to invoke it, its version policy, and its release process. The generated `docs/repo-harness/usage.md` documents the harness workflow, not the product.
6. Carry over the current review report.
7. Define how a consumer repository detects that its installed copy of the builder is stale against canonical. See the open gap below.
8. Deferred and optional: a presentation pass for harness user messages. Explore portable Markdown headings, simple callout blocks, and restrained emoji for session start, alerts, decisions, and session close. Adopt only what renders clearly across operating systems and agents. Avoid runtime-specific formatting and anything that complicates the builder.

## Open Gap Found During Handoff

In `skills-pack-ld9`, the runtime copy at `.claude/skills/repo-harness-builder/` is not covered by `python .repo-harness/check.py`. That checker reports on six files only, the three session skills across two runtimes. The builder projection can therefore drift from canonical without detection, which means a consumer can invoke a copy that is not the version it believes it has. This gap becomes more relevant once canonical lives in a separate repository, and item 7 above is the work it implies.

## Origin And Historical Reference

The builder was designed and implemented in the `skills-pack-ld9` authoring workspace. Keep that workspace as the historical record. Nothing before the split is reconstructable from the new repository's own Git history, because the work predates it.

- Origin workspace: `E:\code\00-skills-pack_ld9`
- Canonical source at the time of the split: `0_mis_skills/repo-harness-builder/`
- External command wrapper: `0_mis_commands/repo-harness-builder.md`
- Review reports: `.ld9/skill-sanitize/skill_review/found/`
- Durable documentation of that workspace: `docs/repo-harness/` and `CHANGELOG.md`

Engram continuity for that period is stored under the project key `skills-pack_ld9`, bound to the path above. Search it there when the reasoning behind a decision is not recorded in this repository.

- Development window: 2026-08-10 through 2026-08-12.
- Observation `#9143`, session summary closing the builder polishing cycle, including the runtime set handling fixes.
- Observation `#9215`, the decision to split the builder into its own repository.
- Useful search terms: `repo-harness-builder`, `runtime selection contract`, `repo-harness-help`, `usage guide`, `tool-owned path boundary`, `preflight`.

Treat Engram as a continuity input, not as durable truth. Anything recovered from it must be reconciled against repository evidence before it is promoted into a canonical document.

## Follow-Up Required In skills-pack-ld9

Once the split happens, `REPO-HARNESS.md` in `skills-pack-ld9` still states that `0_mis_skills/` holds the canonical hand-maintained copies and is the only copy that may be edited. That statement stops being true for the builder and must be corrected in the same change.
