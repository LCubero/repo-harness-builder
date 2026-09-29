# RH-W-0022 Declared Documentation Relationships Design

> **Status: OWNER-APPROVED DESIGN. RH-W-0022 remains proposed; a bilingual implementation plan was drafted under later planning-only authorization and is now owner-approved. Implementation remains unauthorized.**

## Decision

Add one small, repository-local declaration of exact documentation edges and let the existing read-only checker validate only the contract named by each edge. The declaration is explicit: links, filenames, directories, and code symbols never create edges.

The single owner is `.repo-harness/document-relationships.json`. It is a human-owned technical contract, seeded only when absent and preserved byte for byte afterward. It does not become a documentation, lifecycle, chronology, decision, or research owner.

## Minimal Edge Format

The UTF-8 JSON document uses one version and an ordered edge list:

```json
{
  "version": 1,
  "edges": [
    {
      "source": "docs/repo-harness/roadmap.md",
      "relation": "localized-peer",
      "target": "docs/es/repo-harness/roadmap.md"
    }
  ]
}
```

Each edge has exactly three strings: repository-relative `source`, closed `relation`, and repository-relative `target`. Paths must be distinct, normalized, contained regular-file paths. Unknown fields, duplicate edges, unknown relations, absolute paths, traversal, links, missing endpoints, or malformed UTF-8/JSON make that edge unsatisfied. Version 1 has only `localized-peer` and `lifecycle-projection`.

No digest, timestamp, status, locale, evidence, note, wildcard, directory, transitive edge, or inferred reverse edge is stored. List order is presentation order; it grants no priority or execution order.

## Initial Declared Edges

The seed contains only relationships already owned by repository policy:

- `REPO-HARNESS.md`, `README.md`, `CONTRIBUTING.md`, and `CHANGELOG.md` to their exact Spanish peers;
- `docs/repo-harness/README.md`, `usage.md`, `current-state.md`, `roadmap.md`, `decisions.md`, and `i18n-maintenance.md` to their exact `docs/es/repo-harness/` peers;
- `docs/repo-harness/roadmap.md` to `docs/repo-harness/current-state.md` as `lifecycle-projection`, and the equivalent Spanish edge.

Designs, plans, research, archives, runtime projections, generated skills, and arbitrary Markdown remain undeclared. Add an exact edge only when an existing canonical owner says the relationship is required; discovery does not add one.

## Verification Semantics

For `localized-peer`, the checker verifies both endpoints are direct contained UTF-8 Markdown files, the target identifies the exact source as its canonical owner, and the existing locale and semantic-role checks pass. It does not claim translation equivalence, rewrite prose, compare whole-file bytes, or require both files to be edited in one Git diff.

For `lifecycle-projection`, the checker reuses the roadmap lifecycle parser and verifies every lifecycle status and derived portfolio fact asserted by the target against the source. It does not require the projection to repeat every roadmap record or infer a relationship for any other document.

The declaration and every endpoint are read only. A failed check preserves all human-owned bytes. A valid changed source may therefore remain satisfied when its target contract is still true; the edge is an obligation, not a mechanical same-commit rule.

## Exact Checker Result

Each unsatisfied edge adds one entry to the existing `files` array, in declaration order:

```json
{
  "path": ".repo-harness/document-relationships.json",
  "state": "relationship-unsatisfied",
  "source": "docs/repo-harness/roadmap.md",
  "relation": "lifecycle-projection",
  "target": "docs/repo-harness/current-state.md",
  "reason": "status-mismatch"
}
```

`reason` is one closed token: `invalid-declaration`, `missing-endpoint`, `unsafe-endpoint`, `canonical-owner-mismatch`, `locale-contract-failed`, or `status-mismatch`. The overall `status` is `drift`, `writes` remains `0`, and the CLI exits `1`, using the checker's existing JSON serialization. Satisfied edges add no file entry. Multiple failures produce one entry per edge without embedding document content.

## Localization And Ownership

The JSON owner is machine-significant and remains English-only. This design and its Spanish peer explain it to humans. English documents remain canonical factual owners; Spanish peers remain complete human presentations. The checker reports drift and never repairs, translates, stages, or proposes replacement bytes.

RH-W-0044 may later suggest an individually approved human-document change after this checker reports a problem; it does not own or infer edges. RH-W-0065 may later declare an exact relationship for a separately approved reference owner, but owns source provenance and revalidation, not this map or checker cycle. Neither unit is merged with RH-W-0022.

## Required RED Tests For Future Implementation

- Absence of the seeded owner in a new bilingual render fails before implementation and passes after exact seeding; an existing owner is never overwritten.
- One malformed, duplicate, unsafe, or unknown edge yields the exact `relationship-unsatisfied` entry and exit `1` with zero writes.
- A Spanish peer naming the wrong canonical owner yields `canonical-owner-mismatch`; existing human bytes remain unchanged.
- Existing locale or semantic-role failure yields `locale-contract-failed` without claiming translation equivalence.
- A current-state status contradicting its roadmap record yields `status-mismatch`; the same fixture passes after only the projection is corrected.
- A source edit that preserves the declared contract remains satisfied, proving there is no Git-diff or timestamp rule.
- An undeclared Markdown link, matching filename, code symbol, or RH-W-0065 source creates no edge and no result.
- Canonical, OpenCode, Claude, and Pi checkers produce the same result using Python 3.10+ standard library only.

## Non-Goals

- The original design work did not include an implementation plan or authorize implementation, lifecycle advancement, staging, commit, push, pull request, release, or publication.
- No inference from links, names, directories, locale suffixes, or content similarity.
- No symbol graph, call-path analysis, dependency engine, transitive closure, daemon, scheduler, watcher, database, service, or external dependency.
- No translation scoring, semantic-equivalence claim, automatic repair, whole-document regeneration, or change to human-document ownership.
- No duplicate RH-W-0065 reference lifecycle, provenance, archival, or revalidation cycle.

## Approval Boundary

The owner directly reviewed and technically approved this design on 2026-09-06. Later planning-only authorization produced a proposed bilingual [implementation plan](rh-w-0022-implementation-plan.md), now owner-approved. RH-W-0022 remains `proposed`; implementation requires separate authorization.
