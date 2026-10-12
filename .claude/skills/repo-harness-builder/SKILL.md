---
name: repo-harness-builder
description: "Build or check a minimal documentation and continuity harness in the selected repository with bounded root runtime pointers."
license: MIT
metadata:
  author: Luis Diego Cubero Chavarria
  version: "3.1"
---

## Activation Contract

Invoke the skill deliberately: Claude Code `/repo-harness-builder`, Pi `/skill:repo-harness-builder`, or OpenCode's `skill` tool. The exact approval UI and strict rules apply only during an explicitly active builder execution. Repository identity, self-hosting, maintenance, generated-file ownership, session skills, and direct questions do not activate it. Otherwise use ordinary conversational approval and the CLI's default `--activation maintenance`.

Optional arguments may name a report language, target repository, or constraints; resolve language precedence under `references/audit-and-interview.md`. With no arguments, use the current working directory.

## Hard Rules

- Work only in the selected repository. Never follow links, aliases, siblings, or external references.
- Preserve existing content through informed integration. Never overwrite or archive without explicit approval; version control is not a hostile-filesystem boundary.
- For installation/update, show all four action sets and approve only the current content-bound plan digest; never reuse it.
- Strict answers belong only to active builder execution: English `APPROVE`/`DENY`; Spanish `APROBAR`/`DENEGAR`. Maintenance omits that domain. No runtime, profile, methodology, Git, provider, backup, publication, or release authority is implicit.
- Keep repository evidence authoritative. Compose Gentle AI and Engram independently; use only when available or operation/policy-required. Standalone uses repository files only. Never invent workflows or claims.
- Keep conversation, report, documentation, and artifact languages independent. Technical artifacts stay English. Ask one blocking question at a time.

## Decision Gates

| Situation | Load |
|---|---|
| Begin audit or resolve ordered interview | `references/audit-and-interview.md` |
| Runtime/capability selection or architecture question | `references/architecture.md` |
| Classify existing paths, plan, or apply | `references/preservation-and-safety.md` |
| Choose a license only when none is declared | `references/license-selection.md` |
| Correct authority premises or inspect transport | `references/authority-correction.md` |
| Validate behavior, report limitations | `references/validation.md` |
| Edit generated-skill style | `references/gentle-ai-style-guide.md` |

## Execution Steps

Before step 1, if the user explicitly requests inspect/check, take the read-only inspection branch below and stop; otherwise continue the existing installation, update, or correction flow unchanged.

First read `references/validation.md`; if it is missing, unsafe, unreadable, or untrusted, stop without running the checker and report that limitation after evaluating the reference. Then validate the selected root. Require directly contained `.repo-harness/check.py` and `.repo-harness/manifest.json` files: both must be readable regular files with safe direct ancestors. Missing, unsafe, unreadable, or untrusted evidence is a reported limitation and stops. From the selected root run exactly `python -B .repo-harness/check.py` with no arguments. Use authenticated declared configuration only: the installed checker must authenticate the exact manifest bytes and recognized schema. Never infer runtimes, profiles, languages, or capabilities from raw manifest fields. Report exact current/drift/errors/limitations from the checker. Stop without installation interview, fallback, repair, apply, synchronization, Git, or mutation. Direct CLI `check` remains a separate read-only rendered comparison, not this active-builder inspection branch.

1. Read `references/audit-and-interview.md`; inspect read-only and resolve report language. Ask for conversation language and stop; after its answer ask documentation languages and stop, unless `recordedLanguage` supplies them.
2. Report absent runtime surfaces and ask which profiles to include; stop. Do not infer profiles.
3. Read `references/architecture.md`. For integrated mode, gather independent Gentle AI and Engram evidence; skip detection for `--standalone`.
4. Resolve a missing license using `references/license-selection.md`; resolve the optional contribution guide under `references/audit-and-interview.md` (check before asking; preserve existing guides); read `references/preservation-and-safety.md`, audit existing content, and resolve integration versus archive/start clean before planning.
5. Show the four action sets and current plan digest. Never advertise an entry point blocked by the plan. In active execution only, follow the strict language domain in `references/audit-and-interview.md`.
6. Pass conversation, documentation, and report languages explicitly to both preview and apply. Invoke the CLI with all resolved choices: `scripts/harness_tool.py render --root ROOT --project-name NAME --runtime RUNTIME --license LICENSE --report-language REPORT_LANGUAGE [--documentation-language en | --documentation-language es | --documentation-language en --documentation-language es] --conversation-language CONVERSATION_LANGUAGE --engram-evidence-json HOST_EVIDENCE [--contribution-guide] [--standalone] --activation builder --apply --approved-plan-sha256 HASH --approval TOKEN`; repeat `--runtime` for every approved profile, preserve all interview answers/options, then run `check`.
7. Read `references/validation.md`; report observed checks and limitations. If an existing contribution guide was found, suggest reviewing it together with the consumer without judging or editing it. Do not continue into Git or publication without their separate approval.

## Output Contract

For installation/update, report status, readiness, four action sets, generated tree, verification outcomes, exclusions, residual risks, and the current plan digest. For inspection, report only the observed checker outcome and limitations; do not invent action sets, a generated tree, or an apply plan. Distinguish observed evidence from unknowns; do not claim runtime savings or live-agent guarantees without measurement.

## References

Load only the owner a gate names, when triggered.
