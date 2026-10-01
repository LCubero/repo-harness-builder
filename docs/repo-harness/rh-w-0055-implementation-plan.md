# RH-W-0055 implementation plan

**Status: owner-approved plan.** On 2026-09-30 the owner approved this plan and delegated implementation. The [approved design](rh-w-0055-design.md) owns the contract; the [roadmap](roadmap.md#rh-w-0055) owns lifecycle and verification evidence.

This is a documentation-only change. It touches no source, test, generated behavior, checker, or runtime projection.

1. **Place the path where users already look.** Expand Quick Start step 1 in `README.md` and `README.es.md` into one short `Install The Builder` subsection under the existing `quick-start` role. Create no new document, doc-role marker, or relationship edge; the README pair is already declared.
2. **Write the path in design order, linking instead of restating.**
   - Source: a trusted source checkout or an already-extracted release package. Set `ARTIFACT_ROOT` to its root. The guide never downloads, names an archive, or claims an asset exists.
   - Shape check: `skill/repo-harness-builder/` and `command/repo-harness-builder.md` must exist under `ARTIFACT_ROOT`; otherwise stop as an invalid artifact.
   - Prerequisites: Python 3 and the selected runtime already installed.
   - The design's destination table, then one copy-paste POSIX shell block per runtime (Git Bash on Windows), parameterized only by `ARTIFACT_ROOT` and the target repository root. Each block refuses when either destination already exists and never overwrites.
   - Invoke `repo-harness-builder` in the target repository; link [Builder And Generated Skills](../../README.md#builder-and-generated-skills) for the audit, preview, and approval flow.
   - After apply, run `python3 .repo-harness/check.py` (`python` on Windows); only exit 0 with `current` and zero writes means congruence. A runtime may need a fresh process to register new skills.
   - Daily use: link [Daily Workflow](usage.md#daily-workflow).
   - One short list of failure stops: invalid artifact, occupied destination, unregistered runtime, builder refusal, checker drift. Offline works once the artifact and prerequisites are local.
3. **Verify for real, inside the repository.** Run each runtime block once with `ARTIFACT_ROOT` set to this repository and a scratch target under ignored `.repo-harness/tmp/`; confirm the expected files land, run it again to confirm the occupied-destination refusal, then delete the scratch target. Confirm EN/ES headings and links resolve, `git diff --check` is clean, and `python .repo-harness/check.py` reports `current` with zero writes.
4. **Deliver and close.** Use one issue and one PR. After merge, record `completed` in the roadmap and current state (EN/ES), the documentation maps, `CHANGELOG.md`/`CHANGELOG.es.md`, and Engram topic `work/RH-W-0055`, using observed evidence only. Rollback is reverting the single documentation commit.

**Non-goals:** installer scripts, package-manager automation, PowerShell variants, downloads, URLs, archive naming, publication, new commands or state, changes to builder behavior, and any implementation before separate authorization.
