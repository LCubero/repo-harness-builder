# RH-W-0073 Implementation Plan

**Status: RH-W-0073 is `completed`. This plan was executed on 2026-08-27 and its body is retained as historical execution evidence, not as instructions to follow.**

Executed against the owner-approved [RH-W-0073 design](rh-w-0073-design.md) and roadmap Plan 22. Its Spanish peer is [rh-w-0073-implementation-plan.md](../es/repo-harness/rh-w-0073-implementation-plan.md).

## Baseline Confirmed Before Any Edit

Re-measured at `d7e8cea` rather than trusted from the design, which measured at `e290c53`:

| Surface | Before | Budget | After |
|---|---|---|---|
| `REPO-HARNESS.md` | 763 | 700 | 650 |
| `.repo-harness/rules/working-agreements.md` | 344 | 700 | 483 |
| `.repo-harness/rules/response-contract.md` | 436 | 700 | 436 |
| `.repo-harness/rules/memory.md` | 317 | 700 | 317 |
| `REPO-HARNESS.es.md` | 581 | 700 | 581 |

The breach was unchanged from the design's measurement. 574 words held 763 proxy tokens against a 525-word ceiling.

## Tasks As Executed

- [x] 1. Write the RED budget assertion measuring `REPO-HARNESS.md` and every `.repo-harness/rules/` file on disk, naming the offending path. It failed with `763 not less than 700 : REPO-HARNESS.md holds 763 proxy tokens against 700`.
- [x] 2. Write the companion assertion that `RULEBOOK_TOKEN_BUDGET` is still 700, that `render_with_skill_mapping` contains no budget reference, and that `over-budget` appears nowhere in the tool. It passed before the change and after it, proving no refusal returned.
- [x] 3. Move the announce-versus-execute rule into the `Stopping` section of `.repo-harness/rules/working-agreements.md`, leaving `Execute authorized work you have announced; do not stop at the announcement.` in the rulebook.
- [x] 4. Move the evidence-classification taxonomy into the `Honesty` section of the same file, leaving `Label every claim as verified fact, supported inference, unknown, or human decision required, and never substitute confidence for evidence.` in the rulebook.
- [x] 5. Add the regeneration guard. `.repo-harness/rules/` is render-owned, so a regeneration would drop the moved detail and leave the short rulebook lines pointing at nothing. The assertion checks that each short line is present in the rulebook, that the gate row routing to the rule file is present, and that each moved block still exists in the rule file.
- [x] 6. Correct the dead `over-budget` acceptance criterion in `references/validation.md` and add the criterion covering the development-time measurement. Synchronize both runtime projections of that reference.
- [x] 7. Reconcile `REPO-HARNESS.es.md`. See the deviation below.
- [x] 8. Update the documented suite count in both current-state peers, from 451 to 454.
- [x] 9. Verify: full suite, checker, projection parity.

## One Deviation From Plan 22, With Its Reason

Plan 22 step 5 said to reconcile `REPO-HARNESS.es.md` as an equivalent mirror. The Spanish rulebook was compressed the same way as the English one and then **reverted**, because the compression would have stranded the obligation.

The English rulebook carries a gate row routing to `.repo-harness/rules/working-agreements.md`. The Spanish rulebook does not, and did not before this change: at `d7e8cea` the string `working-agreements` occurs zero times in `REPO-HARNESS.es.md`. The English rulebook has 13 gate-table lines, the Spanish 9.

Moving detail out of the Spanish rulebook into a file the Spanish rulebook never routes to would have satisfied a word count while losing an obligation, which the design's own acceptance criterion forbids. The Spanish rulebook keeps the taxonomy inline, measures 581 against 700, and is outside the measured set exactly as the design specifies.

The two peers therefore differ in structure while stating the same obligation: English delegates it behind a gate row, Spanish carries it inline. That is the only arrangement in which both preserve it.

## Finding Raised, Not Resolved

The Spanish rulebook is missing at least the `working-agreements.md` gate row that the English one carries, and the gate-table line counts differ 13 to 9. That gap predates this unit and is not its scope. It is left for the owner rather than fixed here, because deciding which gate rows the Spanish rulebook should carry is a documentation-parity decision, not a budget one.

## Verification

```
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
  Ran 454 tests in 95.936s
  OK (skipped=9)

python .repo-harness/check.py
  status: current, writes: 0
```

Three assertions were added, moving the suite from 451 to 454. The existing assertion comparing the documented suite count to the real one failed first and was satisfied in the same change.

## Acceptance Criteria Met

- `REPO-HARNESS.md` measures 650 against 700, and `working-agreements.md` 483 against 700 after receiving both blocks.
- No Hard Rule obligation is lost. Each moved block is reachable from the English rulebook through exactly one gate row, asserted rather than reviewed. The Spanish peer retains its obligation inline.
- The development-time assertion fails when any measured surface reaches the budget and names the offending path.
- `render()` and `check()` are unchanged. `RULEBOOK_TOKEN_BUDGET` is still 700. No render refusal and no checker state returned, asserted directly.
- `references/validation.md` no longer claims a removed mechanism emits `over-budget`.
- The checker reports `current` with zero writes and both runtime projections are byte-identical to canonical.

## Not Done

The generator template still renders both blocks inline into every generated rulebook. Whether the compression also changes that template remains the owner's open question, recorded in the roadmap and untouched here. A generated rulebook measures 650 against 700, so no consumer breach is in front of us.
