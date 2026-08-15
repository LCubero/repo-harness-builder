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

### RH-W-0031
| Field | Value |
|---|---|
| Origin | A conversation-language audit found that generated session and help contracts can treat English artifacts as switch evidence and can emit fixed English approval wording into a Spanish conversation |
| Priority | P1: direct human approval and help replies can use the wrong language at active workflow boundaries, recurring beyond the bounded RH-W-0017 sentence correction |
| Dependencies | none |
| Status | ready |
| Completed work | Verified the root cause across `session_start_skill()`, `session_stop_skill()`, `help_wrapper()`, `render_with_skill_mapping()`, CLI wording, references, and prose-fragment tests; created the executable [implementation plan](rh-w-0031-implementation-plan.md). |
| Remaining work | Implement one shared conversation-language contract, remove fixed-language direct-reply literals, decouple help guide locale from reply language, align CLI fallback wording, add the behavioral TDD matrix, synchronize package projections, and apply generated consumer projections through the existing approved executor. |
| Additions | Define explicit human preference as strongest evidence, then established human conversation, a clear genuine-switch rule, artifact exclusions, manifest persistent default, and deterministic fallback last. Keep report, conversation, documentation, and artifact languages independent. |
| Exclusions | No natural-language detector, new locale, translation service, approval-scope change, strict builder-token change, network, external mutation, SDD, or implementation in this planning work unit. |
| Deferrals | none |
| Rejections | Rejected another piecemeal literal translation because RH-W-0017 proved that approach does not close the defect class. Rejected letting documentation locale own direct reply language. |
| Evidence | Canonical source embeds exact English fetch and existing-commit push questions, localizes only the commit question with one Python branch, tells help to reply in the guide language, defaults omitted conversation input to English, and has CLI help that says to ask while direct execution silently falls back. Existing tests mostly assert phrases and do not exercise precedence or artifact exclusions. `references/architecture.md` still says help follows report language, contradicting current code and validation. |
| Decision history | 2026-08-15: registered as the next immutable ID after confirming RH-W-0030 is the highest existing record. Kept as one unit because start, stop, help, fallback, and tests share one policy boundary and cannot be released safely with conflicting precedence. |
| Next advancement condition | Begin with the RED `ConversationLanguageBoundaryTests` matrix in the implementation plan, then change only canonical generator behavior and directly related references. |

### RH-W-0030
| Field | Value |
|---|---|
| Origin | The strict builder approval envelope was relayed during an ordinary session-start maintenance turn, and the user identified that the interface must not exist outside an active builder execution |
| Priority | P1: `RH-W-0028` bound the boundary in prose while the CLI kept manufacturing the interface, so the contract and the tool contradicted each other at a human approval boundary |
| Dependencies | RH-W-0028 activation boundary |
| Status | completed |
| Completed work | Added `render --activation builder\|maintenance`, defaulting to `maintenance`. Maintenance omits `approvalPrompt` and `approvalAnswers` from preview output entirely, rejects `--approval` with exit 2, and applies on the matching digest alone. Builder activation keeps the complete localized strict domain and the digest-plus-token requirement. The plan digest is identical under both activations. Synchronized the product contract, three references, this repository's bilingual usage and decisions, and both checked-in builder projections. |
| Remaining work | none |
| Additions | Two CLI regressions proving maintenance withholds the domain and applies on the digest alone, and that builder activation still enforces the exact token. |
| Exclusions | No change to digest computation, plan semantics, apply atomicity, rollback, generated harness content, session-skill consent, or the generated daily skills, which already contained no strict tokens. |
| Deferrals | none |
| Rejections | Rejected leaving the boundary as prose plus tests that only assert the prose, because a preview that always printed the envelope invited every caller to relay it. Rejected removing the token from builder apply, which would weaken the builder's own gate. |
| Evidence | `harness_tool.py` previously emitted `approvalPrompt` and `approvalAnswers` unconditionally on every read-only render. After the change, a maintenance preview of this repository yields zero strict-token matches and a builder-activation preview still yields them. The canonical suite passes 260 tests with 7 skips; both checkers report `current`. |
| Decision history | 2026-08-15: registered and completed after the leak recurred in practice; the fix moved the boundary from prose into the tool that manufactures the interface. |
| Next advancement condition | Reopen only if the strict domain appears in any output path outside `--activation builder`, or if builder activation loses the digest-plus-token requirement. |

### RH-W-0029
| Field | Value |
|---|---|
| Origin | Fresh `RH-W-0008` self-hosted plan reported 15 stale generated surfaces, of which byte-level comparison proved 12 differed only in line endings |
| Priority | P1: the checker reported drift no apply could durably resolve, which made real content drift indistinguishable from a Windows checkout artifact |
| Dependencies | RH-W-0008 approved replacement executor |
| Status | completed |
| Completed work | Derived root `.gitattributes` from the actual render outputs: one exact `<path> text eol=lf` rule for every byte-compared generated path, excluding pointer, ignore, attribute, and human-owned paths. Extended conflict detection to the same derived set and taught attribute matching to evaluate recursive `dir/**` patterns, which nested generated paths need. Applied the resulting plan to this repository. |
| Remaining work | none |
| Additions | Four regressions covering derived rule coverage per runtime set, runtime projection and wrapper protection, the exclusion of human-owned and pointer paths, and recursive pattern conflict detection. |
| Exclusions | No repository-wide line-ending policy, no `core.autocrlf` change, no normalization of unrelated paths, and no pinning of human-owned documents. |
| Deferrals | none |
| Rejections | Rejected broad `.claude/**` and `.opencode/**` rules, which would claim control over human files the harness does not own. Rejected applying the replacement alone, which writes LF that the next checkout would undo. |
| Evidence | `.gitattributes` grew from 2 to 19 protected paths. `git diff` after apply showed real content changes in exactly the three `repo-harness-session-stop` copies, confirming the other 12 were line endings only. Blob and worktree sizes differed by exactly the CRLF count before the fix. Both checkers now report `current` with 0 stale. |
| Decision history | 2026-08-15: the `RH-W-0008` plan surfaced 15 stale paths; byte-level classification proved the cause was incomplete attribute coverage from the issue #19 work, not generated content drift. |
| Next advancement condition | Reopen only if a byte-compared generated path is added without a matching rule, or if a CRLF checkout reports drift again. |

### RH-W-0028
| Field | Value |
|---|---|
| Origin | Strict builder approval activation-boundary defect found during close verification after the repository conversation language changed to `espanol` |
| Priority | P1: strict digest-bound tokens could leak from builder-only execution into ordinary repository maintenance and direct questions, creating an incorrect human approval boundary |
| Dependencies | none |
| Status | completed |
| Completed work | Restricted the strict builder approval protocol to explicitly active `repo-harness-builder` skill execution; clarified that repository identity, self-hosting, generated-file ownership, session skills, ordinary maintenance, and direct questions do not activate it; retained ordinary conversational approval outside that boundary; synchronized canonical product contracts and checked-in OpenCode and Claude projections. |
| Remaining work | none |
| Additions | Added focused product-contract tests for explicit builder activation and for the four approval-bearing references, plus repository lifecycle, current-state, changelog, and projection reconciliation. |
| Exclusions | No change to digest validation, approval tokens, apply semantics, session-skill consent, generated self-hosted outputs unrelated to the checked-in builder package, SDD, Git operations, or network access. |
| Deferrals | none |
| Rejections | Rejected inferring builder activation from repository identity, self-hosting, generated ownership, or the presence of builder files. |
| Evidence | Focused activation-boundary tests pass in the canonical package and both checked-in runtime projections. Full canonical, projection, checker, documentation, and diff verification is recorded in the same close work unit. |
| Decision history | 2026-08-14: completed as the strict approval activation-boundary correction and synchronized across product contract, references, tests, lifecycle documentation, and checked-in projections. |
| Next advancement condition | Reopen only if strict builder tokens or digest-bound approval rules apply outside explicitly active `repo-harness-builder` execution, or active builder execution loses that strict boundary. |

### RH-W-0027
| Field | Value |
|---|---|
| Origin | Full harness audit after RH-W-0006 found confirmed documentation drift |
| Priority | P1: user-facing contracts and lifecycle evidence contradicted completed behavior and chronology |
| Dependencies | none |
| Status | completed |
| Completed work | Corrected fixed daily skill names in both product READMEs, restored RH-W-0004 and RH-W-0005 in the Spanish changelog, removed stale RH-W-0006 next-work and RH-W-0017 ready claims, and recorded the 17 stale self-hosted generated surfaces reported by the manifest-matched owner check as RH-W-0008 evidence without changing them. |
| Remaining work | none |
| Additions | Added focused repository-document assertions for README naming, recent changelog parity, and completed lifecycle status. |
| Exclusions | No generator or product behavior change, no generated self-hosted surface update, no RH-W-0008 implementation, and no change to RH-W-0026. |
| Deferrals | none |
| Rejections | Rejected replacing RH-W-0016's stale transient next-work sentence with another changing work ID; current state owns selected next work. |
| Evidence | Focused tests failed against each confirmed drift before the bilingual documentation-only correction. `python -B skill/repo-harness-builder/scripts/harness_tool.py check --root . --project-name repo-harness-builder` reports drift with exactly 17 paths classified `stale` and 0 writes, and this unit changed none of them. |
| Decision history | 2026-08-14: registered and completed as one bounded audit-correction lifecycle unit; RH-W-0026 remained reserved on its separate branch, making RH-W-0027 the next immutable ID; its record was ported onto main on 2026-08-15. |
| Next advancement condition | Reopen only if the product README naming, recent bilingual chronology, or completed lifecycle projections drift again. |

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
| Next advancement condition | Maintain this contract whenever any tracked reality changes. |

### RH-W-0017
| Field | Value |
|---|---|
| Origin | User-reported Spanish session-stop occurrence; canonical conflict at `skill/repo-harness-builder/scripts/harness_tool.py` in the commit-only question contract |
| Priority | P1: the defect violates the active conversation-language contract at a mandatory approval boundary, but does not cause data loss, unsafe mutation, or an unrecoverable workflow |
| Dependencies | none |
| Status | completed |
| Completed work | Passed the normalized conversation language into the generated session-stop contract and localized only the commit-only approval sentence for Spanish while retaining the English sentence for English. |
| Remaining work | none |
| Additions | Added focused Spanish coverage that requires exact-path wording and the unchanged `<message>` placeholder while rejecting the hardcoded English sentence. |
| Exclusions | No change to approval semantics, no broader session-stop rewrite or language expansion, and no Git or provider mutation. |
| Deferrals | none |
| Rejections | Rejected keeping the exact English wording as mandatory when the surrounding contract requires the active conversation language. |
| Evidence | The focused regression failed against the hardcoded English sentence, then passed after the bounded localization; the complete canonical suite passed 230 tests with 6 skips. |
| Decision history | 2026-08-13: user reported the occurrence and requested durable stop continuity; classified P1 and ready because the cause and bounded correction were known and dependency-free. 2026-08-14: completed with one localized sentence and no approval-scope change. |
| Next advancement condition | Reopen only if a generated commit-only question stops following the active conversation language or loses its exact paths or proposed message. |

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
| Dependencies | RH-W-0024 bounded generated-output migration |
| Status | completed |
| Completed work | Replaced project-derived generated skill names with the fixed `repo-harness-session-start`, `repo-harness-session-stop`, and `repo-harness-documentation` names; preserved commands through explicit mapping; integrated the bounded RH-W-0024 action into normal preview and apply; migrated this self-hosted manifest; and proved arbitrary project IDs and unrelated prefixed user skills remain safe. |
| Remaining work | none |
| Additions | Align skill names with the command layer while preserving command names through the explicit `COMMAND_TO_SKILL` mapping. |
| Exclusions | No change to command names, wrapper contents, or the harness directory layout. |
| Deferrals | none |
| Rejections | Rejected blocking this change on cross-repository name collision. The skills are repository-scoped, live inside each repository, and no global installation exists. |
| Evidence | Focused RED/GREEN naming and migration tests, canonical and projected suites, self-hosted normal CLI migration, fixed manifest and command assertions, and read-only checker reporting `current` with 0 writes. |
| Decision history | 2026-08-14: a hypothetical global-installation collision was withdrawn as speculative; the user confirmed the fixed prefix. 2026-08-14: completed with exact manifest-owned migration and unchanged commands. |
| Next advancement condition | Reopen only if generated skill naming, explicit command mapping, exact ownership, rollback, or projection parity regresses. |

### RH-W-0022
| Field | Value |
|---|---|
| Origin | User-requested capability: a documentation relationship map so an agent can see what a change affects before making it |
| Priority | P2: reconciliation is currently carried by human attention and prose rules, which is the failure mode this repository keeps hitting |
| Dependencies | Design approval on scope, because an unbounded version of this becomes a graph engine the harness must not own |
| Status | proposed |
| Completed work | Recorded the request and the concrete evidence that manual reconciliation fails in practice. |
| Remaining work | Design the smallest declared-relationship format, decide where it lives, and define what the checker verifies. Then implement it test-first. |
| Additions | Make document relationships declared and machine-checked, so touching a canonical owner reports the peers, projections, and mirrors that must move in the same work unit. |
| Exclusions | No code-symbol graph, no call-path analysis, and no dependency engine. CodeGraph already owns code structure and is ignored at the repository root by design. This unit covers documentation relationships only. No daemon, no scheduler, no external service, no new runtime dependency, and no change to the Python 3.10+ standard-library-only constraint. |
| Deferrals | none |
| Rejections | Rejected inferring relationships by scanning link graphs or filename patterns. Inference produces confident wrong answers; a declared edge that the checker verifies produces a correction. |
| Evidence | The `RH-W-0020` work unit required eight coordinated files across four owner/peer pairs, and every edge was resolved by hand from the rulebook gate table. `.repo-harness/check.py` verifies runtime projection parity only; no check covers the English-owner-to-Spanish-peer or roadmap-to-current-state-projection relationships, which are exactly the ones that drifted before. |
| Decision history | 2026-08-14: requested after the README drift showed that documentation reconciliation rules exist only as prose that a reader can miss. Recorded as `proposed` rather than `ready` because the scope boundary is the whole risk in this unit. |
| Next advancement condition | Approve a design that names the declared-edge format, its single owner file, and the exact checker output, and that keeps the map small enough to be maintained by hand. Reference material for the design: `https://github.com/Gentleman-Programming/gentle-ai` and `https://github.com/Gentleman-Programming/engram`. |

### RH-W-0023
| Field | Value |
|---|---|
| Origin | Two failing self-hosted tests found during session close on 2026-08-14 |
| Priority | P1: brittle self-hosted assertions blocked canonical regression evidence and failed again on legitimate roadmap growth |
| Dependencies | none |
| Status | completed |
| Completed work | Replaced the hardcoded record count with exact parser-to-heading structural parity and replaced the obsolete next-work sentence with an ordering assertion for the P0 priority override. |
| Remaining work | none |
| Additions | Keep asserting unique IDs, the complete required field set, allowed priorities and statuses, and the terminal, blocked, and deferred conditions. Those are the checks that carry value. |
| Exclusions | No test framework change, no new dependency, and no weakening of lifecycle validation. |
| Deferrals | none |
| Rejections | Rejected bumping the literal from 17 to the current count. That repeats the defect and makes the next record fail again. |
| Evidence | The focused roadmap tests and the full canonical suite pass: 197 tests with 6 skips. The lifecycle test now compares parsed record IDs with every roadmap heading while retaining unique-ID, field, priority, status, and lifecycle-condition checks; the priority-override test asserts `RH-W-0016` appears before `RH-W-0003` without naming the current next unit. |
| Decision history | 2026-08-14: found while collecting close evidence and recorded for a later bounded correction. 2026-08-14: completed as a test-only work unit without changing product behavior or weakening lifecycle validation. |
| Next advancement condition | Reopen only if roadmap structure or priority-override coverage regresses. |

### RH-W-0024
| Field | Value |
|---|---|
| Origin | issue #16; bounded prerequisite slice of RH-W-0008 required by RH-W-0021 |
| Priority | P1: existing installations cannot adopt renamed generated skills without a safe replacement and removal boundary |
| Dependencies | none; this slice proves ownership directly from the installed manifest and exact generated bytes |
| Status | completed |
| Completed work | Added one typed generated-skill migration plan and executor that binds old and new skill paths, source and destination digests, manifest and checker replacements, and removals; rejects stale or unsafe inputs before mutation; creates targets before removals; rolls back every partial failure; preserves read-only checking; and is idempotent. |
| Remaining work | none |
| Additions | Added focused RED/GREEN coverage for plan identity, ownership, source bytes, paths, collisions, stale plans, rollback, read-only checking, and second application. |
| Exclusions | No arbitrary deletion, human-owned replacement, bulk upgrade, dependency, wrapper redesign, layout redesign, or general migration framework. |
| Deferrals | none |
| Rejections | Rejected extending the additive planner or implementing RH-W-0021 naming inside this prerequisite slice. |
| Evidence | Issue #16; 5 focused migration tests and the complete 202-test canonical suite pass with 6 skips. |
| Decision history | 2026-08-14: implemented as the smallest independently verified prerequisite slice of RH-W-0008; RH-W-0021 then integrated this capability into its naming work unit and completed. |
| Next advancement condition | Use this bounded action in RH-W-0021 without widening it into a general upgrade executor. |

### RH-W-0025
| Field | Value |
|---|---|
| Origin | Approved capability-aware Gentle AI and Engram integration design, supported by [primary-source integration research](integration-research.md) |
| Priority | P2: capability loss and authority drift need explicit behavior, but current repository workflows remain functional and no active unsafe mutation or data-loss defect is demonstrated |
| Dependencies | RH-W-0008 approved replacement and integration executor; RH-W-0022 remains related and independent |
| Status | ready |
| Completed work | Completed primary-source research and approved a generic coordination contract with repository, Engram, Gentle AI, integrated, standalone, promotion, and component-degradation boundaries. |
| Remaining work | Implement bounded capability composition in canonical builder sources with failing contract tests first, covering all four availability combinations, synchronize checked-in builder package projections, and run its own approved consumer regeneration follow-up. |
| Additions | The repository owns durable truth, source, tests, policy, backlog, and approvals. Engram owns semantic memory, relations and conflicts, review-age metadata, and session continuity. Gentle AI optionally owns routing, exact-candidate review and receipts, and delivery gates when enabled. The harness composes available capabilities without another state machine. Integrated mode uses Gentle AI and Engram together. Standalone mode applies when neither is available and uses repository evidence plus approved repository continuity only. Generated consumer harnesses stay generic; this self-hosted repository applies that same rule to its own product source. Durable reusable discoveries are promoted selectively to canonical repository owners after human approval, changing external facts are marked current and revalidated, and incidental noise is not persisted. Component failure degrades only that component unless repository policy makes it mandatory. |
| Exclusions | No auto-enabling review, fabricated approval, copied authority, synthetic SDD, duplicate lifecycle, receipt, or conflict stores, loop-until-clean behavior, third state machine, product-specific consumer wording, or implementation in this documentation work unit. |
| Deferrals | none |
| Rejections | Rejected making memory authoritative, treating disabled capabilities as approval or denial, persisting incidental noise, and coupling this work to RH-W-0022 without an implementation need. |
| Evidence | The approved design and pinned-source analysis are recorded in [integration-research.md](integration-research.md). The RH-W-0008 prerequisite was applied and verified, and both checkers report `current` with 0 stale. The open detection question is resolved: `gentle-ai version` is an advertised read-only surface that returns `gentle-ai <version>` with exit 0 and resolves through PATH, so it is independent of installation layout. Observed locally as `gentle-ai 2.4.0-rc.3`. |
| Decision history | 2026-08-14: research completed and the user explicitly approved the conceptual design. Registered as ready because scope, non-goals, implementation surfaces, start condition, and required tests were bounded. 2026-08-14: changed to blocked after verified executor evidence showed self-hosted regeneration requires RH-W-0008; widening RH-W-0025 would duplicate that owner's approved responsibility. 2026-08-15: returned to ready. RH-W-0008 was applied and verified, and the detection probe left open at medium confidence was resolved against the Gentle AI CLI surface. A proposed split into an Engram half and a blocked Gentle AI half was rejected once the probe existed, because splitting would fragment the four-combination availability matrix across two units. |
| Next advancement condition | Begin with failing contract tests for the four availability combinations, treating only a `verified` probe result as authority to use a capability and never `found_on_path` alone, which `integration-research.md` forbids as executable presence. Retain the separate post-implementation consumer regeneration follow-up; RH-W-0022 remains independent. |

### RH-W-0026
| Field | Value |
|---|---|
| Origin | User-deferred optional CodeGraph integration idea; reference: https://github.com/LCubero/codegraph |
| Priority | P3: the current harness remains functional without this optional capability, and no approved integration design exists |
| Dependencies | Explicit future design or research approval and primary-source review of the pinned or current CodeGraph contract |
| Status | deferred |
| Completed work | Recorded the optional capability, authority boundary, fallback, ownership boundaries, and advancement condition without researching or implementing it. |
| Remaining work | After explicit approval, review the primary source and design whether and how the harness may consume available CodeGraph capabilities. |
| Additions | When available, the harness may optionally use CodeGraph for code structure, symbols, call paths, dependencies, and impact analysis. Repository evidence remains authoritative, and the harness falls back organically when CodeGraph is unavailable. |
| Exclusions | CodeGraph is never required. No implementation, clone, dependency, initialization policy, command contract, or integration design belongs to this unit. The harness must not duplicate a code graph, index, parser, or lifecycle. This remains separate from RH-W-0022, which owns declared documentation relationships, and RH-W-0025, which owns Gentle AI and Engram coordination. |
| Deferrals | Deferred because the user explicitly reserved the idea for another time and no design or research has been approved. |
| Rejections | Rejected making CodeGraph required or treating it as repository authority. |
| Evidence | The user supplied the reference and bounded capability request; this documentation unit did not access or research the external repository. |
| Decision history | 2026-08-14: recorded as a deferred optional capability only, with no implementation or integration commitment. |
| Next advancement condition | Advance only after explicit future design or research approval and primary-source review of the pinned or current CodeGraph contract. |

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
| Status | completed |
| Completed work | Made report, documentation, and conversation language flags render-only; `check` now rejects each as an unrecognized argument while render behavior remains unchanged. |
| Remaining work | none |
| Additions | none |
| Exclusions | No broader CLI redesign. |
| Deferrals | none |
| Rejections | Rejected honoring the flags during checking because installed manifest declarations own documentation and conversation languages, while report language is not a machine-JSON check input. |
| Evidence | Focused public CLI RED/GREEN coverage proves argparse exit 2 for all three flags on `check` and unchanged acceptance on `render`; canonical and projected suites verify the synchronized package. |
| Decision history | 2026-08-13: ordered after legacy locale preservation. 2026-08-14: completed with a parser-only behavior correction and no broader CLI redesign. |
| Next advancement condition | Reopen only if `check` accepts render-only language input or render loses any language flag. |

### RH-W-0005
| Field | Value |
|---|---|
| Origin | Roadmap unit 5; Judgment Day round-1 finding |
| Priority | P1: invalid installed declarations fail with misleading diagnostics |
| Dependencies | RH-W-0003 and RH-W-0004 language contract |
| Status | completed |
| Completed work | Explicit installed `language` declarations now validate object shape, documentation locales through `normalize_documentation_languages()`, and conversation language through `APPROVAL_DOMAINS`, with field-specific diagnostics; absent legacy declarations retain RH-W-0003 inference. |
| Remaining work | none |
| Additions | none |
| Exclusions | No new locale support. |
| Deferrals | none |
| Rejections | none |
| Evidence | A 13-row focused RED/GREEN matrix covers valid explicit and missing legacy declarations plus 10 invalid structure, documentation, and conversation cases; canonical and projected package verification preserve exact parity. |
| Decision history | 2026-08-13: kept behind the language contract prerequisites. 2026-08-14: completed with local installed-declaration validation and no locale, migration, CLI, or RH-W-0006 change. |
| Next advancement condition | Reopen only if explicit invalid declarations stop failing at their owning manifest field or legacy inference regresses. |

### RH-W-0006
| Field | Value |
|---|---|
| Origin | Roadmap unit 6; Judgment Day round-1 finding |
| Priority | P2: checked-in tests fail in valid monolingual layouts |
| Dependencies | RH-W-0003 locale discovery |
| Status | completed |
| Completed work | Made both checked-in documentation tests consult the installed manifest through `declared_languages()`: English maps remain checked in English-only layouts, Spanish map and root-peer assertions run only when `es` is declared, and focused fixtures cover both layouts. |
| Remaining work | none |
| Additions | none |
| Exclusions | No weakening of bilingual checks when Spanish is declared. |
| Deferrals | none |
| Rejections | none |
| Evidence | Focused RED reproduced missing Spanish map and root-peer errors in explicit English-only fixtures; GREEN passes those fixtures while bilingual controls still fail on a broken Spanish link and missing semantic roles. The canonical suite passes 237 tests with 7 skips. |
| Decision history | 2026-08-13: retained as a focused test-contract correction. 2026-08-14: completed with a test-only declared-locale conditional and no generator, manifest, inference, locale, or production behavior change. |
| Next advancement condition | Reopen only if checked-in tests infer Spanish from English files, or declared bilingual layouts stop enforcing complete link and semantic-role checks. |

### RH-W-0008
| Field | Value |
|---|---|
| Origin | Roadmap unit 8; approved replacement and integration gap |
| Priority | P1: approved plans remain descriptive and cannot safely execute |
| Dependencies | RH-W-0003 through RH-W-0007 language and section compatibility corrections |
| Status | completed |
| Completed work | Added typed replacement actions for exact authenticated manifest-owned current-version generated outputs, including human-modified bytes retaining valid current-version provenance. Each action binds path, kind, exact source and destination SHA-256 values, and plan digest; apply recomputes the plan, rejects post-preview changes, and executes replace, create, and append atomically with full rollback. Preview and check remain read-only, second apply is idempotent, human-owned and unverifiable paths remain proposal-only, desired checker bytes must match the desired manifest digest, and RH-W-0024 migration behavior remains separate. |
| Remaining work | none |
| Additions | Added RED/GREEN coverage for typed actions, human-modified current-version generated bytes, ownership refusal, digest binding, stale source and plan rejection, atomic manifest/checker pairing, rollback after mixed writes, read-only preview/check, and idempotence. |
| Exclusions | No bulk upgrade engine. |
| Deferrals | none |
| Rejections | none |
| Evidence | Initial RED failed with 3 failures and 2 errors; correction RED then exposed checker/manifest pairing with 2 focused failures. GREEN passes 9 focused replacement tests, the validation contract regression, and the 251-test canonical suite with 7 skips. Self-hosted preview reports 0 writes and exactly these 17 replacement paths without applying them: `.claude/commands/documentation.md`, `.claude/commands/repo-harness-help.md`, `.claude/commands/session-start.md`, `.claude/commands/session-stop.md`, `.claude/skills/repo-harness-documentation/SKILL.md`, `.claude/skills/repo-harness-session-start/SKILL.md`, `.claude/skills/repo-harness-session-stop/SKILL.md`, `.opencode/commands/documentation.md`, `.opencode/commands/repo-harness-help.md`, `.opencode/commands/session-start.md`, `.opencode/commands/session-stop.md`, `.opencode/skills/repo-harness-documentation/SKILL.md`, `.opencode/skills/repo-harness-session-start/SKILL.md`, `.opencode/skills/repo-harness-session-stop/SKILL.md`, `.repo-harness/skills/repo-harness-documentation/SKILL.md`, `.repo-harness/skills/repo-harness-session-start/SKILL.md`, and `.repo-harness/skills/repo-harness-session-stop/SKILL.md`. Manifest and checker are absent because their current bytes already equal the deterministic desired pair; a future changed valid pair is planned and applied together. |
| Decision history | 2026-08-13: retained as the first mutation capability after prerequisite correctness. 2026-08-14: moved to ready after RH-W-0006 completed the final prerequisite. 2026-08-14: completed with the smallest authenticated generated-output replacement executor; live self-hosted replacement remains a separate approval-bound follow-up. |
| Next advancement condition | Reopen if exact ownership, digest binding, rollback, read-only preview/check, or idempotence regresses. The old 17-path preview is historical evidence only and its old digest is stale. Any future self-hosted follow-up must use a freshly generated exact set and digest from current repository bytes, followed by approval, apply, and verification. |

### RH-W-0009
| Field | Value |
|---|---|
| Origin | Roadmap unit 9; existing README selector gap |
| Priority | P1: reciprocal localized documentation remains safely blocked |
| Dependencies | RH-W-0008 approved integration executor |
| Status | completed |
| Completed work | One-sided selector plans fail closed, and the bounded reciprocal selector action now runs through the approved executor. When an existing `README.md` lacks the reciprocal link, the plan binds one append of the exact selector block with its current and result digests and creates the Spanish peer in the same apply. |
| Remaining work | none |
| Additions | Three regressions: LF and CRLF selector integration with digest-bound append, idempotence on a second run, and fail-closed behavior when the existing README is not a regular file. |
| Exclusions | No general README rewriting. The append adds only the selector block, preserves every existing byte and the file's newline convention, and never reorders content. |
| Deferrals | none |
| Rejections | Rejected keeping the block now that the executor exists, because it left declared bilingual repositories permanently one-sided. Rejected rewriting or reformatting the human README to place the selector. |
| Evidence | The canonical suite passes 263 tests with 7 skips. The retargeted invariant asserts a Spanish peer never appears without a reciprocal link: either the plan appends the selector and creates the peer, or it blocks and apply fails closed with no peer on disk. |
| Decision history | 2026-08-13: prerequisite dependency on RH-W-0008 made explicit so the idea cannot disappear. 2026-08-15: completed once that executor was applied and verified; blocking became the wrong default because the capability to integrate safely now exists. |
| Next advancement condition | Reopen only if a Spanish peer can be created without a reciprocal link, or if the selector append stops preserving existing bytes or the newline convention. |

### RH-W-0010
| Field | Value |
|---|---|
| Origin | Roadmap unit 10; stale installed builder gap |
| Priority | P2: consumers cannot distinguish package staleness from projection drift |
| Dependencies | RH-W-0008 executable upgrade boundary |
| Status | ready |
| Completed work | Fresh, stale, absent, and unverifiable fixture outcomes are specified. |
| Remaining work | Define and detect builder package freshness separately. |
| Additions | none |
| Exclusions | No runtime reload promise. |
| Deferrals | none |
| Rejections | none |
| Evidence | Generated harness checking does not establish installed builder package freshness. |
| Decision history | 2026-08-13: retained as a separate concept from runtime projection drift. |
| Next advancement condition | Begin with failing package freshness classification tests while preserving the RH-W-0008 mutation boundary. |

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
- The user-reported session-stop language leak is `RH-W-0017`; it completed independently without displacing the product work selected at the time.
- The unattended internal-maintenance idea is `RH-W-0018`; it remains deferred until its two consent scopes and safety tests are designed, and it does not change the interactive default.
- The wrapper provenance cost is `RH-W-0019`, the product README drift is `RH-W-0020`, and the fixed `repo-harness-` skill prefix is `RH-W-0021`. `RH-W-0020` was documentation-only, `RH-W-0019` is ready, and `RH-W-0021` completed through the bounded RH-W-0024 action.
- The documentation relationship map is `RH-W-0022` and stays `proposed`, not `ready`. Its scope boundary is the entire risk: declared documentation edges are the request, a code or dependency graph is not, and CodeGraph already owns code structure.
- The bounded generated-output migration is `RH-W-0024`, the completed issue #16 prerequisite slice of RH-W-0008. It unblocks the safe mechanism required by RH-W-0021 without completing the rename itself.
- The generic capability-aware Gentle AI and Engram coordination contract is `RH-W-0025` at `P2` and is blocked on `RH-W-0008`, which owns content-bound replacement and integration for existing generated outputs. It keeps repository authority primary, composes optional owners without a third state machine, and remains separate from but related to `RH-W-0022`.
- The full harness audit correction is `RH-W-0027`; it completed as documentation and test work while leaving all 17 stale self-hosted generated surfaces unchanged for RH-W-0008.
- English is mandatory as the primary documentation locale, and the reason is now a standing decision rather than a bare `require()`. See [decisions.md](decisions.md).
- A speculative cross-repository name collision was raised against `RH-W-0021` and withdrawn. Generated skills are repository-scoped and there is no global installation, so the risk was a future problem that is not real. Solve the problem in front of you.
- Avoided choosing MCP, CLI, binary, third-party, migration-framework, general README rewrite, bulk upgrade, provider mutation, runtime reload, or universal human-doc cap without evidence.
