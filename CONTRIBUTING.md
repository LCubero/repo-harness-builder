# Contributing to repo-harness-builder

Contributions start with an issue and end with maintainer review. Keep each change small, verifiable, and linked to the problem it solves.

## Quick Path

1. Open or choose a [GitHub issue](https://github.com/LCubero/repo-harness-builder/issues). Agree on scope with a maintainer before implementation.
2. Create a short, focused branch such as `docs/contribution-workflow` or `fix/projection-drift`.
3. Change canonical sources first. For product behavior, edit `skill/repo-harness-builder/` or `command/repo-harness-builder.md`; treat `.claude/` and `.opencode/` as runtime projections.
4. Keep implementation, tests, and nearby documentation in the same work unit.
5. Run the relevant checks and inspect the complete diff.
6. Open a focused PR that links the issue with `Closes #<number>` and states scope, verification, and risks.
7. Address review and obtain maintainer approval before merge.

Do not commit directly to `main`. A maintainer may explicitly authorize an emergency exception.

## Development Rules

- Use Python 3.10+ and the standard library. Do not add dependencies without an agreed issue and clear need.
- Follow [REPO-HARNESS.md](REPO-HARNESS.md). Repository evidence overrides Engram or Gentle AI continuity when they differ.
- Keep text files at canonical LF line endings on Linux, macOS, and Windows. The repository `.gitattributes` policy enforces this independently of local `core.autocrlf` settings.
- Preserve `.codegraph/` as ignored tool state. Never include it in a commit or PR.
- Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/), for example `docs: add contribution workflow` or `fix(checker): detect stale projection`.
- Commit by deliverable work unit, not by file type. Tests and docs belong with the behavior they verify or explain.
- Keep PRs easy to review. Around 400 changed lines is a useful target, not an automated gate in this repository. Split work when one reviewer cannot understand it comfortably.

## Verification

Run the full standard-library suite for product or projection changes:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

Run the read-only harness check for documentation, canonical-source, or projection changes:

```powershell
python .repo-harness/check.py
```

The checker must report `current`, exit 0, and report 0 writes. If canonical product sources changed, confirm the corresponding `.claude/` and `.opencode/` projections contain the same intended bytes. Documentation-only changes that do not alter generated contracts must not rewrite those projections.

## PR Checklist

- [ ] An issue exists and the PR links it.
- [ ] The branch and PR contain one focused work unit.
- [ ] Commits follow Conventional Commits.
- [ ] Tests and documentation are included where relevant.
- [ ] Exact verification commands and results are in the PR.
- [ ] The diff contains no secrets, unrelated files, or `.codegraph/`.
- [ ] A maintainer reviewed and approved the change before merge.

This repository currently has no contribution templates, custom workflow labels, or automated approval check. Use the issue and PR conversation as the source of approval.
