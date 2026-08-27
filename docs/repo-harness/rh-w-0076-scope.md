# RH-W-0076 Correction Scope

## Decision

RH-W-0076 owns three declared limits with nothing able to detect a breach. This scope names what each correction changes, what control detects a recurrence, and how the work is sliced. It authorizes no implementation.

Review it against the canonical [RH-W-0076 roadmap record](roadmap.md#rh-w-0076), the approved [RH-W-0073 design](rh-w-0073-design.md) whose control pattern this reuses, and the [Spanish peer](../es/repo-harness/rh-w-0076-scope.md).

## The Owner Answer This Scope Rests On

The record's advancement condition asks what the bundled body ceiling governs. `references/gentle-ai-style-guide.md` states "Target 180 to 450 body tokens, recommended maximum 700, hard maximum 1000" in a paragraph that declares no scope; the sentence naming this skill and everything it generates belongs to the next paragraph and governs the `Trigger:` prefix rule.

The owner answered on 2026-08-27: **the ceiling binds both, because the builder and this repository's harness are reciprocal and recursive.** That is verified, not asserted. `REPO-HARNESS.md` states the repository self-hosts and uses the harness generated here to improve the builder itself; `decisions.md` states this self-hosted repository applies the same generic contract to its own product source; and the canonical builder `SKILL.md` and its `.claude/` projection are byte-identical at 16279 bytes. A rule the builder obeys and its own output does not is not one contract applied recursively, it is two.

An earlier draft of this scope proposed the opposite, that generated contracts are a stated exception. That draft is withdrawn, because the project had already rejected the same move one day earlier: the `RH-W-0073` record rejects raising the 700 constant and, in the same field, **rejects treating the breach as acceptable drift**. Renaming drift an exception does not make it a different decision.

So the generated contracts at 2822 and 2707 are a **measured, tracked breach**, not an exemption. The distinction is the whole point: an exception says the rule does not apply, a tracked breach says the rule applies, the output violates it, and what closing it costs is recorded rather than waved away.

What it costs is verified: `references/` appears zero times in `harness_tool.py` for generated output. Generated skills are rendered to exactly one path, `outputs[f"{base}/skills/{name}/SKILL.md"]`, so they have no surface to move detail into. Closing that breach means teaching the renderer to emit a multi-file generated skill, which is renderer work this unit does not carry. `RH-W-0078` owns it.

RH-W-0076 therefore corrects the guide so the ceiling names what it binds, brings the builder body under it, and records the generated-contract breach with its owner and its measured size. It does not close that breach and does not exempt it.

## Corrections

### 1. The unrunnable command, `SKILL.md` Execution Step 6

Replace the printed command with a form that runs. Do not copy the shape from `references/validation.md`: that form is a minimal smoke invocation and it drops `--conversation-language`, `--documentation-language`, and `--engram-evidence-json`, which Execution Steps 1 and 3 exist to obtain. `--conversation-language` carries no argparse default and resolves through `args.conversation_language or DEFAULT_REPORT_LANGUAGE`, so copying that form would silently fall back to English and reproduce, on three further flags, the exact defect this correction diagnoses for `--runtime`.

The replacement keeps every flag Step 6 already prints and adds the four that are missing:

`scripts/harness_tool.py render --root ROOT --project-name NAME --runtime RUNTIME [--runtime RUNTIME ...] --license LICENSE --report-language REPORT_LANGUAGE [--documentation-language en | --documentation-language es | --documentation-language en --documentation-language es] --conversation-language CONVERSATION_LANGUAGE --engram-evidence-json HOST_EVIDENCE --activation builder --apply --approved-plan-sha256 HASH --approval TOKEN`

Three defects close together: the missing required `--root` and `--project-name`, the missing `--activation builder` that makes `--approval` acceptable, and the missing `--runtime` that otherwise discards the permission Execution Step 2 exists to obtain. The surrounding prose in Step 6 already resolves `--license`, so the flag is named here rather than left implicit.

That the dropped flags are material is executed evidence, not reasoning: rendering the same project twice under `--standalone --activation builder`, once with `--conversation-language espanol` and once without it, produces two different plan digests, `a31fd667...` and `ecc3fec3...`. The two runs install different harnesses. Separately, the replacement form above was executed and returned a valid plan with its own digest and approval prompt, while the form Step 6 prints today exits 2 at the parser before any plan exists.

### 2. The style-guide ceiling

Rewrite the budget sentence in `references/gentle-ai-style-guide.md` to name what it binds: the builder skill and every skill it generates. Record, in the same place, that the generated contracts currently exceed it at 2822 and 2707, that `RH-W-0078` owns closing that breach, and that no exemption is granted. Say the two things a reader otherwise conflates: the ceiling is a limit, verified at development time and never at runtime, and the owner-approved bands from completed `RH-W-0013` are a severity vocabulary rather than a permission scale. A 2026-08-27 review of this repository mistook one for the other with every file in front of it, which is evidence that a future reader will. Then bring the builder `SKILL.md` body under the stated ceiling by moving detail into the `references/` files that already own those topics. No reference gains a budget it did not have.

### 3. Plans for finished units

Retire the seven implementation plans whose unit reached `completed`. One mechanism, applied uniformly: a status line directly under the title naming the unit, its terminal status, and that the body is retained as historical execution evidence. Checkboxes stay as written, because they are the historical record of the plan as executed, and the status line removes the instruction to obey them. The English documentation map row for `rh-w-0025-implementation-plan.md` stops calling it executable. Its Spanish peer already reads as a human mirror and never claimed executable, so that peer is parity work, not a correction.

## Control

One development-time assertion family in the canonical suite, reusing the pattern the RH-W-0073 design establishes and the existing self-hosted tests already use through `ROOT.parents[1]`:

- Every command printed in `SKILL.md` as a runnable invocation parses against the real CLI parser.
- The builder `SKILL.md` body measures under the stated ceiling.
- No implementation plan whose roadmap record is terminal lacks its status line.

Nothing enters `render()` or `check()`. RH-W-0013's removal of runtime budget refusal is not reopened.

## Slicing

Three slices, deliverable independently, in this order:

1. **Step 6 alone.** One file, one command, one RED test that parses the documented invocation. It is the only defect that breaks a first install, and it does not depend on the ceiling answer.
2. **Plan retirement.** Seven status lines plus one English map row correction and its Spanish peer kept in parity, with the terminal-status assertion.
3. **The ceiling.** The guide rewrite and the `SKILL.md` compression, last, because it is the largest and the least urgent. Least urgent is not least risky: it moves roughly 65 percent of the builder's runtime instruction body into `references/` files an agent reads only when a step sends it there, so it carries the most behavior risk of the three and is the only slice whose acceptance depends on what the rewritten file still makes an agent do.

Slice 1 may ship before RH-W-0073 because it needs no budget control. Slices 2 and 3 follow RH-W-0073, because they consume its assertion pattern.

## Acceptance Criteria

- The command printed in `SKILL.md` Execution Step 6 parses against the real CLI parser, proven by a test rather than by reading, and carries every value Execution Steps 1 through 4 resolve, so no interview answer is silently discarded by a flag default.
- `references/gentle-ai-style-guide.md` states that its body ceiling binds the builder skill and every skill it generates, and records the generated-contract breach and its owning unit rather than an exemption, and distinguishes the ceiling as a limit from the `RH-W-0013` bands as a severity vocabulary.
- The builder `SKILL.md` body measures under the stated ceiling, with moved detail landing in an existing `references/` file.
- Every Execution Step that survives the compression still names the reference that now owns its moved detail, so no instruction becomes unreachable by an agent that reads only `SKILL.md`. Slice 3 does not merge until that is checked step by step against the pre-compression body.
- Every implementation plan for a terminal unit carries its status line, and no documentation map row calls a finished plan executable.
- `render()` and `check()` behavior is byte-identical for every existing input; `RULEBOOK_TOKEN_BUDGET` is unchanged.
- The checker reports `current` with zero writes and both runtime projections stay byte-identical to canonical.

## Non-Goals

No runtime refusal, no manifest field, no checker state, no schema change, no new rule file, no `references/` directory for generated skills in this unit, which is `RH-W-0078` work rather than a permanent answer, no raised ceiling, no reopening of RH-W-0013, RH-W-0072, or RH-W-0073, and no deletion of retained historical execution evidence.

## Lifecycle Relationship

RH-W-0076 stays `proposed` until the owner approves this scope. It bundles three defects under one shared cause, so it cannot become `completed` until all three are resolved or receive an authoritative terminal disposition. RH-W-0072 keeps every other product-documentation surface; RH-W-0073 keeps the rulebook and rule-file budget; RH-W-0078 keeps the generated-contract body breach and the renderer work that closing it requires. RH-W-0076 can reach `completed` while that breach is open, because it owns declaring the ceiling honestly, not closing every surface the ceiling binds. An implementation plan and its implementation each require separate explicit authorization.

## Human Decisions

1. Answered on 2026-08-27: the ceiling binds both, and the generated-contract breach is tracked under `RH-W-0078` rather than exempted.
2. Is the status-line mechanism the right retirement for finished plans, or should they be archived instead?
3. Does slice 1 ship on its own before RH-W-0073, or does the whole unit wait?

This scope was itself reviewed read-only on 2026-08-27 and corrected before being offered for approval: its slice 1 command reproduced the defect it diagnoses, three of its measurements mixed two methods, and slice 2 overstated the Spanish work. Those corrections are applied above. On the same day the owner answered the ceiling question, and the answer withdrew this scope's original exemption reasoning; two questions remain.
