# Builder Guide

Detailed product and maintenance guidance for `repo-harness-builder`. For installation, start with the [README](../../README.md); for daily work, use the [usage guide](usage.md).

## Product Positioning
<!-- doc-role:positioning -->

The harness remains useful on its own. Default integrated mode composes available Gentle AI and Engram capabilities independently, while CodeGraph remains optional. An unavailable capability degrades only its owned behavior unless the selected operation or repository policy explicitly requires it. Explicit `--standalone` mode uses repository-only continuity.

Detection does not require a global installation: native Pi Engram tools can supply `surface: "pi-tools"` evidence after a successful relevant tool response. Gentle AI detection checks PATH only, so "not found on PATH" does not mean a host's package-local integration is absent. Gentle Shell, for example, keeps a private Gentle AI copy off the agent shell's PATH by design; reports must not claim its review or coordination is unavailable. No private-folder search or installation repair is required. The legacy `applyAllowed` diagnostic is not an installation gate; optional capability warnings do not invalidate a successful harness install.

| Component | Boundary |
|---|---|
| `repo-harness` | Repository-scoped operating guidance and durable project truth for supported AI coding agents. |
| Gentle AI | A broader AI-agent ecosystem that may provide routing, exact-candidate review, receipts, and bounded review correction; repository policy owns commit, push, PR, and release. |
| Engram | Cross-session semantic memory and continuity. It never replaces repository truth. |
| CodeGraph | The most optional component: a repository-local aid for locating files, symbols, relationships, and impact. It is not an authority store. |

Hermes Agent, OpenClaw, and Pi may inform research, but recreating or copying them is not a product goal. `repo-harness` has different repository-scoped boundaries.

## Quick Start
<!-- doc-role:quick-start -->

After [installing the builder](../../README.md#install):

1. Open the target repository and invoke the skill: Claude Code `/repo-harness-builder`, Pi `/skill:repo-harness-builder`, or ask OpenCode to use the `repo-harness-builder` skill through its `skill` tool.
2. Answer the interview: runtime projections, license, conversation language, and documentation locales. Documentation stays English-only unless you declare another locale.
3. Keep the default root `.codegraph/` ignore, or pass `--no-codegraph-ignore` to persistently opt out.
4. Review the exact plan and use its language-scoped digest-bound approval: `APPROVE` or `DENY` in English, `APROBAR` or `DENEGAR` in Spanish.
5. Start normal work with the generated `repo-harness-session-start` skill.

On an update, the builder reuses the languages the installed manifest recorded instead of asking again, and when the plan has nothing to create, append, replace, or migrate it asks for no approval, and reports the harness current only when nothing is blocked. Its read-only Git queries are part of the audit and need no separate permission.

An explicit inspect/check request stops after the read-only audit. Ordinary skill activation continues through the interview and plan, then waits for approval before writing. Git, a remote, and publication are not required; those actions need separate authorization.

The builder owns audit, preview, approval, and apply. After installing or replacing a skill, use Pi's `/reload`, or start a new session; in other runtimes, use their supported refresh or a new session. After apply, run `python3 .repo-harness/check.py` (`python` on Windows) in the target: only exit 0 with `"status": "current"` and `"writes": 0` means the harness is congruent. Daily work follows the [Daily Workflow](usage.md#daily-workflow).

Stop and inspect instead of forcing when the artifact is invalid, a destination is occupied, the runtime does not list the skill, the builder refuses, or the checker reports drift. Once the artifact and prerequisites are local, no network access is needed.

## What It Builds
<!-- doc-role:outputs -->

- Root `REPO-HARNESS.md` as the concise working entry point.
- Root `README.md` when absent, with a bounded builder-managed section.
- Complete durable documentation in the selected languages, at the paths described in [Documentation Language](#documentation-language).
- Runtime-neutral session and documentation skills under `.repo-harness/skills/`, plus canonical command peers under `.repo-harness/commands/` and `.repo-harness/prompts/` for emitted surface families.
- Selected Claude Code, OpenCode, or Pi runtime projections.
- `.repo-harness/check.py` for ordinary read-only projection drift checks (`writes: 0`); only explicit `repair --apply --approved-plan-sha256 <digest>` may create missing-local projections.
- Root `.gitignore` directives: `.codegraph/` unless explicitly disabled, plus the four harness local-state directives (`/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/`, `/.repo-harness/memory-bk/`).
- Root `.gitattributes` with exact LF protection for every byte-compared generated path.

A harness is English-only unless you declare more documentation locales. See [Documentation Language](#documentation-language).

| Part | What it contributes |
|---|---|
| `REPO-HARNESS.md` | The concise, always-read rulebook: purpose, hard rules, and routing to deeper guidance. |
| `docs/repo-harness/` | Durable owners for usage, current state, and decisions, so facts have one home. |
| `CHANGELOG.md` | A dated record of meaningful completed changes, separate from current and future work. |
| Session continuity | Required structured local backups under `.repo-harness/memory-bk/`, plus optional Engram continuity when available; repository evidence remains authoritative. |
| Runtime projections | Thin Claude Code, OpenCode, or Pi surfaces generated from canonical skills and family-specific command peers. |
| `.repo-harness/check.py` | Read-only verification of generated copies; see the check command above. |

## Builder And Generated Skills
<!-- doc-role:skills -->

The builder is the installer and upgrader. The three repository skills are the daily operating workflow after installation.

| Skill | Use it when | Outcome |
|---|---|---|
| `repo-harness-builder` | Creating, auditing, or safely upgrading a harness | A planned, approved, verified repository harness |
| `repo-harness-session-start` | Starting or resuming work | Repository-first evidence, the newest valid local backup, optional scoped Engram context, and visible reconciliation |
| `repo-harness-documentation` | Durable facts or visible state changed | Canonical docs updated without duplication or stale ownership |
| `repo-harness-session-stop` | Pausing or ending work | An exclusive verified local backup, independent Engram outcome, screened work units, and any required Git question |

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
  -> session-start reads repository evidence, then local backup, then optional Engram context
  -> project work happens under REPO-HARNESS.md rules
  -> documentation skill updates durable owners when needed
  -> session-stop verifies evidence and writes the local-first handoff
  -> next session-start continues from current files and reconciled continuity
```

Both modes use the native local backup. Integrated mode composes available Gentle AI and Engram capabilities independently. Engram can bridge sessions when available, but it never overrides current repository files; standalone uses no external continuity component.

## Example Workflow
<!-- doc-role:example -->

```text
# Claude Code: /repo-harness-builder; Pi: /skill:repo-harness-builder
# OpenCode: ask the agent to use the repo-harness-builder skill.
# Review the audit, answer one choice at a time, inspect the plan, then approve.

Load repo-harness-session-start and continue the API migration.
# Work, test, and verify.

Load repo-harness-documentation and update current state and the changelog.

Load repo-harness-session-stop and prepare the handoff.
```

The builder is invoked directly as a skill, without a separate command/prompt wrapper; the daily skills retain their generated entries and fixed registered names shown above. Runtime command syntax varies, so invoke skills through the runtime's skill mechanism. The generated help command reads `docs/repo-harness/usage.md` for concise repository-specific guidance. It reads `docs/es/repo-harness/usage.md` only when Spanish is a declared documentation locale and the conversation language is Spanish; it never names a guide that the same render did not create.

Generated session outcomes are `ready`, `paused`, `blocked`, `failed`, `completed`, and `degraded`. They are scoped: optional Git failure can degrade a successful local session without falsely failing continuity. Explicit `commit and push` consent covers the unchanged safe sequence through one push; commit-only never implies push, and material changes pause for renewed consent.

## Repository Behavior
<!-- doc-role:repository-behavior -->

### New Repositories

A new or effectively empty repository receives a complete useful baseline after approval, including a root README, rulebook, durable docs, daily skills, selected projections, manifest, and checker.

### Existing Repositories

Builder `check` does not require a preserved product README to adopt the harness template. Root READMEs are checked against the harness section contract only when they declare its `repository-harness` role or either harness boundary; a bilingual language selector alone does not count. Declared harness sections and other documentation remain validated. Removing every declaration loses that evidence; no installation history is inferred.

The builder analyzes relevant existing content and proposes informed integration. It preserves unknown bytes and never treats preservation as freezing the repository. If content is obsolete or structurally incompatible, archive and clean-baseline work is a distinct choice that still requires explicit approval.

An existing repository gains a documentation locale only when the user explicitly requests that change. A legacy harness whose manifest predates language metadata keeps the locale trees it actually has installed, so an older bilingual repository does not silently lose its Spanish checks and an English-only repository does not silently gain Spanish files.

### Documentation Language

Human repository documentation may be English-only, Spanish-only, or bilingual. Omit `--documentation-language` for English-only, pass `--documentation-language es` for Spanish-only, or repeat `en` then `es` for bilingual output.

English-only and Spanish-only use canonical paths such as `README.md`, `CHANGELOG.md`, and `docs/repo-harness/`. Bilingual output keeps English at canonical paths and adds complete Spanish peers. Skills, contracts, wrappers, checker machinery, manifest instructions and schema, code, comments, runtime instructions, and technical artifacts stay in English in every mode.

When a second locale is declared, it is generated complete, not partially. Its documents are full human mirrors that present the same facts and link back to the English owners. They never become independent factual owners.

Conversation language is independent of documentation locales. It sets the language the generated session skills speak to you in. When no language can be resolved from explicit or recorded choices, the builder asks once in both English and Spanish. That answer sets report and conversation language; all subsequent interaction uses the selected language without asking again. On update, recorded languages are reused unless you request a change.

Approval text is relayed exactly from the localized CLI prompt, and plan counts come from the tool output. An empty executable plan needs no approval, empty action table, or digest; blocked entries still prevent a claim that the harness is current. A contribution guide matching the builder's own brief template receives no outdated-guide review suggestion.

You can persist the conversation language and ADHD-friendly or plain-language response modes by hand in a [local preference file](usage.md#response-preferences), or explicitly ask the agent to save them; it never saves preferences on its own.

Reciprocal README selectors are one atomic integration. If a repository already has `README.md` and the exact executable plan cannot update it, `README.es.md` is classified as blocked rather than created. A one-way selector that claims a bilingual repository is never produced.

### Safe Upgrades

Updating the builder skill does not update an installed harness. Download the new runtime archive, inspect the existing builder folder, and approve replacing only that skill. Refresh the runtime as above, invoke `repo-harness-builder`, and ask it to update this repository's harness. Review the selected runtimes explicitly; do not infer them from installed tools. Approve the resulting plan before applying.

Older harnesses are audited rather than blindly replaced. Managed paths are classified as `current`, `stale`, `missing`, `human-evolved`, or `legacy`. Replacement, integration, archive, and deletion remain proposal-only until explicitly approved. An upgrade replaces generated skills but keeps human-owned files, so `human-evolved` can mean a human edit or text an earlier version generated; after an upgrade the agent flags those files as possibly outdated. An approved comparison with a fresh render may be needed; preservation does not certify that their guidance is current.

The README is human-owned after creation. Its bounded builder section identifies the initial harness entry points, while maintainers may evolve the full product guide. Existing `CLAUDE.md` and `AGENTS.md` files are byte-preserved: the builder appends only an exact missing pointer line, and repeated runs are idempotent.

Root `.gitignore` is also human-owned. The approved plan creates it when absent or appends missing exact directives, preserving BOM, existing bytes, comments, blank lines, line endings, and unrelated rules. It includes `.codegraph/` by default and always includes `/.repo-harness/tmp/`, `/.repo-harness/archive/`, `/.repo-harness/state/`, and `/.repo-harness/memory-bk/`. `--no-codegraph-ignore` records the opt-out in the manifest and suppresses only the `.codegraph/` directive; the four local-state directives remain, and existing ignore lines are never removed. When existing rules would ignore planned harness paths (often `.claude/`, `.opencode/`, `.pi/`, `AGENTS.md`, or `CLAUDE.md`), the agent asks before approval whether to keep them local (the default) or version them, and edits `.gitignore` only after approval.

Root `.gitattributes` remains human-owned. The builder creates or appends one exact `<path> text eol=lf` rule for every byte-compared generated path, preserving unrelated rules and existing line endings. A matching rule that assigns incompatible text or EOL behavior blocks the plan instead of creating ambiguous checkout semantics.

Operational facts remain single-owned. `REPO-HARNESS.md`, `CHANGELOG.md`, and the English durable state owners are canonical. When another locale is declared, its peers are complete localized presentations that link back to those owners. See the [bilingual maintenance contract](i18n-maintenance.md), which applies only to a repository that declared more than one locale.

## Development
<!-- doc-role:development -->

Editable product sources are:

- `skill/repo-harness-builder/`

`.opencode/`, `.claude/`, and `.pi/` contain runtime projections used for execution and testing. Do not edit them as canonical sources. This repository self-hosts recursively: its generated repository harness is used to improve the builder that generates repository harnesses. See [CONTRIBUTING.md](../../CONTRIBUTING.md) for the contribution workflow.

For self-hosting changes, edit the canonical source, synchronize affected managed outputs in `.repo-harness/`, `.claude/`, `.opencode/`, and `.pi/` through maintenance preview and current-digest apply, then verify parity on disk. If registered-name validation is needed, run it in a fresh runtime process or use a supported explicit registry reload when the runtime provides one. The process that performed the edit may retain older registered content; that expected cache does not invalidate verified files, and no reload command should be assumed.

Run the canonical suite from the repository root:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Check this repository's runtime projections from the repository root:

```powershell
python .repo-harness/check.py
```

For manual removal, follow [Removing The Harness Manually](usage.md#removing-the-harness-manually). Existing help reads that guide without deleting anything. New installs receive the section; existing human-owned usage guides need an explicitly approved documentation edit, not automatic replacement.
