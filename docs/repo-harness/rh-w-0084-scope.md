# RH-W-0084 Proposed Research Scope

## Status and purpose

**Lifecycle closure update, 2026-09-04: the owner accepted the reinforcement finding as a control under `RH-W-0083`, satisfying the roadmap advancement condition and completing `RH-W-0084`. The separately authorized `RH-W-0083` implementation is now completed. The research was executed on 2026-08-31 after `RH-W-0012` was reconciled and completed. The historical scope wording below is preserved as written.**

**Status: proposed; bilingual scope owner-approved.** This scope frames auditable, LLM-first research only. Scope approval leaves `RH-W-0084` `proposed` and authorizes no external network or source read, product implementation, product test, generated surface, manifest, checker, or lifecycle advancement.

**Evidence labels:** the LLM-first objective and this scope are human-direction artifacts. Any external guidance remains unknown until read under the approved research boundary; it is not stated here as fact.

## Exact research questions

1. **Format:** Which instruction formats are most reliably consumed and applied by the target LLM context: short prose, headings, lists, tables, fenced blocks, key-value forms, or another bounded form?
2. **Size:** What body, section, and loaded-context size ranges preserve correct retrieval and application without losing material instructions?
3. **Path and location:** Which canonical path, link placement, and load point make a required instruction reachable when the relevant action occurs?
4. **Reinforcement:** When should an instruction be repeated at the action point, and when is one canonical owner plus a reference sufficient?
5. **Forms and visibility:** Which visible forms, labels, delimiters, or response structures make a required action, warning, or question recognizable and usable?
6. **Control question:** Given a finding, when does it become a development-time test or control, generated-contract requirement, or documented guidance, and which existing owner receives it?

## Admissible evidence and source boundary

Admissible evidence classes, in priority order, are:

1. verified measurements and behavior from this repository;
2. primary documentation from the relevant model, runtime, or standards owner;
3. peer-reviewed or standards-body research with enough provenance to assess applicability; and
4. secondary material only as a lead or comparison, never alone as a repository fact or control target.

This scope authorizes **no read** from any external class. After owner approval, `RH-W-0012` must be re-evaluated against these questions before any external source is read. `RH-W-0065` retains the research-evidence and durable-reference boundary, including treatment of located but unread sources.

## From finding to repository control

1. Record each candidate finding in the future thematic research owner with its evidence label, source class, applicability limit, and unresolved uncertainty.
2. Compare it with repository measurements; a source claim alone is not a repository conclusion.
3. Route an accepted conclusion to one existing owner: a deterministic, repeatable requirement becomes a development-time test or control; exact agent-facing operational wording becomes a generated-contract requirement; non-deterministic explanation becomes documented guidance.
4. Obtain the separately required owner approval and implementation authority before changing any target. This scope creates no control or target change itself.

## Acceptance criteria

- Both language peers name the six questions, admissible source classes, evidence boundary, and routing rule above.
- The scope explicitly requires a new `RH-W-0012` evaluation after approval and before any external read.
- It names `RH-W-0065` as the evidence/reference boundary and preserves the distinct owners of informed surfaces.
- It neither asserts unresearched external guidance as fact nor authorizes research execution, implementation, or generated-surface change.

## Coordination and non-goals

Coordinate evidence and references with `RH-W-0065`; coordinate possible informed surfaces with `RH-W-0067`, `RH-W-0073`, `RH-W-0076`, `RH-W-0078`, and `RH-W-0083`. These units retain their own scope, lifecycle, and implementation boundaries; this research does not merge, reopen, or advance them.

Non-goals: selecting a target now; reading an external source; changing product source, tests, generated projections, manifest, checker behavior, network policy, or runtime behavior; creating a new reference or lifecycle store; and treating vendor or community advice as verified repository guidance without applicable evidence.

## Lifecycle relationship

`RH-W-0084` remains `proposed`. The owner-approved bilingual scope does not make the record `ready` or authorize implementation. `RH-W-0012` has been re-evaluated for the named questions; its safety unit must be reconciled before separately authorized external research. The thematic research document and every resulting control remain separately authorized work.

Both conditions were later satisfied on 2026-08-31: `RH-W-0012` was reconciled, its seven boundaries specified and accepted, and the owner then authorized research execution. Every resulting control still requires its own owner's separate authorization, and none has been applied.
