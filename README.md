# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

**English** | [Español](./README.es.md)

<!-- README-I18N:END -->

`repo-harness-builder` gives a repository a small, durable operating system for AI-assisted work. It keeps working rules visible, project state current, session context recoverable, runtime instructions portable, and handoffs verifiable without taking ownership away from maintainers.

The result is less context loss between sessions, safer repository evolution, clearer documentation ownership, and the same core workflow across Claude Code, OpenCode, and Pi.

## Quick Start
<!-- doc-role:quick-start -->

1. Install or project the builder skill and command into a supported runtime.
2. Open the target repository and invoke `/repo-harness-builder`.
3. Choose the runtime projections and license when asked.
4. Review the exact plan and use its language-scoped digest-bound approval: `APPROVE` or `DENY` in English, `APROBAR` or `DENEGAR` in Spanish.
5. Start normal work with the generated `<project>-session-start` skill.

With no operation, the builder performs a read-only audit. It does not initialize Git, mutate remotes, commit, push, or publish unless those separate actions are later requested and approved.

## What It Builds
<!-- doc-role:outputs -->

- Root `REPO-HARNESS.md` as the concise working entry point.
- Root `README.md` and `README.es.md` when absent, with reciprocal language selectors and bounded builder-managed sections.
- Complete English durable documentation under `docs/repo-harness/` and Spanish peers under `docs/es/repo-harness/`.
- Runtime-neutral session and documentation skills under `.repo-harness/skills/`.
- Selected Claude Code, OpenCode, or Pi runtime projections.
- `.repo-harness/check.py` for read-only projection drift checks.

| Part | What it contributes |
|---|---|
| `REPO-HARNESS.md` | The concise, always-read rulebook: purpose, hard rules, and routing to deeper guidance. |
| `docs/repo-harness/` | Durable owners for usage, current state, and decisions, so facts have one home. |
| `CHANGELOG.md` | A dated record of meaningful completed changes, separate from current and future work. |
| Engram continuity | Temporary cross-session memory in integrated mode, always reconciled with repository evidence. |
| Runtime projections | Thin Claude Code, OpenCode, or Pi surfaces generated from runtime-neutral canonical skills. |
| `.repo-harness/check.py` | A read-only congruence check that detects missing or stale runtime projections. |

## Builder And Generated Skills
<!-- doc-role:skills -->

The builder is the installer and upgrader. The three repository skills are the daily operating workflow after installation.

| Skill | Use it when | Outcome |
|---|---|---|
| `repo-harness-builder` | Creating, auditing, or safely upgrading a harness | A planned, approved, verified repository harness |
| `<project>-session-start` | Starting or resuming work | A scoped outcome, current evidence, local Git state, and one concrete remote-check question when safe |
| `<project>-documentation` | Durable facts or visible state changed | Canonical docs updated without duplication or stale ownership |
| `<project>-session-stop` | Pausing or ending work | Verified continuity, screened exact-path work units, and one outcome-scoped Git question when needed |

## Lifecycle
<!-- doc-role:lifecycle -->

```text
First builder run
  -> read-only repository audit
  -> runtime, integration, and license choices
  -> exact plan plus digest
  -> explicit apply approval
  -> generated harness plus verification

Normal session
  -> session-start recovers and reconciles context
  -> project work happens under REPO-HARNESS.md rules
  -> documentation skill updates durable owners when needed
  -> session-stop verifies evidence and records the handoff
  -> next session-start continues from current files and continuity memory
```

In integrated mode, Gentle AI and Engram must be available for continuity. Engram helps bridge sessions, but it never overrides current repository files. In standalone mode, continuity remains repository-only.

## Example Workflow
<!-- doc-role:example -->

```text
/repo-harness-builder
# Review the audit, answer one choice at a time, inspect the plan, then approve.

Load my-project-session-start and continue the API migration.
# Work, test, and verify.

Load my-project-documentation and update current state and the changelog.

Load my-project-session-stop and prepare the handoff.
```

Runtime command names vary, so invoke skills by their registered names through the runtime's skill mechanism. The generated help command reads `docs/repo-harness/usage.md` for concise repository-specific guidance.

Generated session outcomes are `ready`, `paused`, `blocked`, `failed`, `completed`, and `degraded`. They are scoped: optional Git failure can degrade a successful local session without falsely failing continuity. Explicit `commit and push` consent covers the unchanged safe sequence through one push; commit-only never implies push, and material changes pause for renewed consent.

## Repository Behavior
<!-- doc-role:repository-behavior -->

### New Repositories

A new or effectively empty repository receives a complete useful baseline after approval, including a root README, rulebook, durable docs, daily skills, selected projections, manifest, and checker.

### Existing Repositories

The builder analyzes relevant existing content and proposes informed integration. It preserves unknown bytes and never treats preservation as freezing the repository. If content is obsolete or structurally incompatible, archive and clean-baseline work is a distinct choice that still requires explicit approval.

### Safe Upgrades

Older harnesses are audited rather than blindly replaced. Managed paths are classified as `current`, `stale`, `missing`, `human-evolved`, or `legacy`. Replacement, integration, archive, and deletion remain proposal-only until explicitly approved.

The README is human-owned after creation. Its bounded builder section identifies the initial harness entry points, while maintainers may evolve the full product guide. Existing `CLAUDE.md` and `AGENTS.md` files are byte-preserved: the builder appends only an exact missing pointer line, and repeated runs are idempotent.

Operational facts remain single-owned. `REPO-HARNESS.md`, `CHANGELOG.md`, and the English current-state owners are canonical; complete Spanish peers provide localized presentation and link back to those owners. See the [bilingual maintenance contract](docs/repo-harness/i18n-maintenance.md).

## Development
<!-- doc-role:development -->

Editable product sources are:

- `skill/repo-harness-builder/`
- `command/repo-harness-builder.md`

`.opencode/` and `.claude/` contain runtime projections used for execution and testing. Do not edit them as canonical sources. This repository self-hosts recursively: its generated repository harness is used to improve the builder that generates repository harnesses.

For self-hosting changes, edit the canonical source, synchronize `.repo-harness`, `.claude`, and `.opencode`, then verify parity on disk. If registered-name validation is needed, run it in a fresh runtime process or use a supported explicit registry reload when the runtime provides one. The process that performed the edit may retain older registered content; that expected cache does not invalidate verified files, and no reload command should be assumed.

Run the canonical suite:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Check this repository's runtime projections:

```powershell
python .repo-harness/check.py
```

## License
<!-- doc-role:license -->

Licensed under the [MIT License](LICENSE).
