# Harness Architecture

## Canonical Core

Root `REPO-HARNESS.md` is the stable concise visible entry point. `.repo-harness/manifest.json` records the project, enabled runtimes, canonical skill names, write semantics, and exactly one `integrationMode`: `integrated` by default or `standalone` for `--standalone`. Canonical runtime-neutral skills live below `.repo-harness/skills/`. There is no ownership ledger: the renderer is deterministic, so the expected content of any managed file is recomputed on demand instead of stored.

The rulebook answers how work is done here and never where the project stands, so it carries a token budget and a gate table instead of the detail itself. `.repo-harness/rules/` is where that detail goes down to: seeded with `memory.md` and `working-agreements.md`, each loaded only when its gate fires, each under the same budget. Without this directory the budget rule would have no exit and would break on the first project with real rules.

Durable detail lives in `docs/repo-harness/`: a concise human usage guide, documentation ownership, verified current state, recent completed work, pending work, next step, decisions, and constraints. Root `CHANGELOG.md` records concise dated completed changes, not plans. Session output and memory remain temporary until verified and promoted to a canonical owner.

## Generated Skills

- `<project>-session-start`: read-only repository and documentation recovery, discrepancies, remaining work, and next step. Integrated mode requires Engram recovery and reconciliation; standalone mode uses repository files only.
- `<project>-session-stop`: evidence-based handoff, path validation, approval-gated documentation and meaningful changelog updates, and separate optional external gates. Integrated mode requires significant observations and a session summary saved to Engram with visible failure semantics; standalone mode records continuity only in repository files.
- `<project>-documentation`: ownership, maps, links, freshness, duplication control, factual changelog maintenance, and durable versus temporary state.

## Runtime Projection

OpenCode and Claude-style profiles receive generated skill projections and thin commands that invoke registered skill names. Pi receives thin prompts through `.pi/prompts/`. Each enabled profile also receives `repo-harness-help`, a thin read-only wrapper that reads `docs/repo-harness/usage.md` and presents concise help without loading the builder or duplicating the guide. If the guide is missing, it reports an incomplete or outdated harness and recommends running the builder in read-only inspection mode. Disabled profiles produce no output. No wrapper contains an installation path.

Claude Code also requires root `CLAUDE.md` with the exact line `@REPO-HARNESS.md`. OpenCode and Pi require root `AGENTS.md` with the exact line `Read and follow REPO-HARNESS.md before working in this repository.` Runtime combinations use the union of those two files without duplicate directives.

Canonical and projected files carry generator provenance in their frontmatter and an HTML comment naming their source. `check` renders the expected harness in memory, compares it against the working copy, and writes nothing.

`.repo-harness/check.py` ships inside the generated harness because the builder does not. It embeds the SHA-256 of the exact generated manifest bytes, authenticates those bytes before JSON parsing, validates the strict checker schema and every path, then compares each canonical skill against its `.claude/` and `.opencode/` projections. It ignores only the `canonical-source:` line a projection is allowed to rewrite and exits non-zero on invalid input, a stale copy, or a missing copy. Pi carries prompts rather than skill copies, so it has nothing to compare.

## Integration Modes

Integrated mode is the default and requires Gentle AI plus Engram. The builder workflow validates the Gentle AI CLI through an available status, doctor, or version capability and validates Engram through MCP or CLI before approval or apply. Generated Python does not invent runtime dependency detection because Engram may be exposed only through MCP.

`--standalone` is the only mode flag. It preserves the same paths and runtime layouts while rendering repository-only session and rule instructions with no Gentle AI or Engram requirement.

## One Operation, Not Three Modes

There is no `bootstrap`, `adopt`, or `upgrade`. An empty repository and a mature one take the same path, because the rule is the same in both: create what is absent, leave what is present. The three modes existed only to describe when overwriting was allowed, and overwriting no longer happens.

`render` prints `create`, `append`, `existing`, and `identical` sets, a deterministic plan digest, and the exact case-sensitive approval prompt whose complete answer domain is `APROBAR` and `DENEGAR`. `render --apply --approved-plan-sha256 HASH` executes only that approved current plan. `check` reports congruence afterward.

Managed regions are not advertised. The core manages complete files only, and only at creation time. This narrower contract is deliberate and honest.
