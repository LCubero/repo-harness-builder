# Harness Hardening Proposal

## Objective
Document the smallest owner-aligned proposal for the owner's cross-platform Git, generated-skill, deterministic-mechanics, and per-session backup notes without implementing product or runtime behavior.

## Problem And Why
The notes span existing completed and proposed owners. Treating them as one new implementation would duplicate lifecycle and create unnecessary coupling. The proposal must distinguish already-satisfied behavior, a new bounded Git guidance question, existing RH-W-0078 generated-skill work, and the now-resolved RH-W-0090 backup choice.

## Scope
- Verify the current `.gitattributes` cross-platform contract and avoid reopening it without a measured defect.
- Propose conditional Windows `core.longpaths` guidance without automatic global mutation.
- Draft the pending RH-W-0078 bilingual scope for generated-skill style and selective deterministic ownership in the existing Python renderer/checker, with no new compiled binary.
- Record the owner's RH-W-0090 decision to retain separate timestamped exclusive backups per session and reject one incremental shared file.
- Reconcile canonical English owners, complete Spanish peers, maps, current-state projection, and continuity only where lifecycle evidence changes.

## Constraints
- Proposal and documentation only; no product, test, generated contract, runtime projection, dependency, binary, Git configuration, consumer repository, commit, push, PR, release, or publication change.
- Preserve one canonical owner per concern; do not merge RH-W-0078, RH-W-0090, and the cross-platform Git proposal.
- Keep `session_id` as bounded technical metadata unless separate evidence establishes a defect; it is not authority and need not be user-visible.
- Do not add a global configuration mutation or claim that `core.longpaths=true` is universally required.

## Tasks
- [x] **HHP-T1 — Audit current owners and behavior.** Map `.gitattributes`, skill style, deterministic mechanics, binary, and backup evidence.
- [x] **HHP-T2 — Resolve overlaps and lifecycle placement.** Classify satisfied, retained, rejected, and genuinely new proposal surfaces.
- [x] **HHP-T3 — Draft bilingual owner-aligned proposal artifacts.** Updated the smallest canonical owners, declared peers, maps, chronology, and active projections for RH-W-0078, RH-W-0092, and RH-W-0090.
- [x] **HHP-T4 — Verify proposal consistency.** Diff hygiene, checker, six focused lifecycle/link tests, independent verification, and native review passed for the prior documentation candidate.
- [x] **HHP-T5 — Record RH-W-0078 scope approval.** Reconciled the corrected bilingual scope approval across lifecycle, current-state, chronology, and continuity owners while preserving separate design and implementation authorization.
- [x] **HHP-T6 — Record RH-W-0092 scope approval.** Reconciled the corrected bilingual scope approval while preserving separately authorized design, implementation, and delivery boundaries.
- [x] **HHP-T7 — Correct prior-session review and current-state reconciliation.** Preserved the owner-approved RH-W-0078 scope projection in both current-state peers and reconciled the prior-session review chronology without authorizing design or implementation.

## Acceptance Criteria And Checks
- Existing `.gitattributes` behavior is not presented as missing.
- Global Git configuration remains suggestion-only and operation-scoped.
- RH-W-0078 keeps progressive disclosure and deterministic Python ownership while rejecting a compiled binary.
- RH-W-0090 records separate per-session backups as the selected existing behavior and closes the shared-file alternative without implementation.
- English and Spanish owners remain semantically aligned.
- `python .repo-harness/check.py` reports `current` with zero writes.
- `git diff --check` passes for the complete candidate.

## Progress And Evidence
- Research clone `OthmanAdi/planning-with-files` remains ignored temporary evidence at commit `2fbbd77ba9a74cddb9504285935ef9ae0837cdec`.
- Current repository evidence already generates exact `text eol=lf` rules for byte-compared paths and tests `core.autocrlf=true` compatibility.
- `skill/repo-harness-builder/references/gentle-ai-style-guide.md` already bundles skill structure and size guidance.
- RH-W-0078 already rejects a new compiled binary and selects the existing Python renderer/checker as the deterministic machine boundary.
- RH-W-0069 already writes one timestamped exclusive backup per pause or close with bounded `session_id` metadata; RH-W-0090 owns the alternative shared-file comparison.

## Lifecycle Placement Decision
- `.gitattributes`: no new work item; the existing generated exact-LF contract and Windows `core.autocrlf=true` verification already satisfy the stated compatibility need.
- Windows long paths: new immutable RH-W-0092 proposal limited to conditional guidance and diagnostics; no automatic or implicit global Git mutation.
- Skill style and deterministic mechanics: advance existing RH-W-0078 to a bilingual scope proposal using its bundled style guide and existing Python renderer/checker; retain the compiled-binary rejection.
- Backup shape: close RH-W-0090 by owner decision against one incremental shared file; retain delivered RH-W-0069 timestamped exclusive per-session backups and bounded technical `session_id` metadata.

## Verification Evidence
- `git diff --check`: passed.
- `python .repo-harness/check.py`: `current`, zero writes.
- Focused `GeneratedCoreTests`: six lifecycle/link tests passed.
- Independent verification: no severe or medium findings after correcting the Spanish RH-W-0078 plan projection and Spanish style-guide link.
- Native review: historical proposal-review lineage `review-0deebb8572c733bd` was approved and acknowledged for the 15-path, 327-line documentation candidate; the prior session's final approval lineage `review-7b0f7ae498784096` covered its 15-path, 339-line candidate.

## Next Step
Both corrected bilingual scope approvals are recorded. RH-W-0078 and RH-W-0092 remain `proposed`; no implementation is authorized.
