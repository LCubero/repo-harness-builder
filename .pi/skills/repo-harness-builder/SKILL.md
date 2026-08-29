---
name: repo-harness-builder
description: "Build or check a minimal documentation and continuity harness in the selected repository with bounded root runtime pointers."
license: MIT
metadata:
  author: Luis Diego Cubero Chavarria
  version: "3.1"
---

## Activation Contract

Invoke deliberately through `/repo-harness-builder`. This skill never activates on its own.

The exact approval UI and strict builder rules exist only during an explicitly active `repo-harness-builder` skill execution. Repository identity, self-hosting, ordinary repository maintenance, generated-file ownership, session skills, and direct questions do not activate this interface. Outside an active builder skill execution, use ordinary conversational approval rules.

## Product Identity

`repo-harness` is a repository-scoped system of prompts, guardrails, harness rules, durable documentation, skills, and commands that strengthens and assists AI coding agents such as OpenCode, Claude Code, and Pi. `repo-harness-builder` creates, audits, installs, and safely evolves that system inside a selected repository.

The harness is independently useful in standalone mode and may use optional ecosystem components according to the selected integration mode. Gentle AI provides broader routing, review, receipts, and delivery capabilities. Engram provides semantic continuity but never replaces repository truth. CodeGraph is the most optional repository-local code intelligence and navigation aid and is never an authority store. Hermes Agent, OpenClaw, and Pi are inspiration sources only, not dependencies or products this tool recreates.

Build or inspect root `REPO-HARNESS.md`, durable documentation, operational skills, and selected-runtime wrappers. Default integrated mode composes Gentle AI and Engram independently when each capability is available. Use `--standalone` for explicit repository-only continuity. With no operation, check read-only and report gaps.

An optional argument sets the report language to `español` or `english`; otherwise follow runtime preference, then the language the user is writing in. Resolve report, conversation, documentation, and artifact language independently. A persisted explicit conversation preference changes only through a later verified explicit human instruction.

## Hard Rules

- In a new or effectively empty repository, create the complete useful baseline after normal plan approval, including `README.md`.
- In an existing repository, analyze relevant content and propose informed integration or updates that reuse and improve it. Preservation never means freezing existing files.
- When existing content is obsolete, structurally incompatible, or the user prefers a reset, offer a conversational choice to archive it and start from a clean generated baseline.
- Never overwrite or archive existing content without explicit approval.
- Version control is the safety net. Do not present this tool as a boundary against a hostile filesystem.
- Resolve and inspect only the selected repository. Never traverse links, siblings, aliases, or external references.
- Show four action sets. Approve only the current digest; never reuse it.
- Only an explicitly active builder execution's content-bound apply plan uses strict answers, declared to the CLI with `--activation builder`. English accepts exactly `APPROVE` or `DENY`; Spanish accepts exactly `APROBAR` or `DENEGAR`.
- The CLI defaults to `--activation maintenance`: it withholds the strict domain from preview output entirely and applies on the current digest alone, so ordinary maintenance cannot relay an approval interface it must not present.
- Keep wrappers thin and path-free: they load a generated skill by registered name, and Pi-like runtimes use their prompt mechanism.
- Generate `repo-harness-session-start`, `repo-harness-session-stop`, and `repo-harness-documentation` in every repository. Keep `session-start`, `session-stop`, and `documentation` commands unchanged through explicit mapping, never prefix removal.
- Keep repository evidence authoritative. The repository owns durable truth, source, tests, policy, backlog, and approvals. Engram owns semantic continuity. Gentle AI owns routing, exact-candidate review and receipts, and delivery gates when enabled. Never copy an owner's state into the harness.
- In integrated mode, compose Gentle AI and Engram independently from separate `not_found`, `found_on_path`, and `verified` detector outcomes. Fail closed only for a capability required by the selected operation or repository policy. Standalone mode uses neither capability. Persist only `integrated` or `standalone`, never detection or effective states.
- Never perform Git initialization, provider, backup, publication, or specialized methodology behavior automatically.
- Treat Git initialization and provider publication as separate approval boundaries. Publication visibility is another explicit choice.
- Ask one blocking question per turn. Localize user reports and human documentation; keep generated technical contracts in English.
- Declare the human documentation locales as English-only, Spanish-only, or bilingual. English-only and Spanish-only use canonical paths. Bilingual output keeps English canonical owners plus complete Spanish peers. Guide locale selects an available reading surface only and never determines reply language. Never name a document the render does not create.
- Keep skills, contracts, wrappers, checker machinery, manifest instructions and schema, code, comments, runtime instructions, and all technical artifacts in English. Spanish-only applies only to human repository documentation such as `README.md`, `CHANGELOG.md`, and durable files under `docs/repo-harness/`.
- Treat reciprocal README selectors as one atomic integration. When an existing `README.md` lacks the reciprocal link, append only the exact selector block through the content-bound plan and create the peer in the same apply. If that append is not possible, classify `README.es.md` as blocked. Never create a one-way peer or claim bilingual completeness.
- Validate localized human-owned documents with semantic roles plus proportional locale baselines for required content and known cross-language leakage. These checks detect incompleteness; they do not claim automated proof of translation equivalence.
- For an existing repository that declared more than one documentation locale, analyze every expected localized path and propose integration. Never overwrite human-evolved documentation merely to install or normalize localization.
- Use Python 3.10+ and the standard library only.
- Generate one exact `<path> text eol=lf` rule in root `.gitattributes` for every byte-compared generated path; preserve unrelated attributes, append only missing rules, and block conflicting matches.

## Decision Gates

| Evidence | Action |
|---|---|
| Managed path is absent | Create it |
| Existing repository content is relevant | Analyze it and propose an integrated improvement |
| Existing content is obsolete or incompatible | Offer archive and clean-baseline generation as a distinct conversational choice |
| Required root pointer exists without its exact directive | Append only that newline-delimited directive |
| Required root pointer already contains its exact directive | Leave every byte unchanged |
| Generated file matches rendered bytes | Report `current` |
| Exact manifest-owned generated file with builder provenance differs from rendered bytes | Emit a digest-bound replacement action; apply only after explicit approval |
| Proven older generated file differs from rendered bytes | Report `legacy`; propose replacement only after explicit approval |
| Human-owned file differs while retaining its contract | Report `human-evolved`; propose integration or an explicit archive/start-clean choice |
| Human-owned file lost a promised section | Report the missing sections; never rewrite it |
| Repository declares a license | Pass its SPDX identifier to `--license` |
| No license found | Generate `UNLICENSED` and say so in the report |
| No license choice has been made | Read `references/license-selection.md` and ask one conversational choice before planning |
| Root `README.md` is absent | Create the product guide with a bounded generated section |
| Root `README.md` exists | Analyze it and propose an informed update; markers do not exempt it from improvement |
| An integrated capability is unavailable | Degrade only its owned behavior; refuse selection, approval, or apply only when that capability is required by the selected operation or repository policy |
| `--standalone` selected | Generate repository-only continuity instructions |
| Runtime profile disabled or unknown | Do not generate its wrapper |
| No runtime supplied | Report every runtime surface found and ask; never infer one |
| Root `.gitattributes` lacks either exact LF rule | Append only missing rules while preserving every existing byte and line ending |
| An existing attribute rule conflicts with LF for either protected path | Block apply; never append an ambiguous policy |
| Installed manifest declares exactly `{project_id}-session-start`, `{project_id}-session-stop`, and `{project_id}-documentation`, and every manifest-owned generated byte is exact | Include one digest-bound `migrate-generated-skills` action in normal render preview; approved apply creates the fixed destinations and removes only those exact sources |

## Stateless Correction Authority

Preview and apply are separate CLI processes with no shared memory, so the only thing that can
carry an approval between them is bytes the host hands back. That transport is the canonical
authority context, and it is opt-in: builder activation on its own still means only the strict
approval domain.

- **Host.** Writes a canonical authority context: exact compact ASCII JSON, sorted keys, no
   spaces after separators, and no trailing newline. It declares the material revision, the
   project, the exact builder configuration this run will use, classified premises, separate
   human decisions, and typed evidence bindings.
- **Preview.** Reads it with `--authority-context-file` and `--project-id`, resolves and re-reads
   every declared evidence item, and emits `authorityContextSha256`, `targetBindingSha256`,
   `planSha256`, and `generationSha256` alongside stable identifiers and explicit redaction
   markers. It emits no premise value, no decision statement, no raw evidence, and no absolute
   path.
- **Human.** Reviews those bindings and approves the plan.
- **Apply.** Receives the same context plus `--approved-plan-sha256` and
   `--approved-generation-sha256`. It re-reads the context, re-validates every binding against
   current bytes, recomputes the generation, and refuses before any write when anything differs.

If a premise was wrong, the host must refresh the context with a new material revision and
preview again. A corrected identical plan keeps the same `planSha256` and gets a different
`generationSha256`, which is exactly why the plan digest alone cannot authorize the write.

Every authority failure at apply collapses into one content-free `fresh preview` refusal, because
naming the difference would echo the evidence the binding exists to keep out of the output. An
initial preview still reports exactly what is wrong with the context it was handed.

Under the default `--activation maintenance` all three authority arguments are rejected and
maintenance applies on the matching plan digest alone.

## Self-Update And Runtime Cache

- Distinguish on-disk canonical and projection parity, active-process registered skill freshness, and runtime reload or restart capability.
- If a skill changed during the active process but loading it by registered name returns older content, classify that as an expected active-process cache limitation. Do not mark repository state, documentation close, continuity, or verified on-disk projections blocked or failed.
- Verify disk parity now and defer registered-name validation. A fresh runtime process will load the updated projection; a supported explicit registry reload may be used if the runtime provides one. Never claim or invent a reload command, and require immediate restart only if the user wants live validation now.

## Execution Steps

1. Read `references/audit-and-interview.md`, resolve the report language, and check the repository without writes. Then ask for the conversation language and stop. After the human answers, immediately ask for the documentation languages in the next turn and stop again. Do not wait for filler input. Keep all three values distinct even when the user chooses the same language for more than one purpose.
2. Report what is absent across every runtime profile inspected, then ask one question: which profiles to include. That answer is the permission to plan. Stop for it.
3. Read `references/architecture.md`. For integrated mode, validate the Gentle AI CLI by running exactly `gentle-ai version` through PATH with bounded execution and no shell, then classify it as `not_found`, `found_on_path`, or `verified`. Validate the Engram MCP or CLI through the host's supported capability evidence using the same three classifications. Derive each effective state independently for the selected operation, preserve both component outcomes, and refuse only when an unavailable capability is required by that operation or repository policy. Skip capability detection for `--standalone`.
4. Read `references/license-selection.md` and resolve the license choice. Read `references/preservation-and-safety.md`. If a prior harness exists, audit its manifest, generator markers, managed files, and runtime projections, then classify paths as `current`, `stale`, `missing`, `human-evolved`, or `legacy`. Exact manifest-owned generated outputs may receive digest-bound replacement actions even when a human modified their bytes, as long as they retain valid builder provenance whatever version it names; human-owned, human-evolved, and unverifiable paths remain proposal-only. A verifiable project-prefixed legacy harness receives one bounded migration action; mixed, modified, or unverifiable legacy installations are blocked. If the repository is existing, analyze relevant content and conversationally resolve integrate/update versus archive/start clean before forming the apply plan.
5. Show the content-bound apply plan and `planSha256`, then follow the resolved language domain in `references/audit-and-interview.md`.
6. Run `scripts/harness_tool.py render --root ROOT --project-name NAME --runtime RUNTIME --license LICENSE --report-language REPORT_LANGUAGE [--documentation-language en | --documentation-language es | --documentation-language en --documentation-language es] --conversation-language CONVERSATION_LANGUAGE --engram-evidence-json HOST_EVIDENCE --activation builder --apply --approved-plan-sha256 HASH --approval TOKEN` with the independently resolved values, exact host-supported Engram MCP or CLI evidence, exact approval, approved digest, and resolved `--license`, then `check`. Repeat `--runtime` once per profile approved in step 2; omitting it renders the core only and discards that permission. Omit `--documentation-language` for English-only. Use `es` alone for Spanish-only human documentation and repeat `en` then `es` for bilingual documentation. Confirm changed bytes, resolved links, and zero writes from the check.
7. Validate with `references/validation.md` and report limitations, including deferred registered-name validation when the active process retained older registered content.
8. After harness verification, optionally ask whether to initialize Git. This is a separate approval and must never happen without explicit approval.
9. Separately offer provider publication, suggesting GitHub without making it mandatory. Ask whether the repository should be private or public, then obtain explicit publication approval. Never authenticate or publish without it.

## Output Contract

Return status, all action sets, generated tree, verification outcomes, exclusions, residual risks, and readiness.

## References

- `references/audit-and-interview.md` - repository audit and localized decisions.
- `references/architecture.md` - generated core and runtime projection.
- `references/preservation-and-safety.md` - write semantics and containment limits.
- `references/license-selection.md` - license choices, tradeoffs, and authoritative sources.
- `references/validation.md` - behavior-level acceptance checks.
- `references/gentle-ai-style-guide.md` - local skill style summary.
