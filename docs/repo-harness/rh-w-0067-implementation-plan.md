# RH-W-0067 Response Contract Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILLS: use `test-driven-development` for every behavior change and `verification-before-completion` before delivery. Execute the checkboxes in order. This plan authorizes no implementation, lifecycle transition, commit, push, publication, or external access.

**Goal:** Generate one conditional response contract, route all six response triggers to it, and reduce the three daily contracts to one exact shared reference without changing existing authority boundaries.

**Architecture:** `rule_outputs()` becomes the sole renderer of `.repo-harness/rules/response-contract.md`; `rulebook()` receives one exact gate row and six exact compressions; the three daily skill renderers interpolate one shared concise reference. Existing render inventory, `human_owned()`, `byte_compared_paths()`, check, preview, and apply behavior handle the new file without schema or manifest changes.

**Tech Stack:** Python 3.10+ standard library, `unittest`, deterministic Markdown rendering, and byte-identical canonical, OpenCode, and Claude package projections.

**Lifecycle owner:** [RH-W-0067](roadmap.md#rh-w-0067), status `completed`. The approved [design](rh-w-0067-design.md) owns exact behavior. Completed RH-W-0013 satisfied the prerequisite. This plan was executed under separate direct implementation authority without RH-W-0060 concurrency or Git delivery actions.

## Implementation Result

Completed on 2026-08-24. The required RED failures were observed before production edits, focused GREEN passed 34 tests, canonical/OpenCode/Claude each passed 441 tests, digest-bound maintenance apply returned `VALIDATED`, and the generated checker reported `current` with zero writes. The roadmap owns complete lifecycle and delivery evidence. Commands and checkboxes below remain the approved historical execution sequence.

## File Map

| Path or symbol | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/tests/test_harness_tool.py:4191 RuleFileTests` | RED/GREEN ownership, routing, budget, delimiter, setext, contextual, migration, and attribute coverage. |
| `skill/repo-harness-builder/tests/test_harness_tool.py:6249 QuestionVisibilityTests` | Replace daily syntax assertions with exact retained-reference and non-duplication assertions. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1117 VISIBILITY_CONTRACT` | Rename to one shared exact daily reference string. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1148 session_start_skill` | Interpolate the shared reference once in the Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1280 session_stop_skill` | Interpolate the shared reference once in the Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1391 documentation_skill` | Interpolate the shared reference once in the Output Contract. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1465 rulebook` | Apply six exact compressions and add the exact six-trigger gate row. |
| `skill/repo-harness-builder/scripts/harness_tool.py:1540 rule_outputs` | Render the only complete response-contract owner with provenance and existing budget wording. |
| `.opencode/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Byte-identical package projections after canonical GREEN. |
| `.claude/skills/repo-harness-builder/{scripts/harness_tool.py,tests/test_harness_tool.py}` | Byte-identical package projections after canonical GREEN. |
| `REPO-HARNESS.md` | Self-hosted generated rulebook with equivalent compressions and gate. |
| `.repo-harness/rules/response-contract.md` | New self-hosted generated seed and sole complete syntax owner. |
| `.repo-harness/skills/repo-harness-{session-start,session-stop,documentation}/SKILL.md` | Self-hosted daily outputs containing only the exact shared reference. |
| Roadmap, current state, changelog, localized peers, and Engram topic `work/RH-W-0067` | Completion reconciliation only after implementation, delivery evidence, and separate lifecycle authority. |

No manifest field, checker format, `.gitattributes` entry for the new human-owned rule, generator version, schema, parser, command, prompt, wrapper, runtime-specific rule copy, or dependency changes.

## Task 1: RED For The Rule Owner And Exact Gate

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py:4191-4245`

- [ ] **Step 1: Extend the rule seed and add focused contract tests**

Change `RuleFileTests.SEEDS` to include `.repo-harness/rules/response-contract.md`, then add these methods to `RuleFileTests`:

```python
    RESPONSE_RULE = ".repo-harness/rules/response-contract.md"
    GATE = (
        "| An answer requires a notice, pending action, question, approval, visible next action, "
        "or contextual explanation | `.repo-harness/rules/response-contract.md` |"
    )

    def test_response_contract_has_one_exact_six_trigger_gate(self):
        outputs = TOOL.render("Example", [])
        self.assertEqual(outputs[RULEBOOK].count(self.GATE), 1)
        self.assertIn(
            "Load when: an answer requires a notice, pending action, question, approval, visible next action, or contextual explanation.",
            outputs[self.RESPONSE_RULE],
        )

    def test_response_contract_owns_literal_32_byte_forms_and_setext_safety(self):
        text = TOOL.render("Example", [])[self.RESPONSE_RULE]
        equals = "=" * 32
        pluses = "+" * 32
        self.assertEqual(text.splitlines().count(equals), 2)
        self.assertEqual(text.splitlines().count(pluses), 2)
        self.assertIn(f"```text\n{equals}\nDecision required", text)
        self.assertIn(f"```text\n{pluses}\nNext action", text)
        self.assertIn("setext heading", text)
        self.assertIn("blank line", text)

    def test_response_contract_keeps_context_proportional_without_five_headings(self):
        text = TOOL.render("Example", [])[self.RESPONSE_RULE]
        for dimension in ("why", "purpose", "location", "timing", "method"):
            self.assertIn(dimension, text)
        self.assertIn("only the relevant dimensions", text)
        self.assertIn("never requires five headings", text)
        self.assertIn("evidence classification", text)
        self.assertIn("authority", text)
        self.assertIn("no-unrequested-work", text)
        self.assertIn("authorized-follow-through", text)

    def test_rulebook_compression_preserves_obligations_and_budget(self):
        text = TOOL.render("A Fairly Long Project Name Here", ["opencode", "claude-code", "pi"])[RULEBOOK]
        for replacement in (
            "Apply the smallest sufficient change.",
            "Do not introduce unrelated scope.",
            "After repeated failure, stop with evidence and request a decision.",
            "1. Update when a rule or gate changes.",
            "2. Keep never-miss rules here; move detail to",
            "3. Stay under 700 tokens",
        ):
            self.assertIn(replacement, text)
        for superseded in (
            "Apply the smallest change sufficient for the current objective.",
            "Do not introduce unrelated tools, processes, abstractions, or scope.",
            "After a repeated failure, stop, present evidence, and request a decision.",
            "1. **When.** Update this file when a rule, gate destination, or statement changes.",
            "2. **Where.** Keep never-miss rules here. Move detail to",
            "3. **Budget.** Stay under",
        ):
            self.assertNotIn(superseded, text)
        self.assertLess(TOOL.token_proxy(text), TOOL.RULEBOOK_TOKEN_BUDGET)

    def test_rulebook_contains_no_response_syntax_beyond_the_gate(self):
        text = TOOL.render("Example", [])[RULEBOOK]
        self.assertEqual(text.count(self.GATE), 1)
        for syntax in ("Decision required", "Next action", "setext heading", "=" * 32, "+" * 32):
            self.assertNotIn(syntax, text)
```

- [ ] **Step 2: Observe the missing owner RED**

Run:

`python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rules_directory_is_seeded RuleFileTests.test_response_contract_has_one_exact_six_trigger_gate -v`

Expected: two failures. The first reports `.repo-harness/rules/response-contract.md` absent from outputs. The second reports the exact gate count as `0 != 1`. A syntax error, import error, or different failure is not valid RED.

- [ ] **Step 3: Observe delimiter, setext, context, compression, and separation RED independently**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_contract_owns_literal_32_byte_forms_and_setext_safety -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_contract_keeps_context_proportional_without_five_headings -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rulebook_compression_preserves_obligations_and_budget -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_rulebook_contains_no_response_syntax_beyond_the_gate -v
```

Expected: the first two error with `KeyError: '.repo-harness/rules/response-contract.md'`; compression fails on the first absent replacement; separation passes because the rulebook does not yet contain response syntax. Keep that passing negative control.

## Task 2: RED For Daily References And Existing Lifecycle Behavior

**Files:**
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py:6249-6332`
- Modify: `skill/repo-harness-builder/tests/test_harness_tool.py:4191-4299`

- [ ] **Step 1: Replace RH-W-0058 daily syntax tests with exact reference ownership tests**

Keep `DAILY_SKILLS` and `rendered_daily()`. Replace the methods from `test_every_daily_contract_carries_the_visibility_obligation` through `test_the_rulebook_is_untouched_by_the_visibility_obligation` with:

```python
    REFERENCE = (
        "Response shaping: for any notice, pending action, question, approval, visible next action, or "
        "contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own "
        "reply and reported output."
    )

    def test_every_daily_contract_contains_the_exact_reference_once(self):
        for path, text in self.rendered_daily().items():
            with self.subTest(path=path):
                output_contract = text.split("## Output Contract", 1)[1]
                self.assertEqual(output_contract.count(self.REFERENCE), 1)

    def test_notice_and_pending_action_are_independent_gate_triggers(self):
        gate = TOOL.render("Example", ["opencode"])["REPO-HARNESS.md"]
        self.assertIn("notice, pending action, question", gate)
        self.assertIn("pending action", self.REFERENCE)
        self.assertIn("visible next action", self.REFERENCE)

    def test_daily_contracts_do_not_duplicate_response_syntax(self):
        forbidden = (
            "=" * 32,
            "+" * 32,
            "setext heading",
            "why, purpose, location, timing, and method",
            "a line of `=`",
            "a line of `+`",
        )
        for path, text in self.rendered_daily().items():
            with self.subTest(path=path):
                for phrase in forbidden:
                    self.assertNotIn(phrase, text)

    def test_response_syntax_has_one_generated_owner(self):
        outputs = TOOL.render("Example", ["opencode"])
        owners = [path for path, text in outputs.items() if "Decision required" in text]
        self.assertEqual(owners, [".repo-harness/rules/response-contract.md"])
```

- [ ] **Step 2: Add fresh, missing, human-evolved, attribute, and runtime projection tests**

Add to `RuleFileTests`:

```python
    def test_response_rule_is_missing_then_human_evolved_without_schema_changes(self):
        outputs = TOOL.render("Example", ["opencode"])
        manifest = json.loads(outputs[TOOL.MANIFEST_PATH])
        self.assertNotIn("responseContract", manifest)
        expected_response_rule = outputs[self.RESPONSE_RULE]
        with tempfile.TemporaryDirectory() as directory:
            root = Path(directory)
            install(root, runtimes=("opencode",))
            target = root / self.RESPONSE_RULE
            target.unlink()
            missing = TOOL.check(root, "Example", ["opencode"])
            missing_entry = next(item for item in missing["files"] if item["path"] == self.RESPONSE_RULE)
            self.assertEqual(missing_entry["state"], "missing")
            target.write_text(expected_response_rule + "\nHuman note.\n", encoding="utf-8")
            evolved = TOOL.check(root, "Example", ["opencode"])
            evolved_entry = next(item for item in evolved["files"] if item["path"] == self.RESPONSE_RULE)
            self.assertEqual(evolved_entry["state"], "human-evolved")
            self.assertEqual(evolved["status"], "current")
            self.assertEqual(evolved["writes"], 0)

    def test_response_rule_is_not_byte_pinned_or_runtime_duplicated(self):
        outputs = TOOL.render("Example", ["claude-code", "opencode", "pi"])
        self.assertNotIn(f"{self.RESPONSE_RULE} text eol=lf", outputs[".gitattributes"])
        for prefix in (".claude/", ".opencode/", ".pi/"):
            self.assertNotIn(prefix + self.RESPONSE_RULE, outputs)
```

- [ ] **Step 3: Observe daily and lifecycle RED**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py QuestionVisibilityTests -v
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests.test_response_rule_is_missing_then_human_evolved_without_schema_changes RuleFileTests.test_response_rule_is_not_byte_pinned_or_runtime_duplicated -v
```

Expected: `QuestionVisibilityTests` has four failing methods: exact reference count `0 != 1`, missing `notice, pending action, question`, forbidden legacy `a line of \`=\``, and owner list `[]`. The lifecycle test errors with `KeyError` for the absent response rule. The byte-pinning/runtime test passes as a negative control.

## Task 3: Minimal GREEN In The Canonical Renderer

**Files:**
- Modify: `skill/repo-harness-builder/scripts/harness_tool.py:1117-1590`

- [ ] **Step 1: Replace the shared daily constant**

Replace `VISIBILITY_CONTRACT` and its obsolete placement comment with exactly:

```python
RESPONSE_CONTRACT_REFERENCE = (
    "Response shaping: for any notice, pending action, question, approval, visible next action, or "
    "contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own "
    "reply and reported output."
)
```

Replace the three `{VISIBILITY_CONTRACT}` interpolations at the ends of `session_start_skill()`, `session_stop_skill()`, and `documentation_skill()` with `{RESPONSE_CONTRACT_REFERENCE}`. Do not alter any other daily contract text.

- [ ] **Step 2: Apply the six exact rulebook substitutions and add the exact gate**

In `rulebook()`, make only these replacements:

```text
Apply the smallest sufficient change.
Do not introduce unrelated scope.
Do not retry indefinitely. After repeated failure, stop with evidence and request a decision.
1. Update when a rule or gate changes.
2. Keep never-miss rules here; move detail to `.repo-harness/rules/` with at most one gate row.
3. Stay under {RULEBOOK_TOKEN_BUDGET} tokens, measured as `wc -w` times 1.33. Growth moves down; the budget never rises.
```

Insert exactly this row after the memory gate and before the documentation-map gate:

```markdown
| An answer requires a notice, pending action, question, approval, visible next action, or contextual explanation | `.repo-harness/rules/response-contract.md` |
```

- [ ] **Step 3: Add the sole response rule output**

In `rule_outputs()`, after `agreements`, define `response_contract` with this complete content:

````python
    response_contract = f"""# Response Contract

{marker}

Load when: an answer requires a notice, pending action, question, approval, visible next action, or contextual explanation.

## Visibility

- A notice, pending action, or question that must survive skimming gets a separate block with a short label in the reply language.
- A blocked reply that cannot continue until the human answers uses exactly 32 ASCII `=` bytes above and below the label, necessary explanation, and single final question.
- A reply that asks nothing and names a visible next action uses exactly 32 ASCII `+` bytes above and below the label and action.
- Never put both forms in one reply. Never use `=` for informational content or `+` to ask a question. Never use side borders or box corners.
- When nothing is pending and no visible next action is useful, close with a plain status line and one concrete recommendation, never an empty block.
- This binds the agent's own reply and reported output and strengthens, rather than replaces, the single-final-question rule.

```text
================================
Decision required

<Only the context needed to answer.>

<One concrete final question?>
================================
```

```text
++++++++++++++++++++++++++++++++
Next action

<One concrete next action.>
++++++++++++++++++++++++++++++++
```

A line of `=` directly below prose can become a Markdown setext heading and disappear as a delimiter. Keep a blank line before the opening `=` delimiter and keep both delimiter lines standalone.

## Contextual Explanation

When context helps the human understand or act, explain only the relevant dimensions among why, purpose, location, timing, and method. This semantic rule never requires five headings, a fixed template, empty dimensions, repeated information, or explanation when the answer is already self-explanatory.

## Preserved Boundaries

Use the existing owners for proportionality, evidence classification, authority, no-unrequested-work, and authorized-follow-through. This file does not expand those boundaries.

## Budget

Stay under {RULEBOOK_TOKEN_BUDGET} tokens, measured as `wc -w` times 1.33. A file that outgrows it splits or sheds detail; it never raises the number.
"""
````

Return it beside the two existing rules:

```python
    return {
        ".repo-harness/rules/memory.md": memory,
        ".repo-harness/rules/response-contract.md": response_contract,
        ".repo-harness/rules/working-agreements.md": agreements,
    }
```

- [ ] **Step 4: Run focused GREEN**

Run:

```powershell
python -B skill/repo-harness-builder/tests/test_harness_tool.py RuleFileTests QuestionVisibilityTests GeneratedCoreTests.test_rulebook_stays_within_its_token_budget GeneratedCoreTests.test_rulebook_owns_proportional_verification_principle_for_all_core_skills GeneratedCoreTests.test_rulebook_follow_through_preserves_pause_and_scope_boundaries RepositoryLineEndingContractTests -v
```

Recorded result: the corrected reproducible command selects 34 tests and all pass. If the response-rule wording exceeds the existing budget, shorten only redundant prose while preserving every asserted obligation and both literal examples. Never raise `RULEBOOK_TOKEN_BUDGET`.

- [ ] **Step 5: Work-unit boundary**

Planned commit only after separate authorization: `feat(harness): generate response contract`. Stage canonical script and tests together. Do not stage package projections, self-hosted outputs, lifecycle files, or this plan in this first work unit.

Rollback: restore the prior shared daily constant and interpolations, six prior rulebook strings, remove the gate row and response output, and restore the replaced tests. No schema, manifest, or state cleanup exists.

## Task 4: Synchronize Package Projections And Self-Hosted Outputs

**Files:** the four package projection paths and five generated self-hosted paths in the File Map.

- [ ] **Step 1: Prove canonical suite GREEN before copying projections**

Run:

`python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v`

Expected: exit 0. Stop on any failure. Do not copy a red canonical package.

- [ ] **Step 2: Copy only canonical script and test projections**

Run:

```powershell
python -c "from pathlib import Path; import shutil; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; src=Path('skill/repo-harness-builder'); roots=[Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; [(d.parent.mkdir(parents=True,exist_ok=True),shutil.copyfile(src/p,d)) for p in paths for d in [r/p for r in roots]]"
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
```

Expected: both commands exit 0 with no output.

- [ ] **Step 3: Apply the exact equivalent compression and gate to the human-owned self-hosted rulebook**

Edit `REPO-HARNESS.md` directly with the same six substitutions from Task 3, Step 2, preserving this repository's existing project-specific wording around them. Add the exact six-trigger gate row after the memory gate. This direct edit is required because the self-hosted rulebook is human-owned after seeding and ordinary maintenance apply preserves its bytes rather than replacing it. Do not copy the generic generated Purpose text or remove repository-specific gates.

- [ ] **Step 4: Preview the self-hosted maintenance render**

Run exactly from the repository root:

```powershell
$engramEvidence = '{"surface":"mcp","status":"verified","evidence":"Engram MCP capability verified for this implementation session"}'
$preview = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --engram-evidence-json $engramEvidence | ConvertFrom-Json
$preview | ConvertTo-Json -Depth 20
```

Expected: exit 0; preview reports only the intended create/replacement actions, including creation of `.repo-harness/rules/response-contract.md` and replacement of the three canonical daily skills and their Claude/OpenCode projections. `REPO-HARNESS.md` remains human-owned and is not replaced. The preview must not propose a manifest field change, response-rule runtime copy, or response-rule `.gitattributes` entry. Stop if Gentle AI or Engram preflight is not verified or `blocked` is non-empty.

- [ ] **Step 5: Apply the exact preview once through maintenance mode**

Run in the same PowerShell process:

```powershell
$applied = python -B skill/repo-harness-builder/scripts/harness_tool.py render --root . --project-name repo-harness-builder --runtime claude-code --runtime opencode --runtime pi --license MIT --report-language english --documentation-language en --documentation-language es --conversation-language espanol --engram-evidence-json $engramEvidence --apply --approved-plan-sha256 $preview.planSha256 | ConvertFrom-Json
$applied | ConvertTo-Json -Depth 20
```

Expected: exit 0 and `status` equals `VALIDATED`. Do not pass `--approval`, `--activation builder`, or authority-context arguments. This is the existing digest-bound maintenance path, not new authority behavior.

- [ ] **Step 6: Verify generated ownership and exact bytes**

Run:

```powershell
python -c "from pathlib import Path; p=Path('.repo-harness/rules/response-contract.md'); t=p.read_text(encoding='utf-8'); assert t.count('='*32)==2; assert t.count('+'*32)==2; assert 'source: core/rules:3.0' in t; assert '.repo-harness/rules/response-contract.md text eol=lf' not in Path('.gitattributes').read_text(encoding='utf-8')"
python -c "from pathlib import Path; ref=\"Response shaping: for any notice, pending action, question, approval, visible next action, or contextual explanation, apply `.repo-harness/rules/response-contract.md` to the agent's own reply and reported output.\"; paths=['.repo-harness/skills/repo-harness-session-start/SKILL.md','.repo-harness/skills/repo-harness-session-stop/SKILL.md','.repo-harness/skills/repo-harness-documentation/SKILL.md']; assert all(Path(p).read_text(encoding='utf-8').count(ref)==1 for p in paths)"
```

Expected: both commands exit 0 with no output.

- [ ] **Step 7: Work-unit boundary**

Planned commit only after separate authorization: `chore(harness): synchronize response contract projections`. Stage the four package projections and generated self-hosted outputs that the reviewed apply actually changed. Keep tests with their canonical production commit if project review prefers one combined product work unit; do not split tests from behavior merely to reduce line count.

Forecast: the behavior and tests are likely 180 to 260 changed lines; copied package projections multiply the mechanical diff to roughly 540 to 780 lines. Keep review burden under 400 lines per conceptual work unit by reviewing canonical behavior first, then exact-byte projections and generated outputs as a mechanical synchronization unit. Do not create chained PR machinery unless the measured diff cannot be reviewed as one focused PR.

## Task 5: Full Verification, Lifecycle Completion, And Delivery Boundary

**Files:** all changed implementation paths; lifecycle documents only after Step 4 evidence and separate authorization.

- [ ] **Step 1: Run every package suite**

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py" -v
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py" -v
```

Expected: all three commands exit 0 with equal test totals. Existing runtime-specific skip differences may remain. Any unequal total means projection drift and blocks completion.

- [ ] **Step 2: Run checker, self-test, parity, and scope checks**

```powershell
python .repo-harness/check.py
python -B skill/repo-harness-builder/scripts/harness_tool.py self-test
python -c "from pathlib import Path; import hashlib; paths=['scripts/harness_tool.py','tests/test_harness_tool.py']; roots=[Path('skill/repo-harness-builder'),Path('.opencode/skills/repo-harness-builder'),Path('.claude/skills/repo-harness-builder')]; bad=[p for p in paths if len({hashlib.sha256((r/p).read_bytes()).hexdigest() for r in roots}) != 1]; assert not bad,bad"
git diff --check
git status --short
git diff --stat
git diff --name-only
```

Expected: checker reports `current` and `writes: 0`; self-test reports `pass`; parity and `git diff --check` exit 0; changed paths match this plan and no RH-W-0060 work appears.

- [ ] **Step 3: Run documentation and forbidden-content review**

```powershell
python -c "from pathlib import Path; paths=[Path(p) for p in ['docs/repo-harness/rh-w-0067-design.md','docs/es/repo-harness/rh-w-0067-design.md','docs/repo-harness/rh-w-0067-implementation-plan.md','docs/es/repo-harness/rh-w-0067-implementation-plan.md']]; bad=[str(p) for p in paths if '\u2013' in p.read_text(encoding='utf-8') or '\u2014' in p.read_text(encoding='utf-8')]; assert not bad,bad"
python -c "from pathlib import Path; paths=[Path('docs/repo-harness/rh-w-0067-implementation-plan.md'),Path('docs/es/repo-harness/rh-w-0067-implementation-plan.md')]; banned=('T'+'BD','T'+'ODO','implement '+'later','fill in '+'details','Similar to '+'Task'); bad=[(str(p),term) for p in paths for term in banned if term in p.read_text(encoding='utf-8')]; assert not bad,bad"
python -c "from pathlib import Path; import re; paths=[Path('docs/repo-harness/rh-w-0067-implementation-plan.md'),Path('docs/es/repo-harness/rh-w-0067-implementation-plan.md')]; missing=[]; [(missing.append((str(p),link)) if not (p.parent/link).resolve().exists() else None) for p in paths for link in re.findall(r'\[[^]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)',p.read_text(encoding='utf-8')) if '://' not in link]; assert not missing,missing"
```

Expected: all commands exit 0 with no output. Then re-read every changed file and inspect the complete diff, not only the stat.

- [ ] **Step 4: Reconcile lifecycle only after verified implementation and separate authority**

After observed RED, complete GREEN, reviewed self-hosted apply, all verification above, and explicit lifecycle authorization, update in one work unit:

- `docs/repo-harness/roadmap.md` and `docs/es/repo-harness/roadmap.md`: set RH-W-0067 and Plan 17 to completed with exact RED/GREEN and delivery evidence.
- `docs/repo-harness/current-state.md` and `docs/es/repo-harness/current-state.md`: project completion and select the next roadmap-owned unit without starting it.
- `CHANGELOG.md`: add one factual completed entry.
- Engram topic `work/RH-W-0067`: mirror the completed owner evidence after repository reconciliation.

Do not alter lifecycle status merely because code is locally green. Do not begin RH-W-0060.

- [ ] **Step 5: Issue, commit, PR, and publication boundary**

Issue creation, staging, commits, push, and PR creation require separate explicit authorization and external-access approval. When authorized, create or select one RH-W-0067 issue first, retain its returned number, stage exact paths by work unit, use Conventional Commits, and open one focused PR whose body uses `Closes #` followed by that returned number. The PR reports scope, observed RED messages, GREEN commands, checker zero-write result, mechanical projection provenance, risks, and rollback. Never invent an issue number in advance and never access the provider during plan execution.

Rollback before delivery: revert only RH-W-0067 canonical tests and renderer changes, copy the reverted canonical package files to both projections, run the maintenance preview/apply to remove or restore generated outputs according to the reviewed rollback plan, and rerun all Task 5 checks. Preserve any human-evolved installed response rule; automatic deletion is forbidden. After delivery, use a normal reviewed revert commit rather than history rewriting.

## Acceptance Checklist

- [ ] One generated response rule is the only complete syntax owner.
- [ ] The exact gate and each daily reference name notice, pending action, question, approval, visible next action, and contextual explanation.
- [ ] Both fenced examples have exactly two delimiter lines of exactly 32 ASCII bytes and preserve the setext-safe blank line.
- [ ] Context uses only relevant why, purpose, location, timing, and method dimensions and rejects five mandatory headings.
- [ ] All six rulebook substitutions are exact and preserve proportionality, evidence, authority, no-unrequested-work, authorized follow-through, and the 700-token development control.
- [ ] Daily skills contain the exact reference once and no syntax duplication.
- [ ] Fresh, missing, human-evolved, authorized maintenance, and zero-write checker behavior use existing mechanisms.
- [ ] The manifest and checker formats are unchanged; no new field is invented.
- [ ] The response rule has no runtime copy and no exact-byte `.gitattributes` rule.
- [ ] Canonical and package script/test projections are byte-identical.
- [ ] RH-W-0013 remains completed and RH-W-0060 did not run concurrently.
- [ ] No RDD, schema, parser, AI analysis, runtime dependency, taxonomy, automatic rewrite, unrelated refactor, or budget increase appears.
- [ ] Lifecycle and delivery occur only under their separate evidence and authority gates.

## Remaining Human Decisions

No product decision remains. Implementation and lifecycle completion are complete. Issue creation, commit, push, and PR publication remain unperformed and outside this work.
