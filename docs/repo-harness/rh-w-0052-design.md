# RH-W-0052 Minimal Archive-Only Design

> **Design status: approved; archive-only slice complete, verified, and delivered through issue #177 and PR #178. Unit lifecycle: `deferred`.** The [roadmap record](roadmap.md#rh-w-0052) owns the owner-delegated residual deferral and evidence-based re-entry condition. Uninstall, reinstall, and deletion remain undesigned; future design and implementation require separate authority. The historical design body below grants no new authority.

## Review Contract

Review against the canonical [roadmap record](roadmap.md#rh-w-0052), [standing decisions](decisions.md), the completed [RH-W-0051 taxonomy](rh-w-0051-design.md), and the [Spanish peer](../es/repo-harness/rh-w-0052-design.md). English is canonical. Repository evidence owns current behavior.

## Scope

RH-W-0052 defines only a future explicit archive copy into the existing non-authoritative, Git-ignored `.repo-harness/archive/` namespace. An archive preserves a selected source and creates one new archive copy; it never removes, moves, overwrites, normalizes, reinstalls, or repairs anything.

This authorized design covers only the minimal archive-copy slice. RH-W-0052 continues to own the future lifecycle behavior for archive, uninstall, reinstall, and deletion; beyond this slice, those operations remain undesigned and unauthorized here.

The failed VPS prototype is historical origin evidence only. It is neither a design input that proves safety nor shipped behavior. RH-W-0048 remains a completed, separate additive runtime-projection recovery unit; its authority, repair behavior, tests, and delivery evidence do not authorize this archive operation.

## Ownership And Preservation

- RH-W-0051 owns the archive namespace, containment, ignore behavior, and its non-authoritative status. RH-W-0052 owns only the future archive-copy decision boundary.
- Preserve every local-state path, including all contents under `.repo-harness/` other than the one new archive target created by an approved apply.
- Preserve every unknown, human-owned, existing, indirect, and tool-owned path. In particular, `.atl/`, `.codegraph/`, and `.engram/` remain outside harness ownership.
- Do not assume Git exists. When Git is absent, no Git-backed ownership or recovery claim is made.
- The archive namespace is recoverable state, not canonical truth, a manifest, a lifecycle store, an approval record, or evidence that the source may later be deleted.

## Archive Safety Boundary

A future archive candidate must be an explicitly named repository-relative, direct regular source within the selected repository root. The source cannot be inside `.repo-harness/archive/`, cannot traverse an indirect path, and cannot escape the root. No recursive discovery, inference from path names, traversal of unknown directories, or inspection of tool-owned paths selects a candidate.

The exact destination parent chain must already exist as direct real directories. A missing, indirect, or non-directory parent refuses without writes. Apply performs no `mkdir`, rollback deletion, or cleanup. This owner-selected minimal correction avoids the impossible combination of absent parents, a race collision, zero-write refusal, and no rollback deletion.

**Owner-delegated technical decision:** after instructing the agent to read and verify repository evidence and decide, the owner approved authenticated current-manifest ownership as the required archival eligibility rule. An archive candidate is eligible only when an authenticated current manifest proves that exact direct source is harness-owned. Missing, invalid, stale, or non-covering proof yields `ownership-unresolved` and preserves the source.

## Inspect, Preview, Approval, And Apply

| Stage | Required boundary | Prohibited outcome |
|---|---|---|
| Inspect | Read only the explicitly named candidate, applicable ownership evidence, root containment, source kind, destination availability, and direct real parent chain. Report `eligible`, `ownership-unresolved`, or a bounded refusal reason. | No directory creation, copy, move, deletion, manifest/checker change, Git action, or scan of unknown or tool-owned paths. |
| Preview | Produce one exact proposed source-to-new-archive-target mapping and bounded identity evidence; report `writes: 0`. A collision, missing/indirect/non-directory parent, unsafe path, missing ownership proof, or changed evidence yields no eligible action. | No implicit selection, overwrite, mutation, or authority from a prior preview. |
| Approval | Require a current explicit human approval of that exact preview. Material source, destination, ownership, or evidence change invalidates it. | No inherited session, Git, manifest, repair, or prior approval authority; no approval for deletion, reinstall, update, or delivery. |
| Apply | Reconfirm the approved mapping and safety facts immediately before creating only the new archive target. Success requires the source to remain intact and the new archive copy to be complete; otherwise report failure while preserving the source. | No source move, deletion, overwrite, `mkdir`, rollback deletion, cleanup architecture, Git initialization, staging, commit, push, network, or publication. |

These are design-level safety boundaries, not a command, schema, digest format, transaction algorithm, or implementation plan.

## Non-Goals

- Uninstall, reinstall, deletion, automatic cleanup, retention automation, migration, or deletion architecture.
- Product source, tests, runtime projections, skills, checker, manifest, or implementation changes.
- Archiving arbitrary, unknown, local-state, human-owned, or tool-owned paths.
- Any Git assumption, delivery, network action, commit, or publication.
- A database, ledger, cache, second manifest, scheduler, daemon, service, or generic archive subsystem.

## Future Acceptance Boundary

The separately authorized implementation fulfilled the bilingual plan: inspect and preview write nothing; apply creates only the approved new archive copy; sources, unknown paths, local state, and tool-owned paths remain unchanged; and destination parents must already be direct real directories. The plan retains the complete strict-TDD and verification receipts. No authority extends to uninstall, reinstall, deletion, cleanup, delivery, or lifecycle advancement.

## Authorization Boundary

The owner-approved rule leaves RH-W-0052 `PROPOSED`. The archive-only implementation is complete and verified under separate authority, with no remaining implementation work for that slice. Uninstall, reinstall, deletion, cleanup, lifecycle advancement, delivery, network access, commit, push, and publication remain separately undesigned or unauthorized.
