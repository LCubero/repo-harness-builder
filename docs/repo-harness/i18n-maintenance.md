# Bilingual Documentation Maintenance

`REPO-HARNESS.md`, `CHANGELOG.md`, and the English durable documents own operational facts. Spanish peers are complete localized presentations.

## Canonical Ownership
<!-- doc-role:canonical-ownership -->

Update factual owners first. If localized wording conflicts with a canonical fact, the canonical owner wins and the translation must be repaired.

## Translation Sync
<!-- doc-role:translation-sync -->

Update each affected Spanish peer in the same work unit. Equivalent meaning is required, not literal heading or sentence equality. Natural locale-specific phrasing is allowed when facts, constraints, commands, paths, and outcomes remain equivalent.

## Entry Mapping
<!-- doc-role:entry-mapping -->

In a list a reader counts, such as a dated `CHANGELOG.md` section, the Spanish peer maps one entry to one entry. A reader comparing the two must not have to guess whether a missing entry is a translation choice or a lost fact.

One exception exists, and it must be declared where it applies rather than inferred. A historical section may consolidate several English entries into fewer thematic Spanish entries only when every fact survives in some entry and the section itself says it is consolidated. An undeclared count difference is a defect, not a style choice. New sections are always one to one.

## Links
<!-- doc-role:link-validation -->

Keep exactly one reciprocal selector in each root README. Resolve and validate relative links from the containing file. Run the canonical tests and `python3 .repo-harness/check.py` (`python` on Windows) after documentation changes.

## Freshness
<!-- doc-role:freshness -->

`CHANGELOG.md` is the only chronological owner. `docs/repo-harness/current-state.md` is the only current-state owner. Spanish peers present those facts and identify their owner, preventing contradictory dates or status.
