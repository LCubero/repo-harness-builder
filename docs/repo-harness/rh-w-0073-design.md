# RH-W-0073 Rulebook Budget Enforcement Design

## Decision

RH-W-0073 closes a Hard Rule that is being violated with nothing able to detect it. This repository's self-hosted `REPO-HARNESS.md` exceeds the 700 proxy-token budget the file itself declares, and the acceptance criterion that would have caught it describes a mechanism completed RH-W-0013 deliberately removed.

The design answers three questions and nothing else: which control surface detects a breach, which exact detail moves down out of the rulebook to return it under budget, and what replaces the dead acceptance criterion.

Review this design against the canonical [RH-W-0073 roadmap record](roadmap.md#rh-w-0073), the [RH-W-0013 design](rh-w-0013-design.md) whose removal must not be reopened, and the [Spanish peer](../es/repo-harness/rh-w-0073-design.md).

## Measured Baseline

Measured read-only with the product's own `TOOL.token_proxy` and `TOOL.RULEBOOK_TOKEN_BUDGET`, at `HEAD` `e290c53`:

| Surface | Proxy tokens | Budget | State |
|---|---|---|---|
| `REPO-HARNESS.md` (self-hosted) | 763 | 700 | over |
| `REPO-HARNESS.es.md` (declares the same budget) | 581 | 700 | under |
| `.repo-harness/rules/response-contract.md` | 436 | 700 | under |
| `.repo-harness/rules/working-agreements.md` | 344 | 700 | under |
| `.repo-harness/rules/memory.md` | 317 | 700 | under |
| Rendered rulebook for a long project name | 650 | 700 | under |

The maximum word count that still measures under 700 is 525. The self-hosted rulebook holds 574 words, so at least 49 words must leave it.

Two further facts frame the gap. `token_proxy` has zero production callers: it survives only in tests, and every surviving budget test asserts on `TOOL.render(...)` output rather than on any file on disk. And `over-budget` now occurs exactly once in the whole product, as an acceptance criterion in `references/validation.md`, with no code, checker branch, or test able to emit it.

The breach is recent and monotonic: 699.58 proxy at `a2257ef` on 2026-08-23, 715.54 at `7ef4993` on 2026-08-24, and 763.42 at `41a23d7` on 2026-08-25. It opened the day the removal shipped and has widened since.

## Two Separate Problems

They are separated deliberately, because they can be answered differently.

1. **The breach.** This repository's rulebook is over its own budget today. Fixing it is a bounded editorial move, not a mechanism.
2. **The absent control.** Nothing measures a hand-evolved rulebook or rule file on disk against the budget it declares. Without this, the breach reopens the next time a unit adds a Hard Rule.

Fixing only the first leaves the harness with a rule that nobody can enforce. Fixing only the second leaves a known violation in the file that loads every session.

## Control Surface

The control is a **development-time test in this repository's canonical suite** that measures the self-hosted budgeted surfaces on disk against `RULEBOOK_TOKEN_BUDGET`.

This reuses a mechanism the suite already has rather than inventing one. `test_harness_tool.py` already resolves the repository through `ROOT.parents[1]` and asserts against real checked-in files: roadmap lifecycle fields, current-state agreement with its owner, line endings, and changelog peer coverage all work this way. A budget assertion joins that existing family.

Measured set:

- `REPO-HARNESS.md`, the file loaded every session.
- Every file under `.repo-harness/rules/`, each of which states the same budget in its own `Budget` section.

`REPO-HARNESS.es.md` is deliberately excluded. It declares the same budget and measures 581, so there is no breach in front of us, and the record scopes this unit to the rulebook and its rule files. Measuring it would solve a problem that has not arrived.

What the control must not become is as important as what it is:

- No render refusal. `render_with_skill_mapping()` gains no budget `require()`. RH-W-0013 removed exactly that and does not reopen.
- No checker branch. `check()` gains no `over-budget` state, no new status, and no new drift path. It stays read-only and non-blocking.
- No cap on consumer documentation. The control measures this repository's own self-hosted surfaces. A consumer's hand-evolved rulebook remains theirs, and the RH-W-0013 exclusion against an arbitrary cap on human documentation stands.
- No raised constant. `RULEBOOK_TOKEN_BUDGET` stays 700. The rulebook states the budget never rises, and this unit rejects raising it.

An advisory measurement reported by `check.py` without changing status was considered and rejected. It would put budget logic back inside the runtime checker for no gain the development-time test does not already deliver, and any later unit could turn an advisory field into a blocking one without reopening this decision.

## Compression

Two blocks move down, both into `.repo-harness/rules/working-agreements.md`. They follow the RH-W-0067 precedent, which kept the obligation in the rulebook as one short line and moved the detail into the rule file that owns it behind a gate row. Here that file already exists and already has its gate row, so no surface is added.

**Move 1: the evidence-classification taxonomy (81 words).** The Hard Rules bullet beginning "Classify each claim as verified fact" carries four definitions, a substitution ban, and a verify-before-asking procedure. The obligation is never-miss and stays as one line. The taxonomy is detail and moves into the existing `.repo-harness/rules/working-agreements.md`, under its `Honesty` section, which already carries "Verify before claiming". No new rule file and no new gate row, because the rulebook already routes to that file. Estimated net saving: 57 words.

A dedicated `evidence-classification.md` was considered and rejected. An 81-word taxonomy does not justify a new owned surface, a new gate row, and a third place to look. The working agreements already own the honesty obligation this taxonomy refines.

**Move 2: the announce-versus-execute rule (35 words).** The Hard Rules bullet beginning "Do not stop after merely announcing" is execution discipline, which `.repo-harness/rules/working-agreements.md` already owns behind an existing gate row. It moves there in full and leaves a compressed one-line rule behind. No new gate row is needed. Estimated net saving: 23 words.

Both moves land in the same existing file. Together they return the rulebook to roughly 494 words, about 657 proxy tokens, leaving roughly 43 tokens of headroom. `working-agreements.md` rises from 344 to about 499 proxy tokens, well inside the same budget.

That headroom is deliberately modest and is not the mechanism. The control is. A large margin would only postpone the next breach; a measured assertion catches it on the day it lands.

The exact replacement wording is implementation work under this design. The blocks named here are the design's commitment; a plan may not substitute a different block without returning to the owner.

## Corrected Acceptance Criterion

`references/validation.md:12` currently reads, in part, "each budgeted file reports `over-budget` once it passes the limit". No mechanism does that. The criterion is corrected to state what is true after RH-W-0013: each budgeted file declares its limit, while runtime render and check stay non-blocking. A second criterion records the development-time measurement of the self-hosted surfaces.

Per the recorded plan-coherence relationship, RH-W-0073 owns this file for this change and RH-W-0072 does not edit it. RH-W-0072 keeps every other documentation-drift surface.

## Acceptance Criteria

- The self-hosted `REPO-HARNESS.md` measures under 700 proxy tokens, and `.repo-harness/rules/working-agreements.md` stays under it after receiving both moved blocks.
- The Spanish peer stays an equivalent mirror under the i18n contract, reconciled in the same work unit but outside the measured set.
- No Hard Rule obligation is lost. Each moved block is reachable from the rulebook through exactly one gate row.
- A development-time test fails when any measured self-hosted surface reaches the budget, and names the offending path.
- `render()` and `check()` behavior is byte-identical for every existing input. Exact-700 and exact-701 rulebooks still render, and a large human-evolved rule file still reports `human-evolved` with overall `current`.
- `RULEBOOK_TOKEN_BUDGET` is unchanged at 700.
- `references/validation.md` contains no claim that a removed mechanism emits `over-budget`.
- The checker reports `current` with zero writes, and both runtime projections stay byte-identical to canonical.

## Tests And Evidence

Implementation is test-first and focused, not a full recursive suite. Expected RED cases:

1. The self-hosted `REPO-HARNESS.md` measures at or above 700. This must fail before the compression and pass after.
2. Each `.repo-harness/rules/` file measures under 700, with the failing path named.
3. Each moved obligation resolves from the rulebook through exactly one gate row.
4. `render()` still emits an exact-700 and an exact-701 rulebook unchanged, proving no refusal returned.
5. `check()` reports no `over-budget` state for an oversized human-evolved rule file.

Focused GREEN plus the canonical suite, checker `current` with zero writes, and projection parity are sufficient closing evidence.

## Non-Goals

This design adds no runtime budget feature, no manifest field, no checker state, no schema change, no parser, no new status vocabulary, no cap on consumer or human documentation, no raised constant, and no reopening of RH-W-0013. It does not correct any RH-W-0072 documentation surface other than the one acceptance criterion named above, and it does not authorize implementation.

## Migration And Compatibility

Consumer-visible behavior does not change. No installed manifest is rewritten, no generated output changes shape, and no existing approval is invalidated. A consumer whose own rulebook is over budget continues to render and check exactly as today.

The compression changes this repository's self-hosted `REPO-HARNESS.md` and `REPO-HARNESS.es.md` and extends one existing rule file. It adds no rule file and no gate row. Both language peers reconcile in the same work unit under the i18n maintenance contract.

## Lifecycle Relationship

RH-W-0073 stays `proposed` until the owner approves this design. It coordinates with three other plans without merging any of them:

- **RH-W-0072** yields `references/validation.md` for this change only.
- **RH-W-0074** and **RH-W-0075** both land in `.repo-harness/rules/working-agreements.md` and both are forbidden from adding rulebook bytes while this unit is open. Completing RH-W-0073 is what lifts that constraint. Both moves here land in that same file, so the three units must not be implemented concurrently and RH-W-0073 goes first. After this unit the file sits near 499 of 700 proxy tokens, leaving roughly 150 words for the two later units. That is workable but not generous, and a later unit that needs more must shed detail rather than raise the budget.

An implementation plan and its implementation each require separate explicit authorization.

## Human Decisions

1. Is a development-time control wanted at all, or is the compression alone the whole unit? This design recommends the control, because the compression without it has already been shown to decay within two days.
2. Are the two named blocks the right detail to move down? Both land in the existing working agreements, which keeps the surface count unchanged but concentrates three units on one file.
3. Is the resulting `working-agreements.md` headroom acceptable for RH-W-0074 and RH-W-0075, or should one of the two blocks go somewhere else to leave them more room?
