# Bilingual Documentation Maintenance

`REPO-HARNESS.md`, `CHANGELOG.md`, and the English durable documents own operational facts. Spanish peers are complete localized presentations.

## Canonical Ownership

Update factual owners first. If localized wording conflicts with a canonical fact, the canonical owner wins and the translation must be repaired.

## Translation Sync

Update each affected Spanish peer in the same work unit. Equivalent meaning is required, not literal heading or sentence equality. Natural locale-specific phrasing is allowed when facts, constraints, commands, paths, and outcomes remain equivalent.

## Links

Keep exactly one reciprocal selector in each root README. Resolve and validate relative links from the containing file. Run the canonical tests and `python .repo-harness/check.py` after documentation changes.

## Freshness

`CHANGELOG.md` is the only chronological owner. `docs/repo-harness/current-state.md` is the only current-state owner. Spanish peers present those facts and identify their owner, preventing contradictory dates or status.
