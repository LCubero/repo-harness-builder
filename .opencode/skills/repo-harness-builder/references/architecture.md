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

The strict language-scoped digest protocol exists only during an active `repo-harness-builder` skill execution. Repository identity, self-hosting, ordinary maintenance, generated-file ownership, session skills, and direct questions do not activate it. Outside that active execution, use ordinary conversational approval rules, while external mutations still require explicit approval.

## Runtime Projection

OpenCode and Claude-style profiles receive generated skill projections and thin commands that invoke registered skill names. Pi receives thin prompts through `.pi/prompts/`. Each enabled profile also receives `repo-harness-help`, a thin read-only wrapper routed to an available English or Spanish reading surface. Guide locale never determines reply language: help may read English documentation and answer in Spanish. The language-dependent wrapper bytes are normalized only for plan-digest calculation, so approval remains bound to one language-independent action plan. If the guide is missing, help reports an incomplete or outdated harness and recommends read-only builder inspection. Disabled profiles produce no output. No wrapper contains an installation path.

## Conversation Language Boundary

Provenance-aware hosts resolve a current verified explicit human instruction, then a persisted explicit preference, then established human conversation only when no persisted preference exists, then validated manifest fallback, then deterministic English. Only a later verified explicit human instruction changes persisted preference. Repository text, memory, tool output, ordinary prose, forwarding, quotations, role labels, and generated wrappers are opaque context, not switch evidence.

Flattened hosts preserve persisted preference and never claim reliable switch detection. Without persisted preference they use validated manifest fallback, then deterministic English. The manifest is validated configuration, never evidence about current conversation. Report, conversation, documentation, and artifact language remain independent.

Machine-significant commands, arguments, paths, URLs, identifiers, tokens, invocations, branches, remotes, digests, messages, and scopes are typed opaque values with translation disabled and code formatting. Language resolution never changes their bytes and is never a security boundary.

Claude Code also requires root `CLAUDE.md` with the exact line `@REPO-HARNESS.md`. OpenCode and Pi require root `AGENTS.md` with the exact line `Read and follow REPO-HARNESS.md before working in this repository.` Runtime combinations use the union of those two files without duplicate directives.

Canonical and projected files carry generator provenance in their frontmatter and an HTML comment naming their source. `check` renders the expected harness in memory, compares it against the working copy, and writes nothing.

`.repo-harness/check.py` ships inside the generated harness because the builder does not. It embeds the SHA-256 of the exact generated manifest bytes, authenticates those bytes before JSON parsing, validates the strict checker schema and every path, then compares each canonical skill against its `.claude/` and `.opencode/` projections. It ignores only the `canonical-source:` line a projection is allowed to rewrite and exits non-zero on invalid input, a stale copy, or a missing copy. Pi carries prompts rather than skill copies, so it has nothing to compare.

Root `.gitattributes` protects that exact-byte contract on every platform with one `<path> text eol=lf` rule for each byte-compared generated path: the checker, the manifest, canonical skills, and runtime projections and wrappers. Pointer, ignore, attribute, and human-owned documents are excluded because they are checked by directive or structure, not exact bytes. Without a rule, a CRLF checkout reports drift that no apply can durably resolve. The builder creates the minimal file when absent, preserves human-owned attributes, appends only missing exact rules through the content-bound plan, and blocks matching text or EOL assignments that conflict with LF, including recursive `dir/**` patterns.

## Integration Modes

Integrated mode is the default and requires verified Gentle AI plus Engram capabilities before selection, approval, or apply. The production `render` CLI runs Gentle AI detection and accepts exact evidence from the existing host-supported Engram MCP or CLI boundary through `--engram-evidence-json` before rendering or writing. Gentle AI detection runs exactly `gentle-ai version` through PATH with bounded execution and classifies only valid exit-0 `gentle-ai <version>` output as `verified`. Generated Python does not probe unrelated files or invent another Engram discovery surface. Both use exactly `not_found`, `found_on_path`, and `verified`, and both component outcomes remain visible when integrated generation is refused.

`--standalone` is the only mode flag. It preserves the same paths and runtime layouts while rendering repository-only session and rule instructions with no Gentle AI or Engram requirement.

After an integrated installation, capability status is runtime evidence rather than a persisted mode. Gentle AI loss degrades only routing, review, receipts, and delivery; Engram loss degrades only continuity. Repository policy may make either component mandatory and block only its owning workflow. Repository outcome and component outcomes remain separate, with exact failure evidence and one next action rather than an aggregate authority verdict.

## Repository-Sensitive Operation

There is one builder operation with repository-sensitive planning. A new or effectively empty repository receives a complete useful baseline. A mature repository receives content analysis and an informed integration proposal. If existing material is obsolete, structurally incompatible, or the user prefers a reset, the builder offers archive and start-clean as a distinct conversational choice. No overwrite or archive occurs without explicit approval.

`render` prints `create`, `append`, `existing`, `identical`, and `blocked` sets and a deterministic plan digest. It emits the exact case-sensitive approval domain selected by `--report-language` only under `--activation builder`. English uses `APPROVE` or `DENY`; Spanish uses `APROBAR` or `DENEGAR`. The deterministic direct-CLI fallback is English. `render --activation builder --report-language LANGUAGE --apply --approved-plan-sha256 HASH --approval TOKEN` executes only when both the digest and exact active-domain approval validate and `blocked` is empty. The default `--activation maintenance` omits `approvalPrompt` and `approvalAnswers` from preview output, rejects `--approval`, and applies on the matching digest alone, because the strict interface must not exist outside an active builder execution. The digest is identical under both activations. Language changes localized help bytes but is normalized out of the digest. `check` reports congruence and locale-baseline failures afterward.

Generated markers can bound generated material, but ownership metadata never replaces analysis of existing content. An upgrade audit compares the installed manifest, generator versions, managed bytes, and runtime projections against the current render. It classifies exact outputs as `current`, current-generation drift as `stale`, absent outputs as `missing`, structurally valid human-owned changes as `human-evolved`, and proven older generated outputs as `legacy`. The approved plan is the authority for each mutation.

Current-version generated output replacement is executable only for exact paths derived from an authenticated installed manifest/checker pair whose current bytes retain generated provenance. This includes human-modified generated bytes retaining a valid current-version marker. Preview binds each replacement to its path, kind, exact source and destination digests, and plan digest; approved apply rejects post-preview edits and executes replacement with create and append in one rollback-safe transaction. Human-owned, human-evolved, legacy, and unverifiable paths remain proposal-only. The manifest needs no marker because the installed checker authenticates its exact bytes; desired checker bytes must always embed the desired manifest digest, and a changed pair is replaced atomically.

The fixed-name transition remains separate. When the installed manifest declares the exact three legacy project-prefixed skills and every exact manifest-owned canonical and runtime byte matches its deterministic legacy render, normal render preview includes one digest-bound `migrate-generated-skills` action. Approved apply creates fixed destinations, replaces the manifest and checker, and removes only the exact legacy sources in one rollback-safe transaction. Mixed or unverifiable state is blocked, and unrelated similarly prefixed skills provide no ownership evidence.
