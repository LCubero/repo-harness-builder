# Audit And Decisions

## Read-Only Audit

Resolve one explicitly selected repository root. Inspect only descendants needed to classify intended harness paths, relevant ancestors, existing project documentation, runtime surfaces, and ownership evidence. Reject links, aliases, sibling repositories, and external references. Do not read secret values or mutate the target.

Classify the repository as new or effectively empty, or existing, using current content rather than Git presence alone. For an installed prior harness, audit the manifest, generated markers, managed files, and runtime projections. Classify each intended managed path as `current`, `stale`, `missing`, `human-evolved`, or `legacy`; an exact root pointer directive is `present`. In an existing repository, analyze relevant content and summarize what should be reused, improved, added, or superseded. Report uncertainty and the exact missing evidence instead of guessing.

Resolve integration before approval. Default to integrated mode and validate the Gentle AI CLI through the host's available status, doctor, or version capability, then validate Engram through its available MCP or CLI. If either validation fails, surface the failure and do not approve or apply an integrated plan. `--standalone` is the sole exception and generates repository-only continuity without either dependency.

While auditing, resolve the license the generated skills will carry: read a root `LICENSE` or `LICENSE.md`, then a `license` field in `package.json`, `pyproject.toml`, or `Cargo.toml`. Use the first non-empty, single-line SPDX expression found. Reject values containing characters outside letters, digits, spaces, `.`, `+`, `-`, `:`, and parentheses. Pass the exact accepted value to the renderer, which encodes it as a quoted YAML string rather than interpolating it as a plain scalar. When nothing declares one, read `license-selection.md` and ask the user to choose; do not choose on the project's behalf. If the user declines to choose, generate `UNLICENSED` and say so.

## Decisions

Ask one blocking decision per turn and stop for the answer. Keep questions clear and localized without forcing ceremonial labels on trivial exchanges. Explain the immediate consequence when it is not obvious.

For the builder's content-bound apply plan only, use the resolved report language consistently for the headline, case-sensitive instruction, allowed answers, question, and invalid-answer message. English accepts exactly uppercase `APPROVE` or `DENY`; Spanish accepts exactly uppercase `APROBAR` or `DENEGAR`. Continue only for the active language's approval token and stop for its denial token. Reject lowercase, mixed case, aliases, prose, and cross-language tokens with the active localized error. Do not normalize or retry. Runtime, license, integrate/update versus archive/start-clean, and generated session choices are conversational and non-strict.

The first decision after audit permits planning only. A new or effectively empty repository proceeds to a complete baseline plan. An existing repository first receives an evidence-based proposal and, when needed, one conversational choice between integrate/update and archive/start clean. The later write decision must reference the exact current plan and its newly printed digest; never reuse a digest from an earlier preview.

Upgrade diagnostics and integration choices are conversational. The strict language-scoped tokens remain exclusive to the final content-bound apply plan. A proposal may name replacement for proven generated `stale` or `legacy` files, but human-evolved content requires integration or archive/start-clean choices. Never schedule replacement, archive, or deletion merely from classification.

## Runtime Selection

Never infer the runtime set. A runtime surface already present in the repository is evidence, not consent. A runtime the user did not choose is never described as selected, active, or current.

Resolve it once, in this order, and state which source won:

1. An explicit runtime argument on the command.
2. The exact set already recorded in an existing `.repo-harness/manifest.json`.
3. Nothing supplied. The set stays an open decision.

While it is an open decision, report every supported profile the audit inspected, `claude-code`, `opencode`, and `pi`, with the absent paths of each, and name the set as the open decision it is. Reporting a surface found on disk is evidence for the user's choice, never a substitute for it. One `check` call carries every profile, so inspect them together instead of invoking it once per profile.

An installed `.repo-harness/manifest.json` already records a runtime set. When it exists, that set is the default and the audit reports it as the source that won. Read `runtimeSet` in the tool output: `recorded` is what the repository committed to, `requested` is what this run asked for.

When they differ, say so before planning, and say which direction. Only `runtimeSet.unrecorded` needs recovery: those runtimes get new projections the manifest cannot learn about, because it is never rewritten, so the generated checker stays blind to them and keeps reporting `current` while inspecting part of the harness. The recovery is to delete `.repo-harness/manifest.json` and `.repo-harness/check.py`, then run again with the full set, which the builder recreates because they are then absent. The tool names both files in `runtimeSet.staleManifest`. Never present that case as complete without this step.

Dropping a recorded runtime is the harmless direction. Nothing new is created, the manifest still records the dropped profile, and the generated checker still covers it. Report the difference and say plainly that no recovery applies.

Root pointers are runtime-conditional and belong under their own profile, never in the common set. `CLAUDE.md` ships only with `claude-code`; `AGENTS.md` ships only with `opencode` or `pi`, and one file serves both. Only `REPO-HARNESS.md` and `CHANGELOG.md` are common to every selection. Listing a pointer as common promises a file that a selection excluding its profile will never create.

Resolve the set in the single blocking decision that follows the audit. That decision asks exactly one thing: which profiles to include. Naming a set is itself the permission to plan for it, because planning writes nothing and the write gate is the later approval. Never ask a second question to authorize planning, and never bundle the runtime question with any other ask. Planning cannot begin while the set is unresolved.

## Report Language

Resolve it once, in this order, and state which source won:

1. An explicit first argument on the command: `español`, `espanol`, `es`, `english`, or `en`. Nothing else counts as a language argument; treat any other first argument as a repository or operation.
2. A runtime language preference supplied directly by the host.
3. The current conversation language.

A supplied argument wins even as a bare token with no sentence around it. `espanol` alone is the canonical form of step 1, not a weaker one. The wrapper prose, this contract, and every tool output are written in English by design, so none of them is evidence of the conversation language. When an argument matches a language token, resolution stops at step 1 and step 3 is never consulted.

Then translate every user-facing word, not only the titles. Both report templates in `assets/` mark translatable text as `{{t "..."}}`; every one of those markers must come out in the resolved language. A field label left in English is the defect this contract exists to prevent.

Pass the resolved value explicitly to the renderer as `--report-language english` or `--report-language espanol` for both preview and apply. The CLI does not use global language state. When a pure renderer or direct CLI caller supplies no language, the deterministic product fallback is `english`; an orchestrating runtime still resolves the existing argument, host preference, and conversation contract before invoking it. Apply fails closed unless both the current `planSha256` and the exact active-domain `--approval` are supplied. Report language changes only approval presentation and validation; it never enters the action plan or its digest.

Preserve commands, paths, identifiers, enum values, states, and anything inside backticks exactly as written. Generated repository artifacts stay in English regardless of the report language. The human-facing help guide is the one routed surface, and it follows the declared documentation locales and the conversation language, never the report language: it names the Spanish guide only when Spanish is a declared documentation locale and the conversation is Spanish.

After the harness is verified, optional capability questions appear only when relevant. Git initialization and provider publication are separate approval boundaries. For publication, suggest GitHub as one provider, ask whether visibility should be private or public, and require explicit approval before authentication or remote mutation.
