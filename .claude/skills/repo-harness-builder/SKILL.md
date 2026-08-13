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

- Create only absent files. The sole exception appends the exact selected-runtime directive to root `CLAUDE.md` or `AGENTS.md`. Never replace, normalize, or delete existing bytes.
- Version control is the safety net. Do not present this tool as a boundary against a hostile filesystem.
- Resolve and inspect only the selected repository. Never traverse links, siblings, aliases, or external references.
- Show four action sets. Approve only the current digest; never reuse it.
- Approval answers are case-sensitive uppercase: exactly `APROBAR` or `DENEGAR`; reject everything else.
- Keep wrappers thin and path-free: they load a generated skill by registered name, and Pi-like runtimes use their prompt mechanism.
- Keep repository evidence authoritative. Integrated mode requires Gentle AI and Engram; standalone mode must contain neither requirement.
- Never add automatic Git, provider, backup, publication, or specialized methodology behavior.
- Ask one blocking decision per turn. Localize user reports; keep generated artifacts in English.
- Use Python 3.10+ and the standard library only.

## Decision Gates

| Evidence | Action |
|---|---|
| Managed path is absent | Create it |
| Managed path already exists | Leave it untouched and report it |
| Required root pointer exists without its exact directive | Append only that newline-delimited directive |
| Required root pointer already contains its exact directive | Leave every byte unchanged |
| Generated file differs from the rendered bytes | Report `modified`; the human decides |
| Human-owned file lost a promised section | Report the missing sections; never rewrite it |
| Repository declares a license | Pass its SPDX identifier to `--license` |
| No license found | Generate `UNLICENSED` and say so in the report |
| Integrated Gentle AI or Engram preflight fails | Refuse approval and apply; surface the failure |
| `--standalone` selected | Generate repository-only continuity instructions |
| Runtime profile disabled or unknown | Do not generate its wrapper |
| No runtime supplied | Report every runtime surface found and ask; never infer one |

## Execution Steps

1. Read `references/audit-and-interview.md`, resolve the report language, and check the repository without writes.
2. Report what is absent across every runtime profile inspected, then ask one question: which profiles to include. That answer is the permission to plan. Stop for it.
3. Read `references/architecture.md`. For integrated mode, validate the Gentle AI CLI with its available status, doctor, or version capability and validate the Engram MCP or CLI before approving or applying. Refuse on failure. Skip this dependency preflight only for `--standalone`.
4. Read `references/preservation-and-safety.md` and classify each target as create, append, identical, or present.
5. Show the plan and `planSha256`, then follow the exact write-approval contract in `references/audit-and-interview.md`. Proceed only for exact `APROBAR`.
6. Run `scripts/harness_tool.py render --apply --approved-plan-sha256 HASH` with the approved digest and resolved `--license`, then `check`. Confirm changed bytes, resolved links, and zero writes from the check.
7. Validate with `references/validation.md` and report limitations.

## Output Contract

Return status, all action sets, generated tree, verification outcomes, exclusions, residual risks, and readiness.

## References

- `references/audit-and-interview.md` - repository audit and localized decisions.
- `references/architecture.md` - generated core and runtime projection.
- `references/preservation-and-safety.md` - write semantics and containment limits.
- `references/validation.md` - behavior-level acceptance checks.
- `references/gentle-ai-style-guide.md` - local skill style summary.
