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

Build or inspect root `REPO-HARNESS.md`, durable documentation, operational skills, and selected-runtime wrappers. Default integrated mode requires Gentle AI and Engram. Use `--standalone` for repository-only continuity. With no operation, check read-only and report gaps.

An optional argument sets `español` or `english`; otherwise follow runtime preference, then conversation language.

## Hard Rules

- In a new or effectively empty repository, create the complete useful baseline after normal plan approval, including `README.md`.
- In an existing repository, analyze relevant content and propose informed integration or updates that reuse and improve it. Preservation never means freezing existing files.
- When existing content is obsolete, structurally incompatible, or the user prefers a reset, offer a conversational choice to archive it and start from a clean generated baseline.
- Never overwrite or archive existing content without explicit approval.
- Version control is the safety net. Do not present this tool as a boundary against a hostile filesystem.
- Resolve and inspect only the selected repository. Never traverse links, siblings, aliases, or external references.
- Show four action sets. Approve only the current digest; never reuse it.
- Only the builder's content-bound apply plan uses strict answers. English accepts exactly `APPROVE` or `DENY`; Spanish accepts exactly `APROBAR` or `DENEGAR`.
- Keep wrappers thin and path-free: they load a generated skill by registered name, and Pi-like runtimes use their prompt mechanism.
- Keep repository evidence authoritative. Integrated mode requires Gentle AI and Engram; standalone mode must contain neither requirement.
- Never perform Git initialization, provider, backup, publication, or specialized methodology behavior automatically.
- Treat Git initialization and provider publication as separate approval boundaries. Publication visibility is another explicit choice.
- Ask one blocking decision per turn. Localize user reports and bilingual documentation peers; keep generated technical contracts in English.
- Generate complete English and Spanish reader documentation. Route English interaction to `README.md` and `docs/repo-harness/usage.md`; route Spanish interaction to `README.es.md` and `docs/es/repo-harness/usage.md`.
- Keep operational truth single-owned in `REPO-HARNESS.md`, `CHANGELOG.md`, and English durable state owners. Spanish peers are complete localized presentations, not independent factual owners.
- Treat reciprocal README selectors as one atomic integration. If an existing `README.md` cannot be updated by the exact executable plan, classify `README.es.md` as blocked and do not create a one-way peer or claim bilingual completeness.
- Validate localized human-owned documents with semantic roles plus proportional locale baselines for required content and known cross-language leakage. These checks detect incompleteness; they do not claim automated proof of translation equivalence.
- For existing repositories, analyze every expected bilingual path and propose integration. Never overwrite human-evolved documentation merely to install or normalize localization.
- Use Python 3.10+ and the standard library only.

## Decision Gates

| Evidence | Action |
|---|---|
| Managed path is absent | Create it |
| Existing repository content is relevant | Analyze it and propose an integrated improvement |
| Existing content is obsolete or incompatible | Offer archive and clean-baseline generation as a distinct conversational choice |
| Required root pointer exists without its exact directive | Append only that newline-delimited directive |
| Required root pointer already contains its exact directive | Leave every byte unchanged |
| Generated file matches rendered bytes | Report `current` |
| Current-version generated file differs from rendered bytes | Report `stale`; propose replacement only after explicit approval |
| Proven older generated file differs from rendered bytes | Report `legacy`; propose replacement only after explicit approval |
| Human-owned file differs while retaining its contract | Report `human-evolved`; propose integration or an explicit archive/start-clean choice |
| Human-owned file lost a promised section | Report the missing sections; never rewrite it |
| Repository declares a license | Pass its SPDX identifier to `--license` |
| No license found | Generate `UNLICENSED` and say so in the report |
| No license choice has been made | Read `references/license-selection.md` and ask one conversational choice before planning |
| Root `README.md` is absent | Create the product guide with a bounded generated section |
| Root `README.md` exists | Analyze it and propose an informed update; markers do not exempt it from improvement |
| Integrated Gentle AI or Engram preflight fails | Refuse approval and apply; surface the failure |
| `--standalone` selected | Generate repository-only continuity instructions |
| Runtime profile disabled or unknown | Do not generate its wrapper |
| No runtime supplied | Report every runtime surface found and ask; never infer one |

## Self-Update And Runtime Cache

- Distinguish on-disk canonical and projection parity, active-process registered skill freshness, and runtime reload or restart capability.
- If a skill changed during the active process but loading it by registered name returns older content, classify that as an expected active-process cache limitation. Do not mark repository state, documentation close, continuity, or verified on-disk projections blocked or failed.
- Verify disk parity now and defer registered-name validation. A fresh runtime process will load the updated projection; a supported explicit registry reload may be used if the runtime provides one. Never claim or invent a reload command, and require immediate restart only if the user wants live validation now.

## Execution Steps

1. Read `references/audit-and-interview.md`, resolve the report language, and check the repository without writes.
2. Report what is absent across every runtime profile inspected, then ask one question: which profiles to include. That answer is the permission to plan. Stop for it.
3. Read `references/architecture.md`. For integrated mode, validate the Gentle AI CLI with its available status, doctor, or version capability and validate the Engram MCP or CLI before approving or applying. Refuse on failure. Skip this dependency preflight only for `--standalone`.
4. Read `references/license-selection.md` and resolve the license choice. Read `references/preservation-and-safety.md`. If a prior harness exists, audit its manifest, generator markers, managed files, and runtime projections, then classify paths as `current`, `stale`, `missing`, `human-evolved`, or `legacy`. If the repository is existing, analyze relevant content and conversationally resolve integrate/update versus archive/start clean before forming the apply plan.
5. Show the content-bound apply plan and `planSha256`, then follow the resolved language domain in `references/audit-and-interview.md`.
6. Run `scripts/harness_tool.py render --report-language LANGUAGE --apply --approved-plan-sha256 HASH --approval TOKEN` with the same resolved language, exact approval, approved digest, and resolved `--license`, then `check`. The selected language also generates runtime help routed to that locale, while the action-plan digest remains language-independent. Confirm changed bytes, resolved links, and zero writes from the check.
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
