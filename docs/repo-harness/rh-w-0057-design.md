# RH-W-0057 Design: Canonical Commands And Wrapper Coverage

Status: completed on 2026-08-23 after explicit implementation authorization and focused RED/GREEN evidence. The canonical [roadmap record](roadmap.md#rh-w-0057) owns lifecycle and verification evidence; this design remains the behavior authority.

## The Defect, Measured

`.repo-harness/check.py` compares skill projections and nothing else. `SKILL_SURFACES` holds exactly two entries, both `skills/{name}/SKILL.md`, and `check_projections()` iterates only `manifest["canonicalSkills"]`. No command or prompt path is ever opened. A generated command wrapper that is deleted, truncated, or edited stays `current` forever.

The builder does own those files. `harness_tool.py` writes `{base}/commands/{command}.md` for every entry of `COMMAND_TO_SKILL` plus one `repo-harness-help` wrapper, and `.pi/prompts/{command}.md` for the Pi runtime. So the harness generates files that no control in the repository ever checks.

This matters in a consumer repository specifically. The builder is not installed there, as `check_script()` itself states, so `.repo-harness/check.py` is the only local control that exists. A missing wrapper there is silent until a human happens to notice a command that stopped working.

## Root Cause

`.repo-harness/` contains `check.py`, `manifest.json`, `rules/`, and `skills/`. There is no `commands/`. A skill projection can be compared because a canonical peer exists to compare it against; a command wrapper has no peer, so there is nothing to compare it to. The gap is structural, not an oversight in the checker's loop.

## Decision 1: Canonical Layout

Canonical command sources live beside `.repo-harness/skills/`, one file per generated command per surface family: `.repo-harness/commands/{command}.md` for the runtimes that load a command, and `.repo-harness/prompts/{command}.md` for the runtimes that load a prompt. Decision 3 gives the measurement that forces two families rather than one.

This is chosen over recording a wrapper digest in the manifest because a canonical peer keeps the checker's existing shape: read canonical, read projection, compare. A digest field would add a second comparison mechanism and would make the manifest grow every time a command is added. The unit's exclusions already reject the digest approach for this reason.

## Decision 2: Manifest Declaration

The manifest gains one field, `canonicalCommands`, keyed by command name and then by surface family:

```json
"canonicalCommands": {
  "session-start": {
    "commands": ".repo-harness/commands/session-start.md",
    "prompts": ".repo-harness/prompts/session-start.md"
  }
}
```

Only the families the render actually produced appear under a command, so a repository without Pi carries no `prompts` entry and a repository with only Pi carries no `commands` entry.

One field rather than two is deliberate. The generated checker requires exact manifest field-set equality, so an optional second field would either have to always exist, sometimes empty, or break that check. Nesting keeps the field always present and always non-empty while letting the families vary.

The checker validates it the way it validates `canonicalSkills`, one level deeper: a non-empty object, each command key matching `[a-z0-9]+(?:-[a-z0-9]+)*`, each value a non-empty object whose keys are `commands` or `prompts` and whose values pass `safe_relative()`.

## Decision 3: Which Surfaces Are Compared

Every recorded runtime is compared, Pi included. The checker already iterates `manifest["runtimes"]`, so a surface is compared exactly when the repository declares that runtime and skipped when it does not. Nothing needs a special case for presence.

Pi is the runtime that most needs this. Verified after adding Pi to this repository on 2026-08-23: the builder generates no `.pi/skills/` at all, so Pi's only generated surface is its four `.pi/prompts/*.md` command prompts. The current checker compares six files, all of them Claude Code and OpenCode skill projections. Pi is therefore not partially checked today; it is entirely unchecked, and the control that would cover it is precisely the one this unit adds.

The obstacle is one word. Verified: `wrapper(runtime, skill_name)` renders `skill mechanism` for `claude-code` and `opencode` and `prompt mechanism` for `pi`. Verified on disk: all five wrappers under `.claude/commands/` are byte-identical to their `.opencode/commands/` peers, and `.pi/prompts/session-start.md` differs from `.claude/commands/session-start.md` in exactly that word. Verified: `help_wrapper()` accepts a `runtime` argument it never reads, so the help surface does not vary by runtime at all.

The skills solve their equivalent problem with `comparable()`, which drops the whole `canonical-source:` line. That does not transfer, because the Pi difference is a word inside a sentence and normalizing it would mean teaching the checker which words a runtime is allowed to change. That rule rots the first time a wrapper is reworded.

So the canonical source is stored per surface family, one file per command per family:

| Canonical | Compared against | Created when |
|---|---|---|
| `.repo-harness/commands/{command}.md` | `.claude/commands/{command}.md`, `.opencode/commands/{command}.md` | `claude-code` or `opencode` is recorded |
| `.repo-harness/prompts/{command}.md` | `.pi/prompts/{command}.md` | `pi` is recorded |

Each canonical file is byte-identical to the surfaces it covers, so the comparison stays a plain byte comparison with no normalization and no runtime-aware rules. A family whose runtimes are not recorded produces no canonical directory, exactly as `.pi/prompts/` does not exist until Pi is selected. The cost is one extra canonical file per command only for repositories that actually use Pi, and it buys Pi the same coverage the other two get instead of a documented blind spot.

## Decision 4: Installs That Predate The Field

No compatibility shim is needed inside the generated checker, and adding one would be wrong.

The checker authenticates the manifest by exact SHA-256 before reading any field, and then requires exact field-set equality. An old checker paired with a new manifest already fails loudly with `manifest digest mismatch`, and the existing contract replaces the checker and its manifest together in one transaction. An installed harness that predates `canonicalCommands` is therefore updated by the ordinary one-pass update, which is the mechanism `RH-W-0043` already delivered.

The builder's own `check()` is different and must tolerate the absence. It reads installed manifests it did not necessarily write. When `canonicalCommands` is absent it reports command coverage as `undeclared` and continues, because a builder that crashes on an older install cannot be the tool that updates it.

## Reported States

Reuse the existing vocabulary rather than inventing one:

| State | Meaning |
|---|---|
| `current` | Projection matches its canonical peer |
| `stale` | Projection exists and differs |
| `missing` | Projection absent |
| `canonical-missing` | Canonical peer absent, so nothing can be compared |

`drift` remains any state other than `current`. No `unchecked` state is needed, because every recorded runtime is compared and an unrecorded one produces no surface to compare.

## Exclusions

No writes from the checker, no repair or regeneration mode, no change to skill projection comparison, no wrapper digest field in the manifest, and no change to `wrapper()`. Making the wrapper text runtime-neutral would collapse the two canonical families into one, but it changes generated bytes for every Pi installation and belongs to its own unit with its own migration reasoning.

## Incidental Observation, Not Scope

`help_wrapper()` takes a `runtime` parameter it never reads. That is a dead parameter, not a defect, and it is recorded here so the next reader does not assume the help wrapper varies by runtime. Removing it is not part of this unit.

## What Must Fail Before Implementation

Focused tests that fail first:

- The generated checker reports a deleted command wrapper as `missing`.
- It reports an edited wrapper as `stale`.
- It reports an absent canonical peer as `canonical-missing`.
- A manifest declaring `canonicalCommands` with an unsafe or absolute path, an unknown family key, or an empty family object is rejected before any read.
- The builder's own `check()` reports `undeclared` for an installed manifest without the field instead of raising.
- A deleted `.pi/prompts/{command}.md` is reported `missing` when `pi` is recorded, and no Pi surface is reported at all when it is not.
