# RH-W-0065 Research Evidence And Links Design

Status: owner-approved minimal design (2026-09-30). The [roadmap](roadmap.md#rh-w-0065) owns lifecycle.

## Decision

Add two Honesty rules to generated `.repo-harness/rules/working-agreements.md`:

1. When research cites sources, list every source that was located but not read, with a short reason such as authentication, transport failure, incomplete content, or snippet-only evidence. Never present an unread source as evidence.
2. Keep a useful link in the document that owns its subject. Do not create a separate reference list.

No reference file, schema, or new owner is created. The existing rule file already owns how work is done; thematic research documents already own their sources.

## Rejected

The registered nine-field reference schema with deduplication, revalidation, archival, and localization machinery: it solves no present problem in a single-owner repository and adds a file nobody would maintain. Evaluating the owner-supplied external candidate repositories is removed from this unit; it needs separate external-access authority if ever wanted.

## Implementation

1. RED: one test asserting both rules in the rendered working agreements.
2. GREEN: add the two bullets to the template in `skill/repo-harness-builder/scripts/harness_tool.py`; add the same bullets to this repository's human-evolved copy, which maintenance does not replace.
3. Sync builder projections through maintenance preview and current-digest apply; run the full suite and the checker.
4. Deliver through one issue and one PR, then close the lifecycle.

The file stays inside its 700-token budget.
