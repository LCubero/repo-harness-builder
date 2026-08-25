# RH-W-0013 Minimal Context Budget Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: use `test-driven-development` for behavior changes and `verification-before-completion` before delivery. This plan authorizes no implementation or lifecycle change.

**Goal:** Remove two runtime budget blockers while preserving development-time template budgets.

**Architecture:** Delete the budget refusal in `render_with_skill_mapping()` and the `over-budget` drift branch in `check()`. Add no runtime budget feature, schema, parser, propagation, generated content, or new owner.

**Tech Stack:** Python 3.10+ standard library, `unittest`, and exact canonical package projections.

**Lifecycle owner:** [RH-W-0013](roadmap.md#rh-w-0013), status `ready`. The [design](rh-w-0013-design.md) owns scope.

Only `REPO-HARNESS.md` is always loaded; rules and daily skills are conditional. The documentation-only guidance remains 180-450 target, 451-700 acceptable, 701-1000 warning, 1001-1200 priority attention, and immediate review advice above 1200. These bands add no runtime behavior.

## File Map

| Path | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Remove the two blocking budget effects; optionally remove now-unused `budgeted()`. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Exact RED/GREEN regressions while preserving template budget controls. |
| `.opencode/skills/repo-harness-builder/scripts/harness_tool.py` | Exact script projection after canonical GREEN. |
| `.opencode/skills/repo-harness-builder/tests/test_harness_tool.py` | Exact test projection after canonical GREEN. |
| `.claude/skills/repo-harness-builder/scripts/harness_tool.py` | Exact script projection after canonical GREEN. |
| `.claude/skills/repo-harness-builder/tests/test_harness_tool.py` | Exact test projection after canonical GREEN. |
| Roadmap, current-state, changelog, and localized peers | Conditional reconciliation only after authorized implementation and delivery evidence. |

No manifest, generated checker, command, prompt, wrapper, rule, daily skill, reference, or vendored style-guide file changes.

## Task 1: Prove And Remove Both Blocking Effects

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py`
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py:2364-2447,3418-3516`

- [ ] **Step 1: Add the exact-700 and exact-701 render regressions**

Add this focused test using the module's existing `mock` import:

```python
class ContextBudgetBlockingTests(unittest.TestCase):
    def test_exact_budget_rulebook_renders(self):
        original = TOOL.rulebook("example", False)
        exact = next(
            original + "\n" + ("filler " * count)
            for count in range(TOOL.RULEBOOK_TOKEN_BUDGET)
            if TOOL.token_proxy(original + "\n" + ("filler " * count)) == TOOL.RULEBOOK_TOKEN_BUDGET
        )
        self.assertEqual(TOOL.token_proxy(exact), TOOL.RULEBOOK_TOKEN_BUDGET)
        with mock.patch.object(TOOL, "rulebook", return_value=exact):
            outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[TOOL.RULEBOOK_PATH], exact)

    def test_exact_701_rulebook_renders(self):
        original = TOOL.rulebook("example", False)
        exact = next(
            original + "\n" + ("filler " * count)
            for count in range(TOOL.RULEBOOK_TOKEN_BUDGET)
            if TOOL.token_proxy(original + "\n" + ("filler " * count)) == TOOL.RULEBOOK_TOKEN_BUDGET
        )
        exact_701 = exact + "\nfiller"
        self.assertEqual(TOOL.token_proxy(exact_701), 701)
        with mock.patch.object(TOOL, "rulebook", return_value=exact_701):
            outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[TOOL.RULEBOOK_PATH], exact_701)
```

The two exact-boundary fixtures must equal 700 and 701. Both current RED results must be the budget `ContractError` from `render_with_skill_mapping()` containing `over its 700 budget`, not fixture setup failures.

- [ ] **Step 2: Observe render RED**

Run:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_budget_rulebook_renders -v`

Expected: one failure or error at the budget `require()`, not fixture setup.

- [ ] **Step 3: Observe exact-701 render RED independently**

Run:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_701_rulebook_renders -v`

Expected: one error at the same budget `require()`, with `ContractError` containing `over its 700 budget`; the exact-701 assertion passes first.

- [ ] **Step 4: Replace the existing checker regression with the intended contract**

Rename `RuleFileTests.test_check_reports_a_budgeted_file_that_outgrew_its_budget` to `test_check_keeps_large_human_evolved_rule_current`. Keep its temporary installation and large append, then assert:

```python
self.assertEqual(entry["state"], "human-evolved")
self.assertNotIn("tokens", entry)
self.assertEqual(report["status"], "current")
self.assertEqual(report["writes"], 0)
```

- [ ] **Step 5: Observe checker RED**

Run:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_check_keeps_large_human_evolved_rule_current -v`

Expected: failure because current code returns `over-budget` and `drift`.

- [ ] **Step 6: Make the minimal production change**

Delete only:

```python
for path, content in outputs.items():
    if budgeted(path, rulebook_path):
        size = token_proxy(content)
        require(size < RULEBOOK_TOKEN_BUDGET, f"{path} is {size} tokens, over its {RULEBOOK_TOKEN_BUDGET} budget")
```

and this branch from `check()`:

```python
elif budgeted(relative, rulebook_path) and size >= RULEBOOK_TOKEN_BUDGET:
    entry["state"] = "over-budget"
    entry["tokens"] = size
```

Remove the now-unused `size = token_proxy(current_text)` assignment. Search callers after these deletions; remove `budgeted()` only if none remain. Do not remove `token_proxy()` or `RULEBOOK_TOKEN_BUDGET`.

- [ ] **Step 7: Verify focused GREEN and preserved controls**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests RuleFileTests.test_check_keeps_large_human_evolved_rule_current ProductWorkflowTests.test_rulebook_stays_within_its_token_budget RuleFileTests.test_rule_files_stay_within_the_same_budget_as_the_rulebook -v
```

Expected: all pass. The final two tests still enforce the development-time rulebook and rule-file budgets.

- [ ] **Step 8: Verify exact-701 GREEN independently**

Run:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py ContextBudgetBlockingTests.test_exact_701_rulebook_renders -v`

Expected: one test passes; the exact-701 output renders unchanged.

- [ ] **Step 9: Work-unit boundary**

Planned commit after separate authorization: `fix(budget): make runtime checks non-blocking`

Rollback only the focused tests and two production deletions. Do not alter RH-W-0067 or generated runtime surfaces.

## Task 2: Synchronize Projections And Verify Scope

**Files:** the six canonical and projection script/test paths in the File Map; lifecycle files only under Step 6.

- [ ] **Step 1: Run canonical GREEN before projection copies**

Run:

`python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`

Expected: exit 0. Stop if canonical tests fail.

- [ ] **Step 2: Copy only script and test projections**

```powershell
python -c "from pathlib import Path; import shutil; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; src=Path('skill/repo-harness-builder'); roots=[Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; [(d.parent.mkdir(parents=True,exist_ok=True),shutil.copyfile(src/p,d)) for p in paths for d in [r/p for r in roots]]"
```

- [ ] **Step 3: Prove byte identity**

```powershell
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
```

- [ ] **Step 4: Run all package suites and generated checker**

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v
python .repo-harness/check.py
```

Expected: all suites exit 0 with equal totals; existing runtime-specific skip differences may remain. The checker reports `current` with `writes: 0`.

- [ ] **Step 5: Inspect exact scope**

Run `git status --short`, `git diff --stat`, `git diff --name-only`, `git diff --check`, and inspect the complete diff. Reject any runtime schema, parser, preview/apply propagation, generated reference, daily-skill refactor, manifest, checker, command, prompt, wrapper, or vendored style-guide change.

- [ ] **Step 6: Reconcile lifecycle only after delivery evidence**

Only after separately authorized implementation, observed RED, complete GREEN, synchronized projections, and reviewed delivery evidence: update both roadmap records, both current-state projections, `CHANGELOG.md`, and Engram topic `work/RH-W-0013` in one documentation work unit. Preserve `ready` until that evidence and authorization support the next lifecycle transition.

## Acceptance Checklist

- [ ] Exact-700 render no longer fails.
- [ ] Exact-701 render no longer fails.
- [ ] Large human-evolved budgeted files remain `human-evolved` and overall `current`.
- [ ] Real drift behavior remains unchanged.
- [ ] `token_proxy()`, `RULEBOOK_TOKEN_BUDGET`, and template budget tests remain.
- [ ] `budgeted()` is removed only if caller search proves it unused.
- [ ] Only canonical script/tests and matching OpenCode/Claude projections change before lifecycle reconciliation.
- [ ] Every excluded artifact remains absent.
- [ ] The vendored style guide remains unchanged.

## Remaining Human Decisions

No product decision remains. Execution and delivery still require separate authorization.
