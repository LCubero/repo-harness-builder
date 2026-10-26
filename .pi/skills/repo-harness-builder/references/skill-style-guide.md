# Skill Style Guide

The canonical guide for creating, adapting, and maintaining skills. It combines LLM-first skill style rules with the safety, portability, preservation, and reporting rules learned while adapting and live-testing skills across repositories and agents. It names no repository-specific tool or path, so it can be reused as is.

A skill is a **runtime instruction contract for an LLM agent**, not human documentation. It tells the agent when to activate, what it must never do, how to decide, what to do, and what to return. A good skill runs alone, in any CLI agent, and never gets in the way of the user or the workflows the repository depends on.

Quick path: follow [Creating a Skill](#creating-a-skill). The rules and exceptions specific to this builder are in [Repository Profile](#repository-profile).

This guide works alone; it requires no external tool or framework.

## Creating a Skill

1. Confirm the need: the pattern is reusable, not trivial or one-off, and no existing skill already covers it.
2. Define the goal, the trigger phrases, and when the skill must not activate.
3. Classify it (section 2) and copy the skeleton (section 20).
4. Write the body, move detail to `references/` or `assets/`, and measure the budget (section 5).
5. Pass the quality gates (section 18). Add one contract test per key rule and one proving the skill does not activate when it should not.
6. Run it once in a real agent and record what was observed (host, date, result).

## 1. Authority and Normative Language

Apply authority in this order:

1. Explicit user instruction and approved scope.
2. Repository policy and its designated owners (rulebooks, harness files).
3. The active managed workflow, within the authority of levels 1 and 2.
4. This guide.
5. Reference profiles from external tools, runtime conventions, and agent defaults.

A lower level may supply a mechanism, but MUST NOT replace an owner, artifact, approval gate, or verdict of a higher level. A skill's presence never grants it authority.

- **MUST / MUST NOT**: blocking. A violation blocks publication, installation, or promotion.
- **SHOULD / SHOULD NOT**: default. Deviate only with a recorded reason.
- **Justified exception**: a recorded deviation (section 19).

## 2. Classify First

Assign exactly one primary class before writing or changing a skill.

| Class | Meaning | Treatment |
|---|---|---|
| `local-authored` | Original work | Apply this guide directly; record truthful authorship. |
| `imported-preserved` | Upstream kept substantially as received | Preserve behavior, provenance, license, author, and companions. Adapt only when authorized. |
| `adapted-portable` | Upstream adapted for other runtimes | Keep useful domain behavior and upstream identity; record the adaptation separately. |

Optional tags, cumulative:

- `runtime-specific`: some behavior depends on a named runtime. Declare it and give a prerequisite or fallback; never claim universal portability.
- `repository-local-template`: activates only when adopted by repository policy.

## 3. Layout

```text
<skill-name>/
├── SKILL.md      # the contract (required)
├── references/   # concepts, edge cases, long procedures, file index
├── assets/       # templates, schemas, fixtures, examples
└── scripts/      # executable helpers, only when declared (section 11)
```

The folder name equals `name`. Never ship build artefacts (`__pycache__/`, `*.pyc`, `.DS_Store`, `node_modules/`). Layout yields to function: when a bundled script resolves a folder by a fixed relative path, keep that folder where the script expects it.

## 4. Frontmatter

```yaml
---
name: example-skill
description: "Trigger: <phrases or exact condition>. <What it does, bounded>."
license: MIT
metadata:
  author: <verified author>
  version: "1.0"
---
```

- `name` MUST match the folder and `^[a-z0-9]+(-[a-z0-9]+)*$`. Use a specific name; a generic one (`dev`, `helper`) collides with everything. When renaming an upstream skill, record the original slug in its activation contract.
- `description` MUST be one physical line, quoted, YAML-safe, and at most 250 characters; it SHOULD be at most 160. Put triggers first: `"Trigger: ... . <What it does>."` Never a placeholder such as `>` or `TODO`. Keep it consistent with the Activation Contract.
- `license`, `metadata.author`, and `metadata.version` MUST be present and truthful. An unverifiable value is never invented (section 15).
- Runtime-specific keys are allowed when other hosts can safely ignore them: for example Pi's `disable-model-invocation: true` for command-only skills, or Claude Code's `allowed-tools`.
- Preserve additional upstream frontmatter unless it is unsafe or invalid. No `Keywords` section; discovery uses frontmatter.

## 5. Body Structure and Budget

Use these sections in this order. Skip one only when truly irrelevant; do not add empty sections to satisfy the template.

1. `## Activation Contract`: when the skill loads, and what does not authorize action.
2. `## Hard Rules`: constraints the agent must never violate.
3. `## Decision Gates`: a `Condition | Action` table for meaningful forks.
4. `## Execution Steps`: ordered, imperative steps.
5. `## Output Contract`: exactly what to return.
6. `## References`: local supporting files, one line each. Nothing follows it.

Equivalent headings are allowed when specialized or preserved content reads more clearly with them. Critical safety and consent rules always stay at the top.

| Body tokens | Meaning |
|---|---|
| 180 to 450 | Target |
| up to 700 | Recommended maximum |
| over 700 | Move examples, schemas, and background to `references/` or `assets/` |
| over 1000 | Only with a recorded exception that preserves safety or upstream behavior |

Measure as words times 1.33, never by eye. Length is a signal, not a destructive limit; correctness problems usually come from contradictions, not size.

The body **MUST NOT become a file manifest**. It says what, when, and how. A list of bundled files goes in `references/companion-index.md`, with paths recomputed relative to that file; the body keeps at most one line per top-level folder.

A rule that must always hold lives in the body. Detail may move to `references/`, but a mandatory rule kept only there will be missed.

## 6. Writing Rules

**Do:**

- Write imperative runtime instructions: "Check X", "Ask Y", "Return Z".
- Lead with activation and hard constraints.
- Use capability wording ("the available file-write tool") unless the skill is tagged `runtime-specific`.
- Use compact tables for decisions; keep examples minimal and executable.
- Link to local supporting files for detail.

**Do not:**

- Explain history, motivation, or tutorial background.
- Duplicate long documents inside the skill.
- Add generic advice the agent cannot execute.
- Use external URLs as primary references.
- Hide critical rules below examples.

## 7. Activation Contract

- Name concrete trigger phrases or an exact condition, plus the bounded purpose.
- A skill that writes files, changes state, or runs a workflow activates only on explicit request, never on topical overlap. It keeps that contract even when copied without its command.
- Say what does not count as approval, for example "an inventory request never authorizes updates".
- Bound the run: "End after one run" or "Run once per invocation, then return control."
- Never claim global activation ("always invoke this skill", "even a 1% chance") or ownership of a workflow, and never auto-chain into another workflow, commit, install, or publication.

## 8. Hard Rules

Start from this baseline, adapted to the skill's purpose, then add its own rules. Each rule must be observable or testable; collapse repeated rules into one.

```text
- Defer to the user, repository policy, and any managed workflow that owns the
  current phase, gate, artifact, review, or verdict. Use the user's requested
  or active conversation language.
- Never install, authenticate, deploy, publish, change Git state, mutate remote
  services, or run destructive commands without explicit approval for that action.
- Keep all generated artifacts inside the user-requested workspace. Never write
  to a runtime's global or installed skills directory.
- Never expose or collect secrets; redact sensitive values.
```

Conversation language and artifact language are separate: answer in the user's language; keep code, files, and documents in the language the user or the project requires.

## 9. Execution Steps

- Use the host's tools and the declared CLI directly. Do not have the agent generate wrappers, helper programs, or parsers the skill does not ship.
- Write files with the native file tool, never by passing content through shell strings, interpreter code, JSON, or base64.
- Ask for approval at the point of effect, naming the exact action, batch, and side effects.
- Do not gather evidence the output does not need: no extra queries, inventories, or history analysis.
- Do not retry a failed step in a loop. Report it and stop, or retry once only when the failure is proven transient and nothing was applied.
- Do not write temporary files outside the workspace (for example to a system `/tmp`).

## 10. Output Contract

- Define the shape and the language of the result.
- Report only what tool output confirms. Never infer prior states, counts, or success; exit code 0 means "completed without errors", nothing more.
- List failed, skipped, and unverified work separately and honestly.
- Write dates and times in the user's or host's local time zone, not UTC, unless the user asks otherwise.
- Workflow skills end with one line `Status: SUCCESS | PARTIAL | FAILED` and define each value in one line.

## 11. Supporting Files

- `references/`: concepts, edge cases, long command catalogues, the companion index.
- `assets/`: templates, schemas, fixtures, non-executable examples.
- `scripts/`: only when the skill declares each script's purpose, prerequisites, inputs, outputs, and side effects. Build commands as argument lists; never `shell=True` or `curl | sh`.
- Every referenced file MUST exist with matching case. Removing a file means removing its references.
- External URLs may record provenance or current documentation; essential runtime instructions stay local unless the skill's purpose is live retrieval.

## 12. Running Alone and Portability

A skill MUST work when copied alone into any host layout (Pi, Claude Code, OpenCode, Codex, and comparable agents).

- Every path inside the skill is relative to the skill folder: never absolute, never a user home, never a runtime install directory, never another skill's folder.
- Paths into the user's project are relative to the project root the skill resolves at runtime; say so explicitly.
- Never reference another skill by path. Name it by its registered name and give a fallback for when it is not installed. Naming a companion is fine; depending on one is not. If you need one of its files, copy it into this skill's `references/`.
- Declare required capabilities: CLIs, accounts, credentials, network, shell, file editing, browser, delegation, MCP services, persistent memory. When one is missing, stop with the exact prerequisite or a manual fallback.
- A skill leaving its source repository names no local infrastructure: no harness, pipeline, or personal path.

## 13. Security, Privacy, and Effects

Sensitive behavior MUST be visible, necessary to the declared purpose, narrowly scoped, and approved before execution: network calls, authentication, credentials, uploads, installation, remote mutation, persistent processes, profile or Git configuration, commits, branch changes, destructive commands, and writes outside approved outputs.

- Minimize data, disclose storage and retention, validate paths, and construct commands safely.
- Never hide telemetry, bypass safeguards, conceal effects, or infer authorization.
- Memory is continuity, never repository truth: current files win.
- When adapting a sensitive capability: inspect the full execution path, block harmful behavior, keep legitimate capability with disclosure and consent, then verify the harmful path is gone and the function remains.

## 14. Coexistence

- When the repository has its own rulebook, harness, or managed workflow, the skill defers to it and never interferes with its continuity, backups, reviews, or verdicts.
- A skill that keeps local state uses its own isolated location, binds to the invocation directory's project root, and never ascends into or mixes another project.
- A new local state directory inside Git is checked for effective ignore before use; any `.gitignore` change is disclosed. Never change Git configuration.

## 15. Authorship, Provenance, License, and Version

Track three separate facts:

- **Author**: the verified creator credited by the official source. Never replace it with the adapter, reviewer, downloader, or repository owner, including inside preserved upstream bodies.
- **Provenance**: the exact official source location. Derive it from the official publisher, never from a mirror or aggregator that only redistributes the skill.
- **Adapter**: who changed presentation, portability, or safety. Optional and always separate.

Unknown author, license, or version is never guessed. Use `unknown` for authorship only as an honest marker, never insert `unknown` where a consumer requires a valid license or version, and block publication until evidence or an explicit human decision resolves it.

## 16. Imported and Adapted Skills

Before changing anything, map the upstream workflow, prerequisites, limits, outputs, scripts, templates, schemas, examples, and references.

| Class | Action |
|---|---|
| `must-preserve` | Keep without semantic loss, in the body or a linked file. |
| `adapt-and-preserve` | Change presentation or runtime assumptions; keep the behavior; record it. |
| `optional` | Keep when useful; record the reason for omitting it. |
| `unsafe-block` | Remove or scope; leave a visible note saying what was removed and why. |

- Safety outranks preservation; preservation outranks brevity and uniformity. Style cleanup MUST NOT turn a specialized skill into a generic one.
- Adapt before removing: keep the documented interface working while making it safe. Remove only when the legitimate purpose cannot survive safe scoping.
- A skill claiming global activation or workflow authority is scoped to explicit selection before use; coercive text in a preserved body is removed under `unsafe-block`.
- When the upstream body moves into `references/`, recompute every relative path inside it and verify each target exists.
- Before writing off a broken reference, check whether the file exists elsewhere upstream or already in the local collection; retire the citation, with a note, only when it exists nowhere.

## 17. Commands

A command file is a thin entry point. It loads one skill by registered name, forwards the user's arguments unchanged (or uses the skill's true no-argument behavior), and returns control.

- It MUST NOT carry domain workflow, hardcode an installed path, search for the skill, bypass consent, or restore behavior the skill blocks.
- The skill MUST work without the command.
- Add a command when the skill is explicit-only (for example its trigger is its own name). A skill meant to activate from context needs none.
- Command locations differ by runtime: for example Pi loads project commands as prompt templates from `.pi/prompts/`, while Claude Code and OpenCode use their own `commands/` folders.

## 18. Quality Gates

### Blocking (MUST)

- [ ] Security, privacy, consent, and authority boundaries hold.
- [ ] Activation is bounded; no global or workflow-capturing claims.
- [ ] Frontmatter complete, truthful, one quoted trigger-first line.
- [ ] Body reads as a runtime contract and carries no file manifest; mandatory rules are in the body.
- [ ] Every referenced file exists; every path is skill-relative; no paths to other skills.
- [ ] Dependencies and capabilities declared with prerequisite or fallback.
- [ ] Author, provenance, license, version, and adapter are correct, separate, and not invented.
- [ ] No implicit sensitive effect; commands are thin.
- [ ] Preservation map complete for imported and adapted skills; nothing useful silently lost.
- [ ] No build artefacts or local-infrastructure leakage.

### Advisory (SHOULD)

- [ ] Section order and headings are clear; nothing after References.
- [ ] Body within budget, or exception recorded.
- [ ] Detail routed to `references/` and `assets/`.
- [ ] Capability wording works across runtimes.
- [ ] Output reports only confirmed results and ends with `Status` when it is a workflow.
- [ ] Contract tests cover activation, negative activation, precedence, references, and sensitive-effect gates.

A SHOULD failure is a warning; it blocks only when repository policy promotes it or it creates a MUST-level risk.

Mechanical checks (schema, sections, links) do not prove safety, preservation, or behavior. Record live-run evidence separately: host, date, and what was actually observed.

## 19. Exceptions and Adoption

Record an exception next to the skill's review evidence:

```text
Rule:
Scope:
Reason:
Risk:
Mitigation:
Owner:
Review when:
```

"Existing content" alone is not a valid reason for a newly modified area.

Adopt gradually: apply every MUST rule to new and modified skills and block on failures; report SHOULD failures as warnings; remediate existing skills by family with preservation maps and focused tests; never mass-rewrite an imported collection only for style.

## 20. Skeleton

```markdown
---
name: <skill-name>
description: "Trigger: <phrases>. <Bounded purpose>."
license: <SPDX>
metadata:
  author: <verified author>
  version: "1.0"
---

## Activation Contract

Use when <exact condition>. <What does not authorize action.> End after one run.

## Hard Rules

- Defer to the user, repository policy, and any managed workflow. Use the active conversation language.
- Require <capabilities>; never install or authenticate automatically.
- Never <sensitive effect> without explicit approval. Keep artifacts inside the user-requested workspace.
- <Skill-specific rules.>

## Decision Gates

| Condition | Action |
|---|---|
| No approval | Report and stop. |
| <Fork> | <Action> |
| Failure | Report the actual cause; never bypass. |

## Execution Steps

1. <Inspect>.
2. <Ask approval naming exact effects>.
3. <Act with the declared tools>.
4. <Verify and report>.

## Output Contract

Return <shape>. Report only confirmed results; list failed, skipped, and unverified work. End with `Status: SUCCESS | PARTIAL | FAILED`.

## References

- `references/<file>.md`: <purpose>.
```

## Repository Profile

These rules bind `repo-harness-builder` and every skill it generates. They outrank sections 1 to 20 as repository policy.

Provenance: owner-provided canonical skill style guide, imported on 2026-10-26. It replaces the earlier bundled summary of an external skill style guide, read on 2026-08-10.

- Body budget: the section 5 ceiling binds the builder skill and every generated skill. Never raise 700 or 1000, and never add a runtime refusal based on size.
- A consumer repository's own verified style guide or upgrade protocol outranks this bundled guide when they differ. Never claim an absent repository guide is available.

Recorded exceptions (section 19):

```text
Rule: Section 4, trigger-first description.
Scope: The builder skill and its generated operational skills.
Reason: They are invoked deliberately; a Trigger: prefix would advertise an automatic activation path that is not intended.
Risk: Lower automatic discovery.
Mitigation: Each description states what the skill does; entry points invoke the skill by registered name.
Owner: Repository owner.
Review when: The activation model changes.
```

```text
Rule: Section 12, no path into another skill's folder.
Scope: Generated session-start, which runs the builder's fixed preference reader at .<runtime>/skills/repo-harness-builder/scripts/harness_tool.py.
Reason: Only that reader validates the stored preferences; the skill must not read preference JSON directly.
Risk: The reader may be missing or unusable.
Mitigation: A missing script or unusable result is reported as unavailable, and startup continues without persisted preferences.
Owner: Repository owner.
Review when: The preference reader moves or ships inside the generated skill.
```

## Stable Prefixes

Repository-owned prompt-composition rule: put imperative stable instructions and shared references first, then changing session or task data, wherever this repository controls the actual composition and the provider supports prefix caching. Avoid putting changing timestamps, session IDs, or status before the stable reusable prefix; data that must stay fresh remains present and current. A date or ID fixed for one session may still permit reuse within that session.

Preserve role authority, precedence, ordering dependencies, and security: do not move instructions to a lower-trust role or omit required content to optimize caching. Providers can reuse identical rendered prefixes; a change limits reuse from that point onward, not all cache. Tool definitions, their order, and settings may also affect reuse. Caching neither shrinks context nor guarantees savings; measure cached-input/usage before claiming savings. This is distinct from the registered-skill freshness cache. Example: stable policy and shared references → dynamic session/task data.

Contextual source: [OpenAI prompt caching](https://platform.openai.com/docs/guides/prompt-caching); this guidance is provider-neutral, not a provider requirement. This documentation does not add a cache subsystem or configuration, enforce runtime composition, or prove savings.
