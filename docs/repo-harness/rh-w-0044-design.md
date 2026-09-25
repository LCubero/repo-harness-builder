# RH-W-0044 Human-Owned Document Suggestion Design

> **Current status: owner-approved design; RH-W-0044 remains `proposed`. Four partial preview slices (rulebook core, bilingual usage overview, Visibility, EN/ES Translation Sync) are independently verified, locally committed and natively acknowledged; R44-1 remains open with conservative ambiguity pending, R44-2 read-only individual choice/marker preview locally implemented and independently verified in canonical script/test and six projections (588 tests/10 skipped, checker `current`/zero writes, byte parity, empty second preview), while R44-2 remains partial and open; R44-3 untouched, and R44-4's final whole-feature report pending. The earlier documentation commit is separate. No product delivery or complete implementation is claimed.**

## Decision

The builder may compare a bounded set of rendered human-owned contract documents with their current repository versions and report concrete, independent suggestions. Comparison and acceptance never transfer ownership to the builder: preview writes nothing, each suggestion is accepted separately, and an accepted suggestion must enter the existing content-bound plan with fresh source-byte and plan-digest checks before any write.

The owner approved this design after the technical approval decision. That approval does not authorize implementation planning or implementation. The canonical boundaries remain the [roadmap record](roadmap.md#rh-w-0044), [standing decisions](decisions.md), current [architecture](../../skill/repo-harness-builder/references/architecture.md), current [preservation rules](../../skill/repo-harness-builder/references/preservation-and-safety.md), and the [Spanish peer](../es/repo-harness/rh-w-0044-design.md).

## Compared Document Set

Comparison is limited to human-owned Markdown paths that the current renderer emits and whose rendered material defines reusable harness operation or navigation contracts:

- `REPO-HARNESS.md` and `REPO-HARNESS.es.md` when declared;
- `README.md` and `README.es.md` only for bounded harness installation, selector, and daily-use material the renderer can identify;
- `docs/repo-harness/README.md`, `usage.md`, and `i18n-maintenance.md`, plus declared Spanish peers;
- rendered files under `.repo-harness/rules/`.

The pass excludes `CHANGELOG.md`, `CHANGELOG.es.md`, `docs/repo-harness/current-state.md`, `roadmap.md`, and `decisions.md`, plus their localized peers. Their facts, lifecycle, chronology, and decisions have no generic rendered replacement. It also excludes arbitrary repository prose, product documentation, research, plans, designs, archives, session records, untracked files, ignored files, generated files already owned by the authenticated manifest, and directive-only `.gitignore`, `.gitattributes`, and root pointer handling.

An eligible document must be a direct, readable, repository-contained regular file. Missing, indirect, unreadable, malformed, or locale-incomplete paths keep their existing classifications and are not converted into content suggestions.

## Comparison Units

The renderer's complete current file is evidence, not a replacement candidate. Comparison operates only on stable rendered contract units identified by an existing `doc-role`, generated source role, exact bounded selector, or rule heading. Free-form paragraphs without a stable owner are not compared.

| Result | Meaning | Suggestion behavior |
|---|---|---|
| `satisfied` | The current document carries the rendered obligation, allowing project-specific surrounding prose. | No suggestion. |
| `missing-obligation` | A stable rendered obligation has no current counterpart. | Suggest an insertion at its owned role or anchor. |
| `outdated-obligation` | The current unit retains identifiable earlier harness wording whose required behavior conflicts with the current contract. | Suggest the smallest bounded replacement. |
| `ambiguous` | The unit cannot be matched without interpreting repository intent or claiming semantic equivalence. | Report the uncertainty; no proposed bytes. |
| `accepted-divergence` | A valid declaration intentionally excludes this exact contract unit from suggestions. | Report the declaration and suppress the suggestion. |

This is contract-presence comparison, not general prose rewriting, style correction, summarization, or semantic-equivalence scoring. It does not infer that different wording is wrong.

## Suggestion Shape And Presentation

Each suggestion contains a stable `suggestionId` for semantic identity; repository-relative `path`, locale, owner role or anchor, and classification; a concise reason and evidence; the exact current source SHA-256; the exact proposed UTF-8 insertion or replacement; a bounded current excerpt and proposed preview; and explicit choices `accept`, `keep`, or `declare-intentional-divergence`. A separately displayed `suggestionEvidenceSha256` content-binds the contract version, suggestion identity, locale, path, stable contract-unit ID, current source digest, action kind, and exact proposed bytes.

Suggestions are sorted by path and contract-unit order and remain outside the executable plan until accepted. Preview reports `writes: 0`. No suggestion is represented as mandatory, semantically equivalent, or already approved. A path with multiple findings presents one suggestion at a time; resolving one requires a fresh comparison before the next is accepted.

## Individual Acceptance And Write Authority

`accept` authorizes only the displayed suggestion and must supply its displayed `suggestionEvidenceSha256`. The builder re-reads the exact source bytes, rerenders the exact proposal, recomputes the evidence digest, and verifies equality before constructing the existing content-bound plan. It then places one bounded human-document integration action into that plan. The action binds path, source digest, destination digest, action kind, exact bytes, suggestion identity, evidence digest, and plan digest. Existing builder activation, approval, containment, atomic write, rollback, and post-apply verification rules remain unchanged.

Any source change, moved anchor, changed proposed bytes, locale mismatch, indirect path, or additional suggestion invalidates that acceptance. The builder pauses with fresh evidence instead of rebasing prose automatically. There is no `accept-all`, document-wide approval, approval reuse, automatic archive, or direct write from the comparison report.

`keep` preserves every byte and suppresses nothing beyond the current preview. It is not a durable declaration and creates no state.

## Intentional Divergence

Durable repository intent is declared in the human-owned document beside the affected contract unit with one exact comment:

```text
<!-- repo-harness:accepted-divergence <contract-unit-id> -->
```

The declaration suppresses only that stable contract unit in that document and locale. It does not suppress structural roles, locale checks, directives, unrelated suggestions, safety requirements, or generated-file drift. Unknown IDs, duplicates, declarations outside an eligible owner unit, or declarations for obligations that the current builder marks non-suppressible are reported as `ambiguous` and authorize nothing.

Creating the declaration is itself a separately displayed suggestion requiring individual acceptance and the same content-bound plan. The marker records repository intent, not semantic equivalence or permanent correctness. If the current renderer retires the unit ID, changes its safety class, or the declaration no longer sits in the declared owner unit, the builder reports it for human review rather than silently preserving or deleting it.

Non-suppressible units are the selected repository boundary, explicit mutation and publication approval boundaries, generated ownership and byte-preservation rules, and the visible source-of-truth routing needed to locate those owners. A repository may rewrite those obligations in its own words; it may not use suggestion suppression to make their absence appear satisfied.

## Localization And Existing Boundaries

Only declared locales participate. Canonical and localized documents receive independent suggestion IDs scoped by locale, but a contract change affecting both is presented as two separate acceptances. Accepting one never implies accepting its peer. Existing localization checks continue separately.

RH-W-0022 retains declared cross-document relationships. RH-W-0066 may route a future explicit remember request to an existing owner but does not create or accept RH-W-0044 suggestions. RH-W-0045 retains surviving legacy-reference detection. None is merged into this unit.

## Acceptance Criteria

- Preview writes nothing and suggests only eligible rendered contract units in the exact bounded document set.
- Current generated-file, structural, locale, directive, and legacy-reference classifications remain unchanged.
- Project facts in changelog, current state, roadmap, and decisions never receive generic content suggestions.
- Every suggestion includes stable identity, exact path and source digest, reason, bounded evidence, exact proposed bytes, and three explicit choices.
- Every selection supplies the separately displayed evidence digest that binds contract version, identity, locale/path/unit, current source digest, action kind, and exact proposed bytes; it is verified before plan construction.
- Acceptance is one suggestion at a time and becomes executable only through a fresh digest-bound integration plan.
- Changed source or proposal evidence invalidates acceptance before writes.
- `keep` is ephemeral; durable suppression requires one valid, narrow, individually accepted declaration.
- Intentional divergence never suppresses non-suppressible authority, containment, preservation, or owner-routing obligations.
- English and Spanish peers remain independent human decisions.
- No result claims semantic equivalence or builder ownership of human prose.

## Required RED Tests For Future Implementation

- An evolved eligible rulebook with one identifiable missing obligation receives one exact suggestion and zero writes.
- A current obligation expressed with valid project-specific surrounding prose receives no whole-section replacement.
- Changelog, current state, roadmap, decisions, arbitrary product prose, and ignored files receive no suggestions.
- Two suggestions cannot be accepted together, and accepting the first forces fresh comparison before the second.
- A source-byte change after selection rejects before plan construction.
- A renderer or exact proposed-byte change after selection rejects before plan construction.
- Unavailable Git tracked-file evidence fails closed as unavailable/ambiguous with zero suggestions and zero writes; it never implies a clean tracked set.
- `keep` reappears on a later preview, while a valid accepted-divergence marker suppresses only its exact unit.
- An unknown, misplaced, duplicate, or non-suppressible divergence marker suppresses nothing and reports bounded ambiguity.
- Accepting an English suggestion does not accept or mutate its Spanish peer.
- A later failure in a mixed transaction containing the accepted human action plus existing create, append, and replace actions restores all pre-existing destinations byte-for-byte and removes every newly created destination.
- Existing human-owned bytes remain unchanged when preview, comparison, or validation fails.

## Non-Goals

- No implementation, lifecycle advancement, staging, commit, push, PR, release, or consumer-repository mutation.
- No whole-document regeneration, bulk acceptance, automatic merge, semantic model, LLM judge, prose scoring, daemon, database, ledger, or new external dependency.
- No new lifecycle, decision, chronology, research, or memory owner.
- No replacement of generated-file update, directive integration, localization validation, RH-W-0045 scanning, RH-W-0022 relationships, or RH-W-0066 routing.

## Lifecycle And Human Decision

The following design-approval account records the historical boundary before implementation authorization. Current partial progress is stated in the status notice above.

RH-W-0044 remains `proposed`. The owner approved the compared set, contract-unit boundary, suggestion schema, one-at-a-time acceptance flow, and narrow intentional-divergence declaration, then accepted the corrected bilingual [implementation plan](rh-w-0044-implementation-plan.md) after focused review on a local, not-yet-delivered branch. Implementation was the next separately authorized phase and had not yet been authorized at that time.
