# Adaptive Harness Roadmap

This roadmap orders the smallest reviewable work units needed to make `repo-harness-builder` adaptive without committing to an integration architecture prematurely. Each unit proceeds through its own issue, branch, PR, tests, review, and documentation update.

## Outcome And Principles

- Reconcile repository evidence before changing plans. No pending item is added, retained, or removed from one source alone.
- Keep one clear factual owner, use progressive disclosure, and prefer short independently reviewable units.
- Update current state and every affected localized mirror in the same work unit that changes reality.
- AI-facing guidance reads canonical English only to avoid duplicate context. Localized docs are complete human presentations maintained by the documentation skill for user-declared repository languages.
- Reply in the user's stated language, otherwise in the language they are using.
- Adapt to repository evidence, ask only on genuine decisions, and warn on contradictions instead of rigidly imposing structure.
- Bound self-improvement to one verified friction and one minimal reviewable change at a time. Never recurse into improvement or silently change behavioral policy; existing repository approval rules remain authoritative.
- Avoid overengineering, scope drift, and known-disallowed actions.
- Use Engram when available for durable continuity, then revalidate it against current repository evidence.
- Put concise critical rules in generated `REPO-HARNESS.md`, operational detail in skills, continuity in Engram, and human explanations in docs.

## Authority And Placement

| Information | Owner |
|---|---|
| Current behavior | Source and tests, reconciled with owning docs and Git history |
| Current status and next step | [current-state.md](current-state.md) |
| Ordered future work | This roadmap |
| Standing rationale and constraints | [decisions.md](decisions.md) |
| Completed chronology | [CHANGELOG.md](../../CHANGELOG.md) |
| Critical generated operating rules | Generated `REPO-HARNESS.md` |
| Detailed operations | Generated and canonical skills |
| Session continuity | Engram, reconciled before promotion |
| Spanish human presentation | Complete mirrors under [`docs/es/`](../es/repo-harness/README.md), linked to English factual owners |

## Verified Baseline

- `python .repo-harness/check.py` reports `current`, exit 0, and 0 writes.
- PR #2 merged to `main` as `ec56b1b` and linked issue #1.
- Runtime-set widening is complete: `runtime_set_report()` reports `unrecorded` runtimes and `staleManifest`, with `RecordedRuntimeSetTests` coverage.
- Judgment Day round 2 remains incomplete on available evidence.
- Five round-1 findings remain open: legacy manifests can omit Spanish checks; `check` accepts but ignores language flags; invalid installed language declarations are not normalized when read; two documentation tests assume Spanish files; and promised-role to actual-heading compatibility lacks coverage.
- Approved replacement and integration execution, existing README selector integration, historical review migration, stale installed builder detection, and consumer contribution harness design remain open.
- Current tests and validation prohibit `.gitignore` management, so default `.codegraph/` ignoring requires an explicit policy change and its own TDD unit.

## Ordered Work Units

| # | Problem and evidence | Bounded outcome | Verification | Explicit non-goals |
|---|---|---|---|---|
| 1 | Pending facts can drift when one source is trusted alone. | Complete: defined the evidence reconciliation rule across owning docs, source, tests, history, Git, and Engram as applicable. | Contract tests and same-unit English and Spanish state updates. | No implementation backlog changes without reconciled evidence. |
| 2 | `.codegraph/` is currently untracked, while tests and validation prohibit `.gitignore` management. | TDD an explicit default-ignore policy with a persistent, checkable user opt-out. | Red-green tests for default, opt-out, idempotence, and preservation. | No silent `.gitignore` edit and no unrelated ignore policy. |
| 3 | Legacy manifests without language can omit Spanish checking. | Preserve and check known installed locale trees when the language field is absent. | Regression test from a legacy bilingual fixture. | No manifest migration framework. |
| 4 | `check` advertises language flags but ignores them. | Remove or honor the flags through one unambiguous read-only contract. | CLI behavior and help tests. | No broader CLI redesign. |
| 5 | Invalid installed language declarations fail without read-time normalization. | Normalize or reject them consistently with correct field-specific diagnostics. | Invalid manifest regression matrix. | No new locale support. |
| 6 | Two checked-in documentation tests assume Spanish files exist. | Make those tests respect declared documentation locales. | English-only and bilingual test runs. | No weakening of bilingual checks when Spanish is declared. |
| 7 | `missing_sections` lacks promised-role to actual-heading compatibility coverage. | Specify and test that direction, then make the smallest compatible correction. | Focused unit tests for both vocabulary directions. | No document parser rewrite. |
| 8 | Approved replacement and integration choices are descriptive, not executable. | Execute one content-bound approved action while preserving unknown bytes and fail-closed behavior. | TDD for approval digest, changed bytes, rollback boundary, and no-write preview. | No bulk upgrade engine. |
| 9 | Existing README language selectors still fail closed without an executor. | Add the bounded reciprocal selector case using the approved integration executor. | LF, CRLF, reciprocal, duplicate, and changed-plan tests. | No general README rewriting. |
| 10 | Consumer repositories cannot distinguish a stale installed builder from generated harness drift. | Define and detect builder package freshness separately. | Fresh, stale, absent, and unverifiable fixtures. | No runtime reload promise. |
| 11 | The review report from `skills-pack-ld9` is not durably carried over. | Reconcile and migrate still-relevant findings with public repository evidence. | Trace each retained or closed finding to current evidence. | No blind import of private or stale notes. |
| 12 | External research needs a safe, reproducible boundary. | Document primary-source-first research; allow temporary clones only in an ignored area after license and revision review, without execution. Public docs use independent writing and public citations, and the user decides clone retention. | Link checks, citation review, clone-boundary tests where enforceable. | No published dependency on private, ignored, or local paths. |
| 13 | Always-loaded contracts can grow without measured limits. | Add measured budgets, warnings, indices, references, and progressive disclosure for `REPO-HARNESS.md`, skills, and operational contracts, with small justified exceptions when splitting costs more. General human docs use an index and selective reading, not a universal cap. | Boundary tests and documented measurements. | No arbitrary cap on all human documentation. |
| 14 | Contribution support for consumers lacks a capability-aware design. | Design discovery of repository policy and real provider capabilities before proposing `CONTRIBUTING`, issues, branches, PRs, templates, labels, or checks. | Reviewable design scenarios and provider-neutral acceptance criteria. | No provider mutation or implementation. |
| 15 | MCP, CLI, binary, and third-party integrations may become useful later. | Reassess them only after prior units provide evidence and stable boundaries. | Decision record comparing measured needs and tradeoffs. | No current architecture commitment. |

## Decision Log

- Avoided assuming that Engram, one document, one test, or one historical report can establish current truth alone.
- Avoided treating `.codegraph/` ignore behavior as a documentation-only correction because it contradicts current executable policy.
- Avoided choosing MCP, CLI, binary, or third-party integration architecture before grounded consumer evidence exists.
- Unresolved: exact `.codegraph/` opt-out representation, legacy locale inference rule, `check` flag disposition, approved integration action schema, context budget thresholds, and future integration form.
