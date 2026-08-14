# Adaptive Harness Roadmap

<!-- doc-role:lifecycle-owner -->

This is the canonical lifecycle owner for every non-trivial pending item, wish, deferred capability, exclusion, rejection, and completed roadmap outcome. `docs/repo-harness/current-state.md` is only its concise active projection. Engram mirrors outcomes under `work/RH-W-####`; it never owns this backlog.

## Lifecycle Contract

<!-- doc-role:lifecycle-rules -->

- IDs are immutable and independent of presentation order, issue number, and priority.
- Priority is `P0`, `P1`, `P2`, or `P3` with a concise justification. Status is `proposed`, `ready`, `active`, `blocked`, `deferred`, `completed`, `excluded`, or `rejected`.
- `blocked` names the blocker. `deferred` names an observable advancement condition. Terminal records use remaining work `none`.
- `excluded` is outside current scope but reconsiderable. `rejected` is intentionally declined until a later explicit decision supersedes it.
- Reconcile this owner, current state, localized peers, and the small Engram observation in the same work unit when reality changes. Trivial observations need no record.

## Authority And Baseline

Source and tests own current behavior; this roadmap owns lifecycle; [current state](current-state.md) owns the concise active projection; [decisions](decisions.md) owns standing rationale; [CHANGELOG.md](../../CHANGELOG.md) owns chronology. Spanish peers are complete human mirrors.

Verified baseline retained from the prior roadmap: checker `current`, exit 0, 0 writes; PR #2 merged as `ec56b1b` for issue #1; runtime-set widening is complete; issue #8 completed exact `.codegraph/` ignore with persistent opt-out; Judgment Day round 2 remains incomplete after judge B hit a session limit; five round-1 findings and the later capabilities below were identified. Each unit remains bounded to its prior outcome, verification, and non-goal.

## Active Records

<!-- doc-role:active-records -->

### RH-W-0016
| Field | Value |
|---|---|
| Origin | issue #7; lifecycle-foundation request |
| Priority | P0: priority override because durable control must exist before more wishes accumulate |
| Dependencies | RH-W-0001 evidence-reconciliation rule |
| Status | completed |
| Completed work | Added immutable lifecycle records, concise current-state projection, bilingual ownership, same-work-unit contracts, and validation. |
| Remaining work | none |
| Additions | Added issue #7 ahead of old Roadmap unit 3 without renumbering any prior unit. |
| Exclusions | No database file, daemon, scheduler, issue sync, workflow engine, transition API, priority calculator, or trivial-observation mandate. |
| Deferrals | none |
| Rejections | Rejected treating Engram or current-state.md as backlog owner. |
| Evidence | Focused RED/GREEN lifecycle tests and full repository verification for branch `feat/pending-lifecycle`. |
| Decision history | 2026-08-13: approved design selected roadmap ownership and lightweight records; priority override placed this foundation before old unit 3. |
| Next advancement condition | Maintain this contract whenever any tracked reality changes; RH-W-0004 is the next product behavior. |

### RH-W-0017
| Field | Value |
|---|---|
| Origin | User-reported Spanish session-stop occurrence; canonical conflict at `skill/repo-harness-builder/scripts/harness_tool.py` in the commit-only question contract |
| Priority | P1: the defect violates the active conversation-language contract at a mandatory approval boundary, but does not cause data loss, unsafe mutation, or an unrecoverable workflow |
| Dependencies | none |
| Status | ready |
| Completed work | Recorded the observed language leak, reconciled it with the exact canonical contract, and made the correction scope durable. |
| Remaining work | Preserve the exact semantic approval scope, paths, and proposed commit message while localizing the human-facing commit-only sentence in the active conversation language. |
| Additions | Add focused coverage for a non-English active conversation at the commit-only approval boundary when implementation begins. |
| Exclusions | No correction in this work unit, no change to approval semantics, no broader session-stop rewrite, and no Git or provider mutation. |
| Deferrals | none |
| Rejections | Rejected keeping the exact English wording as mandatory when the surrounding contract requires the active conversation language. |
| Evidence | During an active Spanish conversation, session-stop ended with `May I stage exactly these paths and create this proposed commit: ...?`; the canonical contract hardcodes that English sentence while also requiring conversational language continuity. |
| Decision history | 2026-08-13: user reported the occurrence and requested durable stop continuity; classified P1 and ready because the cause and bounded correction are known and dependency-free. |
| Next advancement condition | Start with a failing session-stop contract test for a Spanish commit-only close, then implement localized human-facing wording without changing approval scope, paths, or commit message. |

### RH-W-0018
| Field | Value |
|---|---|
| Origin | User-requested unattended-mode idea for internal harness maintenance and documentation |
| Priority | P2: repeated low-risk approval questions add friction, but the current interactive contract remains safe and functional |
| Dependencies | Session-start consent design, repository-scoped preference storage design, and focused safety tests |
| Status | deferred |
| Completed work | Recorded one autonomy policy with two consent scopes: the normal choice lasts only for the current session, while an explicit remember option persists for that repository until the user changes it. |
| Remaining work | Design the exact session-start question, remembered-preference lifecycle, precedence rules, and tests that prove excluded actions still require interaction. |
| Additions | At each session start, ask whether unattended mode is wanted for internal harness maintenance and documentation. Default to the current interactive behavior. |
| Exclusions | Unattended mode never covers product code, Git, commit, push, pull requests, publication, secrets, providers, destructive actions, or ambiguous decisions. |
| Deferrals | Deferred until the preference lifecycle and safety boundary have an approved design and focused test plan. |
| Rejections | Rejected global or implicit autonomy, silent opt-in, and expansion beyond internal harness maintenance and documentation. |
| Evidence | Current session contracts are interactive; no approved design or tests yet establish a safe repository-persistent preference. |
| Decision history | 2026-08-14: recorded as a pending idea only, preserving the current interactive default and existing approval contract. |
| Next advancement condition | Advance after approving a design that defines both consent scopes and repository preference changes, with tests for the default and every excluded action class. |

### RH-W-0019
| Field | Value |
|---|---|
| Origin | User-reported prompt-cost observation on the generated runtime wrappers |
| Priority | P3: the cost is a few tokens per wrapper invocation and no behavior is wrong |
| Dependencies | none |
| Status | ready |
| Completed work | Located the emission at `skill/repo-harness-builder/scripts/harness_tool.py:1222-1230` and proved the marker is load-bearing for upgrade classification. |
| Remaining work | Move wrapper provenance out of the prompt-visible bytes and give `generated_version()` an equivalent evidence source in the same change. |
| Additions | none |
| Exclusions | No change to skill or documentation provenance, which carries real reader value and lives in files the prompt does not load on every invocation. |
| Deferrals | none |
| Rejections | Rejected deleting the comment on its own, because wrappers have no frontmatter and the `source:` line is their only version evidence. |
| Evidence | `wrapper()` emits `<!-- Generated by repo-harness-builder; source: wrapper/<runtime>:<version> -->`; `generated_version()` at `harness_tool.py:208-217` falls back to the `source: [^\n:]+:(\d+\.\d+)` regex when no frontmatter `version:` exists. |
| Decision history | 2026-08-14: user questioned the token cost; verification confirmed the cost is real and small, and that a blind deletion would silently degrade every wrapper to `legacy`. |
| Next advancement condition | Start with a failing test that classifies a wrapper's version from the replacement evidence source, then remove the prompt-visible line. |

### RH-W-0020
| Field | Value |
|---|---|
| Origin | User-reported defect: this repository's own product README describes behavior the builder no longer has |
| Priority | P1: the README is the primary product surface and currently promises output the builder does not produce |
| Dependencies | none |
| Status | completed |
| Completed work | Confirmed the code is correct and the drift is documentation-only, then rewrote the affected `README.md` and `README.es.md` sections: outputs now list only what an English-only harness produces, a new `Documentation Language` section states the declared-locale contract, the existing-repository section records that no locale is ever added, and the Spanish peer was made faithful to the English owner. |
| Remaining work | none |
| Additions | State the builder's essence plainly: English is the primary locale and is always complete, any other locale exists only when declared, and an existing repository is never given locales it does not already have. |
| Exclusions | No general README rewrite, no change to the generated README template, and no code change. |
| Deferrals | none |
| Rejections | Rejected keeping the bilingual wording as an aspiration; a product guide that describes unimplemented output is a defect, not a roadmap. |
| Evidence | `docs_outputs()` computes `multilingual = len(documentation_languages) > 1` at `harness_tool.py:777` and empties the README selector at `harness_tool.py:785-789`, while `README.md:30-31` and `README.es.md:27-29` promise `README.es.md` and `docs/es/repo-harness/` unconditionally, and `README.es.md:99-101` states the reciprocal-README rule as a general contract where `README.md:105-107` describes preservation and integration. |
| Decision history | 2026-08-14: classified as documentation drift left behind when documentation language became a declared choice, then corrected in the same session because the fix was documentation-only and needed no code. |
| Next advancement condition | Re-check both README files whenever locale behavior changes in `docs_outputs()` or the language interview. |

### RH-W-0021
| Field | Value |
|---|---|
| Origin | User decision on generated skill naming |
| Priority | P2: the current names are correct and unambiguous, so this is a naming contract change rather than a defect |
| Dependencies | none |
| Status | ready |
| Completed work | Confirmed the current contract is `{project_id}-<skill>`, that the observed name is not a regression, and that generated command names are already unprefixed. |
| Remaining work | Change generated skill names to the fixed `repo-harness-` prefix across canonical source, tests, this repository's own projections and manifest, and every English and Spanish document that names them. |
| Additions | Align skill names with the command layer, which already strips the project prefix. |
| Exclusions | No change to command names, wrapper contents, or the harness directory layout. |
| Deferrals | none |
| Rejections | Rejected blocking this change on cross-repository name collision. The skills are repository-scoped, live inside each repository, and no global installation exists. |
| Evidence | Names are built at `harness_tool.py:365, 466, 555, 1455-1457` and consumed at `harness_tool.py:657-658, 842-844, 1049-1051, 1161-1162`; `harness_tool.py:1475` derives command names with `removeprefix(f"{project_id}-")`, which already yields `session-start`, `session-stop`, and `documentation`. |
| Decision history | 2026-08-14: a hypothetical global-installation collision was raised, then withdrawn as speculative under the proportionality rule; the user confirmed the fixed prefix. |
| Next advancement condition | Start with failing tests asserting `repo-harness-session-start`, `repo-harness-session-stop`, and `repo-harness-documentation`, then rename in canonical source before regenerating this repository's projections and manifest. |

### RH-W-0003
| Field | Value |
|---|---|
| Origin | Roadmap unit 3; Judgment Day round-1 finding |
| Priority | P1: legacy bilingual harnesses can silently lose Spanish checking |
| Dependencies | RH-W-0016 lifecycle foundation |
| Status | completed |
| Completed work | Added canonical regression coverage and inferred only installed known locale trees for legacy manifests without `language`; explicit modern declarations remain authoritative. |
| Remaining work | none |
| Additions | Track the incomplete Judgment Day round 2 as evidence, not as permission to lose the finding. |
| Exclusions | No manifest migration framework. |
| Deferrals | none |
| Rejections | none |
| Evidence | Issue #11 on `fix/legacy-bilingual-manifest`; the focused regression failed because the missing Spanish path was absent from expected outputs, then passed with the minimal inference; the 197-test canonical suite passed with 6 skips. |
| Decision history | 2026-08-13: retained as the next product behavior after lifecycle control. 2026-08-14: completed with bounded legacy inference and no manifest migration or CLI redesign. |
| Next advancement condition | Maintain the regression while known installed locale trees or explicit language declarations evolve. |

### RH-W-0004
| Field | Value |
|---|---|
| Origin | Roadmap unit 4; Judgment Day round-1 finding |
| Priority | P1: advertised ignored flags create an ambiguous read-only contract |
| Dependencies | RH-W-0003 locale-tree behavior should establish the language-check contract first |
| Status | ready |
| Completed work | The ignored report, conversation, and documentation language flags are identified. |
| Remaining work | Remove or honor the flags through one unambiguous contract and test CLI behavior and help. |
| Additions | none |
| Exclusions | No broader CLI redesign. |
| Deferrals | none |
| Rejections | none |
| Evidence | Current `check` accepts three language flags without using them. |
| Decision history | 2026-08-13: ordered after legacy locale preservation. |
| Next advancement condition | Start with focused CLI tests that expose the ignored language flags. |

### RH-W-0005
| Field | Value |
|---|---|
| Origin | Roadmap unit 5; Judgment Day round-1 finding |
| Priority | P1: invalid installed declarations fail with misleading diagnostics |
| Dependencies | RH-W-0003 and RH-W-0004 language contract |
| Status | blocked |
| Completed work | Invalid read-time declarations and wrong-field diagnostics are identified. |
| Remaining work | Normalize or reject declarations consistently with field-specific diagnostics and a regression matrix. |
| Additions | none |
| Exclusions | No new locale support. |
| Deferrals | none |
| Rejections | none |
| Evidence | `declared_languages` passes invalid values through to a hard check failure. |
| Decision history | 2026-08-13: kept behind the language contract prerequisites. |
| Next advancement condition | Blocker clears after RH-W-0003 and RH-W-0004 settle discovery and CLI semantics. |

### RH-W-0006
| Field | Value |
|---|---|
| Origin | Roadmap unit 6; Judgment Day round-1 finding |
| Priority | P2: checked-in tests fail in valid monolingual layouts |
| Dependencies | RH-W-0003 locale discovery |
| Status | ready |
| Completed work | Two Spanish-assuming documentation tests are identified. |
| Remaining work | Make tests respect declared locales and verify English-only and bilingual runs. |
| Additions | none |
| Exclusions | No weakening of bilingual checks when Spanish is declared. |
| Deferrals | none |
| Rejections | none |
| Evidence | Existing tests error instead of skipping when Spanish is not declared. |
| Decision history | 2026-08-13: retained as a focused test-contract correction. |
| Next advancement condition | Start with focused English-only fixtures and preserve bilingual checks where Spanish is declared. |

### RH-W-0008
| Field | Value |
|---|---|
| Origin | Roadmap unit 8; approved replacement and integration gap |
| Priority | P1: approved plans remain descriptive and cannot safely execute |
| Dependencies | RH-W-0003 through RH-W-0007 language and section compatibility corrections |
| Status | blocked |
| Completed work | Content-bound approval, preservation, changed-plan, and rollback boundaries are specified. |
| Remaining work | Execute one approved action with digest, byte preservation, rollback boundary, and no-write preview tests. |
| Additions | none |
| Exclusions | No bulk upgrade engine. |
| Deferrals | none |
| Rejections | none |
| Evidence | Current planner describes replacement and integration choices without an executor. |
| Decision history | 2026-08-13: retained as the first mutation capability after prerequisite correctness. |
| Next advancement condition | Blocker clears when RH-W-0003 through RH-W-0007 are completed or explicitly shown irrelevant. |

### RH-W-0009
| Field | Value |
|---|---|
| Origin | Roadmap unit 9; existing README selector gap |
| Priority | P1: reciprocal localized documentation remains safely blocked |
| Dependencies | RH-W-0008 approved integration executor |
| Status | blocked |
| Completed work | One-sided selector plans fail closed and LF, CRLF, reciprocal, duplicate, and changed-plan cases are specified. |
| Remaining work | Add the bounded reciprocal selector action through the approved executor. |
| Additions | none |
| Exclusions | No general README rewriting. |
| Deferrals | none |
| Rejections | none |
| Evidence | Existing README selectors cannot be integrated safely today. |
| Decision history | 2026-08-13: prerequisite dependency on RH-W-0008 made explicit so the idea cannot disappear. |
| Next advancement condition | Blocker clears when RH-W-0008 provides the bounded executor. |

### RH-W-0010
| Field | Value |
|---|---|
| Origin | Roadmap unit 10; stale installed builder gap |
| Priority | P2: consumers cannot distinguish package staleness from projection drift |
| Dependencies | RH-W-0008 executable upgrade boundary |
| Status | blocked |
| Completed work | Fresh, stale, absent, and unverifiable fixture outcomes are specified. |
| Remaining work | Define and detect builder package freshness separately. |
| Additions | none |
| Exclusions | No runtime reload promise. |
| Deferrals | none |
| Rejections | none |
| Evidence | Generated harness checking does not establish installed builder package freshness. |
| Decision history | 2026-08-13: retained as a separate concept from runtime projection drift. |
| Next advancement condition | Blocker clears when RH-W-0008 defines safe package evolution boundaries. |

### RH-W-0011
| Field | Value |
|---|---|
| Origin | Roadmap unit 11; `skills-pack-ld9` historical review |
| Priority | P2: still-relevant review findings need public evidence or explicit closure |
| Dependencies | RH-W-0003 through RH-W-0010 outcomes may close or reshape findings |
| Status | deferred |
| Completed work | Migration must reconcile rather than blindly import private or stale notes. |
| Remaining work | Trace each retained or closed finding to current public repository evidence. |
| Additions | Include the incomplete Judgment Day round-2 receipt in the evidence review. |
| Exclusions | No blind import of private or stale notes. |
| Deferrals | Deferred until prerequisite product corrections reduce stale findings. |
| Rejections | none |
| Evidence | Historical report exists outside this repository; round 2 lacks judge B output. |
| Decision history | 2026-08-13: deferred with an observable evidence threshold. |
| Next advancement condition | Advance when RH-W-0003 through RH-W-0010 are resolved or the report contains a newly urgent independent finding. |

### RH-W-0012
| Field | Value |
|---|---|
| Origin | Roadmap unit 12; external research safety wish |
| Priority | P2: research needs reproducibility without importing unsafe execution |
| Dependencies | RH-W-0011 evidence migration informs what research is needed |
| Status | deferred |
| Completed work | Primary-source-first, ignored temporary clone, license, revision, no-execution, citation, and retention boundaries are specified. |
| Remaining work | Document and test enforceable boundaries and validate links and citations. |
| Additions | none |
| Exclusions | No published dependency on private, ignored, or local paths. |
| Deferrals | Deferred until a concrete research need emerges from reconciled findings. |
| Rejections | none |
| Evidence | Existing roadmap design and preservation rules. |
| Decision history | 2026-08-13: retained as conditional capability, not immediate tooling. |
| Next advancement condition | Advance when RH-W-0011 identifies a question requiring external primary-source research. |

### RH-W-0013
| Field | Value |
|---|---|
| Origin | Roadmap unit 13; context-budget wish |
| Priority | P2: always-loaded contracts need measured growth limits |
| Dependencies | Measured contract growth from preceding units |
| Status | deferred |
| Completed work | Rulebook budget exists; indices, references, progressive disclosure, and justified exceptions are the approved direction. |
| Remaining work | Add measured skill and operational-contract budgets, warnings, and boundary tests. |
| Additions | none |
| Exclusions | No arbitrary cap on all human documentation. |
| Deferrals | Deferred until measurements show a boundary or review burden problem. |
| Rejections | none |
| Evidence | `REPO-HARNESS.md` already has a 700-token proxy budget; other contracts do not. |
| Decision history | 2026-08-13: measurement required before adding more limits. |
| Next advancement condition | Advance when measured always-loaded content approaches an agreed boundary or causes demonstrated context friction. |

### RH-W-0014
| Field | Value |
|---|---|
| Origin | Roadmap unit 14; consumer contribution capability wish |
| Priority | P3: useful future support lacks consumer and provider evidence |
| Dependencies | RH-W-0012 safe research and real consumer evidence |
| Status | deferred |
| Completed work | Capability-aware, provider-neutral discovery scenarios and acceptance criteria are scoped. |
| Remaining work | Design discovery before proposing contribution files, issues, branches, PRs, templates, labels, or checks. |
| Additions | none |
| Exclusions | No provider mutation or implementation. |
| Deferrals | Deferred until a consumer repository demonstrates the need and available provider capabilities. |
| Rejections | none |
| Evidence | Current repository has its own contribution workflow but no consumer capability discovery. |
| Decision history | 2026-08-13: design before implementation remains mandatory. |
| Next advancement condition | Advance with a real consumer case and verified repository/provider capabilities. |

### RH-W-0015
| Field | Value |
|---|---|
| Origin | Roadmap unit 15; MCP, CLI, binary, and third-party integration ideas |
| Priority | P3: architecture commitment without measured need would be overengineering |
| Dependencies | RH-W-0010, RH-W-0012, RH-W-0013, and RH-W-0014 evidence |
| Status | deferred |
| Completed work | Candidate integration forms and required tradeoff comparison are recorded. |
| Remaining work | Reassess measured needs and tradeoffs after stable boundaries exist. |
| Additions | none |
| Exclusions | No current architecture commitment. |
| Deferrals | Deferred until prior units provide measured consumer and operational evidence. |
| Rejections | none |
| Evidence | Current Python skill package and runtime projections solve the present use case. |
| Decision history | 2026-08-13: explicitly deferred rather than silently discarded. |
| Next advancement condition | Advance only when prior records demonstrate a need that the current package cannot meet cleanly. |

## Resolved Records

<!-- doc-role:resolved-records -->

### RH-W-0001
| Field | Value |
|---|---|
| Origin | Roadmap unit 1; evidence drift problem |
| Priority | P0: every later lifecycle decision depends on reconciled truth |
| Dependencies | none |
| Status | completed |
| Completed work | Defined reconciliation across owning docs, source, tests, history, Git, and Engram as applicable. |
| Remaining work | none |
| Additions | Added organic fact resolution without exhaustive reads or rigid questioning. |
| Exclusions | No implementation backlog changes without reconciled evidence. |
| Deferrals | none |
| Rejections | Rejected trusting one source alone. |
| Evidence | Generated contract tests and same-unit English and Spanish state updates. |
| Decision history | 2026-08-13: completed before subsequent roadmap work. |
| Next advancement condition | Reopen only if evidence reconciliation regresses. |

### RH-W-0002
| Field | Value |
|---|---|
| Origin | Roadmap unit 2; issue #8 |
| Priority | P1: untracked CodeGraph data conflicted with ignore policy |
| Dependencies | RH-W-0001 |
| Status | completed |
| Completed work | Added default exact `.codegraph/` ignore, persistent `--no-codegraph-ignore`, manifest checking, byte preservation, and changed-plan rejection. |
| Remaining work | none |
| Additions | Added `project.ignoreCodegraph` persistence. |
| Exclusions | No unrelated ignore policy and no writes inside `.codegraph/`. |
| Deferrals | none |
| Rejections | Rejected documentation-only treatment of executable policy. |
| Evidence | RED/GREEN tests for default, opt-out, idempotence, exact matching, bytes, and changed plans. |
| Decision history | 2026-08-13: completed under issue #8. |
| Next advancement condition | Reopen only if exact ignore or opt-out behavior regresses. |

### RH-W-0007
| Field | Value |
|---|---|
| Origin | Roadmap unit 7; Judgment Day round-1 finding |
| Priority | P2: section compatibility could falsely report older documents as incomplete |
| Dependencies | RH-W-0001 |
| Status | completed |
| Completed work | Made the promised document choose vocabulary and covered both roles/headings compatibility directions. |
| Remaining work | none |
| Additions | Added regression coverage for genuinely dropped sections. |
| Exclusions | No document parser rewrite. |
| Deferrals | none |
| Rejections | Rejected independent vocabulary extraction from both sides. |
| Evidence | `test_section_comparison_uses_one_vocabulary_for_both_sides` and dropped-section coverage. |
| Decision history | 2026-08-13: repository evidence superseded the stale open roadmap row. |
| Next advancement condition | Reopen only if compatibility regressions appear. |

## Decision Log

- Issue numbers remain origins only. They are never lifecycle IDs or presentation order.
- Existing Roadmap units retain IDs `RH-W-0001` through `RH-W-0015`; issue #7 is `RH-W-0016` and its P0 priority override does not renumber them.
- The user-reported session-stop language leak is `RH-W-0017`; it is ready independently and does not displace the already selected RH-W-0004 product unit.
- The unattended internal-maintenance idea is `RH-W-0018`; it remains deferred until its two consent scopes and safety tests are designed, and it does not change the interactive default.
- The wrapper provenance cost is `RH-W-0019`, the product README drift is `RH-W-0020`, and the fixed `repo-harness-` skill prefix is `RH-W-0021`. `RH-W-0020` was documentation-only and closed in the same session; the other two are ready and independent.
- A speculative cross-repository name collision was raised against `RH-W-0021` and withdrawn. Generated skills are repository-scoped and there is no global installation, so the risk was a future problem that is not real. Solve the problem in front of you.
- Avoided choosing MCP, CLI, binary, third-party, migration-framework, general README rewrite, bulk upgrade, provider mutation, runtime reload, or universal human-doc cap without evidence.
