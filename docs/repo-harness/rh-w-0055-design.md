# RH-W-0055 Installation And Onboarding Design

> **Status: RH-W-0055 is `completed`.** The [roadmap record](roadmap.md#rh-w-0055) owns its outcome; the text below is retained as historical evidence.

Status: `proposed`; design owner-approved on 2026-09-06. This approval does not authorize implementation planning, implementation, publication, or network access.

## Problem And Boundary

**Verified fact:** the product supports `claude-code`, `opencode`, and `pi`; the repository already owns builder invocation and daily use in [README.md](../../README.md) and [usage.md](usage.md), while the [roadmap](roadmap.md#rh-w-0055) records no single path from a selected artifact to those owners. An existing release asset is historical evidence, not a future availability promise.

This design adds one artifact-name-agnostic LLM-first onboarding sequence: select and acquire a local builder artifact; set `ARTIFACT_ROOT` to its extracted root; validate its expected source shape; select a runtime explicitly; copy the builder package and matching entry; invoke the builder in the target repository under its existing approval contract; run the generated read-only checker; then use the existing session-start and session-stop workflow.

It adds no command, API, state, store, package-manager automation, URL, generated behavior, or release action.

## Approaches Considered

1. **One universal installer.** Smallest human command, but it creates installation behavior, platform handling, and another authority surface. Rejected as disproportionate.
2. **Independent runtime guides.** Easy to write, but repeats acquisition, verification, builder, and daily-use contracts and invites drift. Rejected.
3. **One shared path with a runtime-specific copy step.** Reuses current owners and varies only the two destinations required by each runtime. **Selected and owner-approved as the minimum coherent design.**

## Selected Contract

The future onboarding guide accepts either a trusted source checkout or an already-acquired, extracted release package. It never downloads, authenticates, publishes, or claims an artifact exists. The local artifact root must contain `skill/repo-harness-builder/` and `command/repo-harness-builder.md`; absence stops installation as an invalid artifact.

The user explicitly selects one or more runtimes. For each runtime, the guide gives a literal copy pair parameterized only by `ARTIFACT_ROOT` and the target repository root:

| Runtime | Builder package destination | Entry destination |
|---|---|---|
| Claude Code | `.claude/skills/repo-harness-builder/` | `.claude/commands/repo-harness-builder.md` |
| OpenCode | `.opencode/skills/repo-harness-builder/` | `.opencode/commands/repo-harness-builder.md` |
| Pi | `.pi/skills/repo-harness-builder/` | `.pi/prompts/repo-harness-builder.md` |

The copy is additive: an occupied destination stops for inspection and never overwrites. The user then opens the target repository and invokes its installed `repo-harness-builder` entry. The builder remains sole owner of repository audit, generated-runtime selection, preview, approval, apply, and post-apply `check`; the guide links to [Builder And Generated Skills](../../README.md#builder-and-generated-skills) instead of restating those contracts.

After successful apply, run generated `python3 .repo-harness/check.py` (`python` on Windows). Only exit 0 with `current` and zero writes establishes on-disk congruence. Runtime registration may require a fresh process when the current process cached older content; no reload command is invented. Daily work follows [Daily Workflow](usage.md#daily-workflow): session-start, documentation when durable state changes, and session-stop.

Failures stop at their owning boundary: invalid artifact before copy; occupied destination before overwrite; unavailable runtime registration before invocation; builder refusal at its existing approval boundary; and checker drift before daily-use readiness. Offline operation is supported only after the artifact is locally available and prerequisites already exist.

## Offline Identity

**Verified fact:** current distributed builder bytes carry generator/package versions, and published releases may provide an external digest. **Supported inference:** embedding another release identifier adds no trustworthy offline proof because identity bytes can be copied with the payload; only an independently obtained digest can authenticate it. A separately signed, independently verifiable package identity would overturn this conclusion.

Therefore this design finds no viable RH-W-0062 offline identity beyond existing manifest freshness and an independently retained release digest. [RH-W-0062](roadmap.md#rh-w-0062) closes as no residue under its existing advancement condition; that lifecycle reconciliation does not approve this design or advance RH-W-0055.

## Acceptance And Exclusions

The approved design requires one coherent EN/ES path, valid relative links, exact runtime destinations, explicit offline and failure boundaries, and no duplicated builder or daily-session detail. Excluded: publication, network execution, archive naming, real renames, generated behavior changes, runtime APIs or state, new stores, commands, assets, URLs, package-manager automation, implementation plans, and implementation.

## Human Gate

Owner decision recorded: the reduced bilingual design is approved. RH-W-0055 remains `proposed`; implementation planning, implementation, publication, network access, and any later phase remain unauthorized.
