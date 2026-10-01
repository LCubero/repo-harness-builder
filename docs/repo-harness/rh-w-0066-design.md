# RH-W-0066 Remember And Note Routing Design

Status: minimal design decided under owner delegation (2026-10-01). The [roadmap](roadmap.md#rh-w-0066) owns lifecycle.

## Decision

Add one Honesty rule to generated `.repo-harness/rules/working-agreements.md`, next to the RH-W-0065 link rule:

> When asked to remember or note something, route it to its existing owner: a decision to `docs/repo-harness/decisions.md`, pending work to `docs/repo-harness/roadmap.md`, a working rule to this file, a personal preference to Engram when available. If it is already recorded, do not duplicate it; if the owner is unclear, ask once. Never create a generic notes file.

Standalone harnesses have no Engram, so their copy omits the personal-preference clause.

This is generated builder behavior, not a project-only convention: every harness receives these requests, and every named owner is a generated file. The registered remaining work is covered without new machinery: classification is the owner list, deduplication is "do not duplicate", conflict and unclear scope are "ask once", and privacy and the Engram boundary stay with the existing memory rule.

## Rejected

A notes file, schema, classifier, reinforcement counter, or separate conflict store: none solves a present problem. `.repo-harness/rules/memory.md` is the natural home but sits at its 700-token budget; the working agreements already own how work is done and have room.

## Implementation

1. RED: one test asserting the rule in the rendered working agreements.
2. GREEN: add the bullet to the template in `skill/repo-harness-builder/scripts/harness_tool.py` and to this repository's human-evolved copy.
3. Sync builder projections through maintenance preview and current-digest apply; run the full suite and the checker.
4. Deliver through one issue and one PR, then close the lifecycle.
