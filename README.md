# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

**English** | [Español](./README.es.md)

<!-- README-I18N:END -->

`repo-harness` is a repository-scoped system of prompts, guardrails, harness rules, durable documentation, skills, and commands that strengthens and assists AI coding agents such as OpenCode, Claude Code, and Pi. It keeps working rules visible, project state current, runtime instructions portable, and handoffs verifiable without taking ownership away from maintainers.

`repo-harness-builder` is the skill and tool that creates, audits, installs, and safely evolves `repo-harness` inside a selected repository.

## Product Positioning
<!-- doc-role:positioning -->

The harness remains useful on its own. Default integrated mode composes available Gentle AI and Engram capabilities independently, while CodeGraph remains optional. An unavailable capability degrades only its owned behavior unless the selected operation or repository policy explicitly requires it. Explicit `--standalone` mode uses repository-only continuity.

| Component | Boundary |
|---|---|
| `repo-harness` | Repository-scoped operating guidance and durable project truth for supported AI coding agents. |
| Gentle AI | A broader AI-agent ecosystem that may provide routing, review, receipts, and delivery gates. |
| Engram | Cross-session semantic memory and continuity. It never replaces repository truth. |
| CodeGraph | The most optional component: a repository-local aid for locating files, symbols, relationships, and impact. It is not an authority store. |

Hermes Agent, OpenClaw, and Pi may inform research, but recreating or copying them is not a product goal. `repo-harness` has different repository-scoped boundaries.

## Quick Start
<!-- doc-role:quick-start -->

1. Install or project the builder skill and command into a supported runtime.
2. Open the target repository and invoke `/repo-harness-builder`.
3. Answer the interview: runtime projections, license, conversation language, and documentation locales. Documentation stays English-only unless you declare another locale.
4. Keep the default root `.codegraph/` ignore, or pass `--no-codegraph-ignore` to persistently opt out.
5. Review the exact plan and use its language-scoped digest-bound approval: `APPROVE` or `DENY` in English, `APROBAR` or `DENEGAR` in Spanish.
6. Start normal work with the generated `repo-harness-session-start` skill.

With no operation, the builder performs a read-only audit. It does not initialize Git, mutate remotes, commit, push, or publish unless those separate actions are later requested and approved.

## What It Builds
<!-- doc-role:outputs -->

- Root `REPO-HARNESS.md` as the concise working entry point.
- Root `README.md` when absent, with a bounded builder-managed section.
- Complete English durable documentation under `docs/repo-harness/`.
- Runtime-neutral session and documentation skills under `.repo-harness/skills/`, plus canonical command peers under `.repo-harness/commands/` and `.repo-harness/prompts/` for emitted surface families.
- Selected Claude Code, OpenCode, or Pi runtime projections.
- `.repo-harness/check.py` for read-only projection drift checks.
- One exact root `.gitignore` directive, `.codegraph/`, unless explicitly disabled.
- Root `.gitattributes` with exact LF protection for every byte-compared generated path.

A harness is English-only unless you declare more documentation locales. See [Documentation Language](#documentation-language).

| Part | What it contributes |
|---|---|
| `REPO-HARNESS.md` | The concise, always-read rulebook: purpose, hard rules, and routing to deeper guidance. |
| `docs/repo-harness/` | Durable owners for usage, current state, and decisions, so facts have one home. |
| `CHANGELOG.md` | A dated record of meaningful completed changes, separate from current and future work. |
| Engram continuity | Optional temporary cross-session memory when available, always reconciled with repository evidence. |
| Runtime projections | Thin Claude Code, OpenCode, or Pi surfaces generated from canonical skills and family-specific command peers. |
| `.repo-harness/check.py` | A read-only congruence check that detects missing or stale skill, command, and prompt projections. |

## Builder And Generated Skills
<!-- doc-role:skills -->

The builder is the installer and upgrader. The three repository skills are the daily operating workflow after installation.

| Skill | Use it when | Outcome |
|---|---|---|
| `repo-harness-builder` | Creating, auditing, or safely upgrading a harness | A planned, approved, verified repository harness |
| `repo-harness-session-start` | Starting or resuming work | A scoped outcome, current evidence labelled with its class, local Git state, and one concrete remote-check question when safe |
| `repo-harness-documentation` | Durable facts or visible state changed | Canonical docs updated without duplication or stale ownership |
| `repo-harness-session-stop` | Pausing or ending work | Verified continuity, screened exact-path work units, and one outcome-scoped Git question when needed |

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

Integrated mode composes available Gentle AI and Engram capabilities independently and withholds only guarantees owned by an unavailable capability. Engram can bridge sessions when available, but it never overrides current repository files. Explicit standalone mode remains repository-only.

## Example Workflow
<!-- doc-role:example -->

```text
/repo-harness-builder
# Review the audit, answer one choice at a time, inspect the plan, then approve.

Load repo-harness-session-start and continue the API migration.
# Work, test, and verify.

Load repo-harness-documentation and update current state and the changelog.

Load repo-harness-session-stop and prepare the handoff.
```

The builder command remains `/repo-harness-builder`; the daily skills have the fixed registered names shown above. Runtime command syntax varies, so invoke skills through the runtime's skill mechanism. The generated help command reads `docs/repo-harness/usage.md` for concise repository-specific guidance.

Generated session outcomes are `ready`, `paused`, `blocked`, `failed`, `completed`, and `degraded`. They are scoped: optional Git failure can degrade a successful local session without falsely failing continuity. Explicit `commit and push` consent covers the unchanged safe sequence through one push; commit-only never implies push, and material changes pause for renewed consent.

## Repository Behavior
<!-- doc-role:repository-behavior -->

### New Repositories

A new or effectively empty repository receives a complete useful baseline after approval, including a root README, rulebook, durable docs, daily skills, selected projections, manifest, and checker.

### Existing Repositories

The builder analyzes relevant existing content and proposes informed integration. It preserves unknown bytes and never treats preservation as freezing the repository. If content is obsolete or structurally incompatible, archive and clean-baseline work is a distinct choice that still requires explicit approval.

An existing repository is never given a documentation locale it does not already have. A legacy harness whose manifest predates language metadata keeps the locale trees it actually has installed, so an older bilingual repository does not silently lose its Spanish checks and an English-only repository does not silently gain Spanish files.

### Documentation Language

Human repository documentation may be English-only, Spanish-only, or bilingual. Omit `--documentation-language` for English-only, pass `--documentation-language es` for Spanish-only, or repeat `en` then `es` for bilingual output.

English-only and Spanish-only use canonical paths such as `README.md`, `CHANGELOG.md`, and `docs/repo-harness/`. Bilingual output keeps English at canonical paths and adds complete Spanish peers. Skills, contracts, wrappers, checker machinery, manifest instructions and schema, code, comments, runtime instructions, and technical artifacts stay in English in every mode.

When a second locale is declared, it is generated complete, not partially. Its documents are full human mirrors that present the same facts and link back to the English owners. They never become independent factual owners.

The conversation language is a separate choice. It sets the language the generated session skills speak to you in. It does not add or remove documentation locales.

Reciprocal README selectors are one atomic integration. If a repository already has `README.md` and the exact executable plan cannot update it, `README.es.md` is classified as blocked rather than created. A one-way selector that claims a bilingual repository is never produced.

### Safe Upgrades

Older harnesses are audited rather than blindly replaced. Managed paths are classified as `current`, `stale`, `missing`, `human-evolved`, or `legacy`. Replacement, integration, archive, and deletion remain proposal-only until explicitly approved.

The README is human-owned after creation. Its bounded builder section identifies the initial harness entry points, while maintainers may evolve the full product guide. Existing `CLAUDE.md` and `AGENTS.md` files are byte-preserved: the builder appends only an exact missing pointer line, and repeated runs are idempotent.

Root `.gitignore` is also human-owned. By default, the approved plan creates it when absent or appends exactly `.codegraph/` when that exact line is missing, preserving BOM, existing bytes, comments, blank lines, line endings, and unrelated rules. `--no-codegraph-ignore` records the opt-out in the manifest and performs no ignore-file mutation.

Root `.gitattributes` remains human-owned. The builder creates or appends one exact `<path> text eol=lf` rule for every byte-compared generated path, preserving unrelated rules and existing line endings. A matching rule that assigns incompatible text or EOL behavior blocks the plan instead of creating ambiguous checkout semantics.

Operational facts remain single-owned. `REPO-HARNESS.md`, `CHANGELOG.md`, and the English durable state owners are canonical. When another locale is declared, its peers are complete localized presentations that link back to those owners. See the [bilingual maintenance contract](docs/repo-harness/i18n-maintenance.md), which applies only to a repository that declared more than one locale.

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
