# Contribution Workflow Research

## Recommendation

Adopt a short issue-first contribution guide with focused branches, linked PRs, Conventional Commits, work-unit tests and docs, and maintainer review before merge. Keep approval conversational until repository automation actually exists.

## Research Question

Which contribution practices improve safety and reviewability for this stdlib Python, self-hosting builder without claiming labels, templates, or checks the repository does not have?

## Primary Sources

- The CONTRIBUTING of a private repository, `repository 1`: concise quick path, flexible issue-first workflow, short branches, Conventional Commits, verification commands, secret review, nearby docs, and a 400-line review target.
- The CONTRIBUTING of a private consumer repository, `consumer 2`: operational integration with REPO-HARNESS and Engram plus clear branch, PR, and approval boundaries.
- [Gentle AI CONTRIBUTING](https://github.com/Gentleman-Programming/gentle-ai/blob/main/CONTRIBUTING.md) (unpinned legacy citation, read on or before 2026-08-13; exact revision not recorded): strict issue-first delivery, branch and commit conventions, work-unit commits, review budget, and automated PR policy.
- [Gentle AI cognitive-doc-design](https://github.com/Gentleman-Programming/gentle-ai/tree/main/skills/cognitive-doc-design) (unpinned legacy citation, read on or before 2026-08-13; exact revision not recorded): answer-first structure, progressive disclosure, chunking, recognition over recall, and reviewer empathy.

## Findings

- A quick path plus final checklist makes the workflow usable without requiring contributors to memorize policy.
- Issue-first work exposes scope before implementation; a linked PR preserves the decision trail.
- Small branches and deliverable work units reduce rollback and review cost. Tests and docs should travel with the behavior, not in later file-type commits.
- Conventional Commits provide enough shared vocabulary without adding tooling.
- A 400-line target is useful review guidance, but this repository has no automated size gate or exception label.
- The repository currently has only GitHub's standard labels and no issue templates, PR template, `status:approved`, or contribution CI checks. Maintainer approval must therefore remain documented and conversational.

## Repository Adaptation

The guide names Python 3.10+ stdlib development, canonical product sources in `skill/` and `command/`, runtime projections in `.claude/` and `.opencode/`, `python .repo-harness/check.py`, Engram and Gentle AI continuity, and untracked `.codegraph/`. It forbids direct work on `main` except an explicit maintainer emergency.

## Tradeoffs

`repository 1` offers the best cognitive size but needs repository-specific source and projection rules. `consumer 2` provides useful continuity detail but is too long and repository-specific to copy. Gentle AI supplies a strong conceptual model, but copying its labels, regex enforcement, templates, or CI checks would create false promises here.

## Future Product Boundary

The builder should eventually be able to propose, create, and manage a contribution harness for consumer repositories. It must first discover existing policy and real repository capabilities, then propose `CONTRIBUTING`, issue-first rules, branch and PR guidance, and only those templates, labels, or checks that exist or receive explicit approval. This research does not implement that product capability.

## Confidence And Gaps

Confidence is high for the documented local workflow because repository files, labels, source ownership, and verification commands were checked directly. Future consumer automation remains undefined and requires product design plus capability discovery.
