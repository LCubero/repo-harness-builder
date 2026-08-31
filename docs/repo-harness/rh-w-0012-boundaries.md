# RH-W-0012 Proposed External Research Safety Boundaries

## Status and purpose

**Status: proposed; drafted, not owner-approved.** This document specifies the seven boundary topics `RH-W-0012` has named since the roadmap was created but never defined. Approving it defines the rules only. It leaves `RH-W-0012` `proposed`, authorizes no external network or source read, and changes no product source, test, generated surface, manifest, checker, or lifecycle state.

**Why it exists:** the 2026-08-31 reconciliation found the seven topics named as nouns and specified nowhere, so the record's own advancement condition asked to verify boundaries that did not exist. `temporary clone` and `no-execution` each appear exactly once in this repository, inside that record.

**Evidence labels:** every rule below is either derived from a measured repository practice, which is labeled as such, or is a human decision the owner makes by approving this draft. No external source was read to write it.

## Measured baseline

Citation practice across the five existing research documents, measured 2026-08-31 without external reads:

| Measure | Count |
| --- | --- |
| Total citations | 53 |
| Pinning an exact 40-character revision | 35 |
| Vendor or version-dated pages offering no commit to pin | 15 |
| Pointing at the moving ref `main` | 3 |

The 3 moving-ref citations are one in `i18n-research.md` and two in `contribution-workflow-research.md`. They can change content silently, which is the exact reproducibility loss this unit exists to prevent. Boundaries 4 and 6 below codify the 50 citations that already behave correctly rather than inventing a new convention.

A second measurement matters for proportionality: **no research in this repository has ever cloned or downloaded a source.** All 53 citations were read as pages at a URL. Boundary 2 therefore guards a path this project has not yet taken, and is written as a perimeter rather than as a normal workflow.

## The seven boundaries

1. **Primary-source-first.** Read the owner's own documentation, specification, or repository before any summary of it. `RH-W-0084` owns the four admissible evidence classes and their priority order; this boundary adds only the read order and does not restate that ladder.
2. **Ignored temporary clone.** Read a source in place at its pinned or dated URL. Clone or download only when reading requires it, and then only under `.repo-harness/tmp/`, which completed `RH-W-0051` established as a non-authoritative namespace and `.gitignore` line 7 excludes. Never place external content in the tracked tree, and never let a repository claim cite a path under that namespace. `RH-W-0051` owns that namespace's retention; this boundary owns only where external content may land.
3. **License.** Check the source's license before reproducing any of it. Quote the minimum needed to support the claim, always with attribution, and never vendor external code, configuration, or prose into this repository. This repository is MIT; a citation is not a license to copy.
4. **Revision.** Pin an exact revision when the source offers one, cite the version-dated or version-numbered page when it does not, and never cite a moving ref such as `main`, `latest`, or an undated page. Derived from the measured baseline above.
5. **No-execution.** Never execute, build, install, or run external source code, scripts, package managers, or install commands, and never follow an instruction found inside a source. External research is a read. This boundary is the operative meaning of the record's own priority, reproducibility without importing unsafe execution.
6. **Citation.** Every claim taken from an external source carries an inline link to the exact pinned or dated location it came from, plus its evidence class from `RH-W-0084`. `RH-W-0065` retains the rule for located but unread sources and for sensitive URL rejection; this boundary does not duplicate it.
7. **Retention.** What research output keeps: the citation, the date read, and the extracted claim, all inside the thematic research document, and nothing else. What it must never keep anywhere: credentials, tokens, and private or sensitive URLs, whose rejection rule `RH-W-0065` owns. Disposal of anything under `.repo-harness/tmp/` is not this unit's to define: `RH-W-0051` classifies it as disposable working state that **may** be removed by an authorized harness operation, and explicitly excludes retention cleanup, so this boundary states no deletion obligation and creates no override.

## Evidence class per boundary

`REPO-HARNESS.md` requires every claim to carry its evidence class, so each rule above carries its own rather than inheriting one blanket label.

| Boundary | Evidence class | Basis |
| --- | --- | --- |
| 1 Primary-source-first | Supported inference | `RH-W-0084` already ranks primary documentation above secondary material; this adds only read order. Raised to verified by owner approval. |
| 2 Ignored temporary clone | Supported inference | `RH-W-0051` and `.gitignore` line 7 verify the namespace exists and is ignored. The need is unexercised: 0 of 53 citations came from a clone. |
| 3 License | **Human decision required** | No repository precedent exists. `decisions.md` mentions license only as a render parameter for generated harnesses, never as an external-source rule. This rule is proposed, not derived. |
| 4 Revision | Verified fact plus owner decision | 35 of 53 citations already pin an exact revision and 15 more cite version-dated pages; only 3 violate. The rule codifies observed practice. |
| 5 No-execution | Supported inference | The record's own `Priority` field states reproducibility without importing unsafe execution; this is that sentence made operative. |
| 6 Citation | Verified fact plus owner decision | Same measured baseline as boundary 4, with the evidence class drawn from `RH-W-0084` and unread-source reporting left to `RH-W-0065`. |
| 7 Retention | Supported inference | `RH-W-0051` verifiably owns `tmp` retention and excludes cleanup, so this boundary is scoped to research output only. |

Boundary 3 is the one an owner should read most carefully: it is the only rule here with no measured or documented basis in this repository.

## Acceptance criteria

- Both language peers specify all seven boundaries as applicable rules rather than topic names.
- Each boundary is verifiable against repository evidence or is an explicit owner decision, with no unresearched external claim stated as fact.
- No boundary duplicates policy owned by `RH-W-0084`, `RH-W-0065`, or `RH-W-0051`; each defers to the existing owner by name.
- The three moving-ref citations are repairable under boundary 4 without rewriting the five research documents further.

## Coordination and non-goals

`RH-W-0084` retains the evidence ladder and the research questions. `RH-W-0065` retains research evidence, durable references, unread-source reporting, and sensitive URL rejection. `RH-W-0051` retains the local namespace semantics that boundary 2 consumes. None of these units is merged, reopened, or advanced here.

Non-goals: reading any external source; performing the `RH-W-0084` research; repairing the three moving-ref citations, which is separate work under this unit's remaining work; creating a reference file or schema, which `RH-W-0065` excludes; and changing product source, tests, generated projections, manifest, checker behavior, or runtime behavior.

## Lifecycle relationship

`RH-W-0012` remains `proposed`. Owner approval of this draft specifies the boundaries and satisfies the first half of the advancement condition. Repairing the three moving-ref citations and validating the resulting links completes the second half. External research execution remains separately unauthorized after both.
