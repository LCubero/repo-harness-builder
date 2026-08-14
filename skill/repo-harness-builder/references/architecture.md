# Harness Architecture

## Canonical Core

Root `REPO-HARNESS.md` is the stable concise visible entry point. `.repo-harness/manifest.json` records the project, enabled runtimes, canonical skill names, write semantics, and exactly one `integrationMode`: `integrated` by default or `standalone` for `--standalone`. Canonical runtime-neutral skills live below `.repo-harness/skills/`. There is no ownership ledger: the renderer is deterministic, so the expected content of any managed file is recomputed on demand instead of stored.

The rulebook answers how work is done here and never where the project stands, so it carries a token budget and a gate table instead of the detail itself. `.repo-harness/rules/` is where that detail goes down to: seeded with `memory.md` and `working-agreements.md`, each loaded only when its gate fires, each under the same budget. Without this directory the budget rule would have no exit and would break on the first project with real rules.

Root `README.md` is the product-facing guide and part of a complete empty-repository baseline. In an existing repository, its content is analyzed and improved through an informed approved proposal. Bounded generated markers may aid maintenance but never make a pre-existing README exempt from analysis. Durable harness detail lives in `docs/repo-harness/`: a concise human usage guide, documentation ownership, verified current state, recent completed work, pending work, next step, decisions, and constraints. Root `CHANGELOG.md` records concise dated completed changes, not plans. Session output and memory remain temporary until verified and promoted to a canonical owner.

## Generated Skills

- `session-start` maps explicitly to `repo-harness-session-start`: read-only repository and documentation recovery, discrepancies, remaining work, and next step. Integrated mode requires Engram recovery and reconciliation; standalone mode uses repository files only.
- `session-stop` maps explicitly to `repo-harness-session-stop`: evidence-based handoff, path validation, approval-gated documentation and meaningful changelog updates, and separate optional external gates. Integrated mode requires significant observations and a session summary saved to Engram with visible failure semantics; standalone mode records continuity only in repository files.
- `documentation` maps explicitly to `repo-harness-documentation`: ownership, maps, links, freshness, duplication control, factual changelog maintenance, and durable versus temporary state.

Generated skill names are fixed across repositories. Repository identity remains in the manifest and generated content, but command names are never derived by removing a project prefix.

Session skills use conversational questions for ordinary continuation and documentation choices. They never inherit the builder's strict language-scoped digest protocol, while external mutations still require explicit approval.

## Runtime Projection

OpenCode and Claude-style profiles receive generated skill projections and thin commands that invoke registered skill names. Pi receives thin prompts through `.pi/prompts/`. Each enabled profile also receives `repo-harness-help`, a thin read-only wrapper routed by the resolved report language to `docs/repo-harness/usage.md` or `docs/es/repo-harness/usage.md`. Runtime selection can therefore generate language-specific help. The language-dependent wrapper bytes are normalized only for plan-digest calculation, so approval remains bound to one language-independent action plan. If the guide is missing, help reports an incomplete or outdated harness and recommends read-only builder inspection. Disabled profiles produce no output. No wrapper contains an installation path.

Claude Code also requires root `CLAUDE.md` with the exact line `@REPO-HARNESS.md`. OpenCode and Pi require root `AGENTS.md` with the exact line `Read and follow REPO-HARNESS.md before working in this repository.` Runtime combinations use the union of those two files without duplicate directives.

Canonical and projected files carry generator provenance in their frontmatter and an HTML comment naming their source. `check` renders the expected harness in memory, compares it against the working copy, and writes nothing.

`.repo-harness/check.py` ships inside the generated harness because the builder does not. It embeds the SHA-256 of the exact generated manifest bytes, authenticates those bytes before JSON parsing, validates the strict checker schema and every path, then compares each canonical skill against its `.claude/` and `.opencode/` projections. It ignores only the `canonical-source:` line a projection is allowed to rewrite and exits non-zero on invalid input, a stale copy, or a missing copy. Pi carries prompts rather than skill copies, so it has nothing to compare.

## Integration Modes

Integrated mode is the default and requires Gentle AI plus Engram. The builder workflow validates the Gentle AI CLI through an available status, doctor, or version capability and validates Engram through MCP or CLI before approval or apply. Generated Python does not invent runtime dependency detection because Engram may be exposed only through MCP.

`--standalone` is the only mode flag. It preserves the same paths and runtime layouts while rendering repository-only session and rule instructions with no Gentle AI or Engram requirement.

## Repository-Sensitive Operation

There is one builder operation with repository-sensitive planning. A new or effectively empty repository receives a complete useful baseline. A mature repository receives content analysis and an informed integration proposal. If existing material is obsolete, structurally incompatible, or the user prefers a reset, the builder offers archive and start-clean as a distinct conversational choice. No overwrite or archive occurs without explicit approval.

`render` prints `create`, `append`, `existing`, `identical`, and `blocked` sets, a deterministic plan digest, and the exact case-sensitive approval domain selected by `--report-language`. English uses `APPROVE` or `DENY`; Spanish uses `APROBAR` or `DENEGAR`. The deterministic direct-CLI fallback is English. `render --report-language LANGUAGE --apply --approved-plan-sha256 HASH --approval TOKEN` executes only when both the digest and exact active-domain approval validate and `blocked` is empty. Language changes localized help bytes but is normalized out of the digest. `check` reports congruence and locale-baseline failures afterward.

Generated markers can bound generated material, but ownership metadata never replaces analysis of existing content. An upgrade audit compares the installed manifest, generator versions, managed bytes, and runtime projections against the current render. It classifies exact outputs as `current`, current-generation drift as `stale`, absent outputs as `missing`, structurally valid human-owned changes as `human-evolved`, and proven older generated outputs as `legacy`. The approved plan is the authority for each mutation.

Upgrade analysis is proposal-only except for the fixed-name transition. When the installed manifest declares the exact three legacy project-prefixed skills and every exact manifest-owned canonical and runtime byte matches its deterministic legacy render, normal render preview includes one digest-bound `migrate-generated-skills` action. Approved apply creates fixed destinations, replaces the manifest and checker, and removes only the exact legacy sources in one rollback-safe transaction. Mixed or unverifiable state is blocked, and unrelated similarly prefixed skills provide no ownership evidence.
