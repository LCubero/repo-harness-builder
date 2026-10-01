# RH-W-0060 implementation plan

> **Status: RH-W-0060 is `completed`.** The [roadmap record](roadmap.md#rh-w-0060) owns its outcome and delivery evidence; the plan below is retained as historical execution evidence, not as instructions to follow.

**Status: owner-approved plan.** On 2026-09-30 the owner approved this plan and delegated implementation. The [approved design](rh-w-0060-design.md) owns the contract; the [roadmap](roadmap.md#rh-w-0060) owns lifecycle and verification evidence.

**Verified premise.** In a scratch repository with `core.autocrlf=true` and `eol=lf`, rewriting a committed file with CRLF only made `git status --porcelain` report ` M`, while `git diff --quiet HEAD -- <path>` exited 0. A substantive edit exited 1, and a differing staged blob made `git diff --cached --quiet HEAD -- <path>` exit 1. Git already applies the path-aware clean filter, so the clause reuses these two commands and needs no new tool.

1. **RED.** Add one focused test beside `test_session_stop_requires_classified_screened_commit_proposals` that renders session-stop and asserts the four design outcomes as contract text: a tracked path whose `git diff --quiet HEAD -- <path>` and `git diff --cached --quiet HEAD -- <path>` both exit 0 is reported EOL/filter-only, not content-modified; exit 1 from the worktree comparison stays content-modified; exit 1 from the index comparison stays content-modified; any other exit, a missing `HEAD:<path>` blob, or an ambiguous filter is `unknown` with the missing evidence named. Confirm it fails before the change.
2. **GREEN.** Add one sentence after the existing step 8 classification in the session-stop template in `skill/repo-harness-builder/scripts/harness_tool.py`. It runs only for a tracked path reported modified, names the compared representations and the result, and never hides a substantive change. No other skill, outcome, or Git command changes.
3. **Sync and verify.** Update the self-hosted outputs (`.repo-harness/`, `.claude/`, `.opencode/`, `.pi/`) through the existing maintenance preview and current-digest apply. Run the focused test, then the canonical suite, then `python .repo-harness/check.py` (`current`, zero writes), and `git diff --check`.
4. **Deliver and close.** One issue and one PR. After merge, record `completed` in the roadmap and current state (EN/ES), the documentation maps, `CHANGELOG.md`/`CHANGELOG.es.md`, and Engram topic `work/RH-W-0060`. Rollback is reverting the single commit.

**Limit stated plainly:** the tests prove the generated contract text; they cannot prove an agent follows it. The scratch-repository probe above is the evidence that the named Git commands behave as the contract says.

**Non-goals:** session-start changes, branch inventory (C12 is closed), automatic cleanup, `.gitattributes` redesign, conflict handling, transport changes, new commands, state, or dependencies, and any implementation before separate authorization.
