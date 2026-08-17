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

### RH-W-0035
| Field | Value |
|---|---|
| Origin | Measured interpreter-invocation audit of this repository, prompted by a confirmed `python` versus `python3` failure observed on a macOS collaborator machine in an unrelated repository |
| Priority | P1: the canonical test suite cannot start on a machine without a bare `python` on PATH, so macOS and PEP 394 Linux contributors cannot verify this builder at all, and every generated harness instructs its consumers to run an interpreter name the same builder does not guarantee |
| Dependencies | none; independent of `RH-W-0034` and of any release |
| Status | ready |
| Completed work | Audited every interpreter invocation in the repository, measured the interpreters actually present on the Windows development machine, classified findings into generated consumer text, test-suite invocations, contributor documentation, and out-of-scope imported material, and confirmed byte parity of the three `harness_tool.py` copies at `f344fca692249d91b7c3c67ec81c54e11a348a3e63a1740b4e41c9b55c60ab12`. |
| Remaining work | Replace the 13 bare `python` subprocess literals in the test suite with `sys.executable`; add one focused portability guard; correct the 7 generated-text sites and the contributor commands in `references/validation.md`; update the 2 literal assertions; synchronize the canonical, OpenCode, and Claude projections; and reconcile lifecycle, Spanish peers, and Engram `work/RH-W-0035`. |
| Additions | Portable interpreter resolution in tests, one guard covering bare interpreter literals in subprocess argument lists, and generated documentation copy that names the Windows and POSIX cases explicitly instead of assuming either one. |
| Exclusions | No change to any other repository, no platform-conditional rendering of generated documentation, no interpreter version bump, no new dependency, no `py` launcher requirement, no Git mutation, no external service, no release, and no changelog entry while planned. |
| Deferrals | Real execution on macOS and Linux hardware is deferred until a collaborator machine is available; Windows evidence is authoritative for this unit and the correction must not depend on a single platform to be valid. |
| Rejections | Rejected a blanket `python` to `python3` substitution, because `python3` is absent from a default python.org Windows installation and this machine only resolves it through Python Manager. Rejected rendering the generated command from the installing machine's platform, because the generated file is committed into a repository that contributors on the other two systems clone. Rejected touching `unified_ai_brain_jl_i3`, `.ld9/referencia_panda/`, `.ld9/gentle-ai/`, and the historical `docs/superpowers/plans/` record. |
| Evidence | `skill/repo-harness-builder/tests/test_harness_tool.py` invokes `subprocess.run(["python", ...])` at lines 83, 91, 433, 451, 902, 912, 1289, 1325, 1764, 1786, 1807, 1831, and 2787 while the same file already uses `sys.executable` at lines 1110, 3555, 3961, 3972, 4053, 4070, 4135, 4156, and 4178. `skill/repo-harness-builder/scripts/harness_tool.py` emits `python .repo-harness/check.py` into consumer text at lines 756, 1011, 1209, 1306, 1330, 1426, and 1519, while line 1642 gives the generated checker a `#!/usr/bin/env python3` shebang, so generated output contradicts itself. Measured on the Windows development machine on 2026-08-17: `python` resolves to `C:\Python314\python.exe` reporting 3.14.3, `python3` resolves through Python Manager reporting 3.14.2, and `py -3` reports 3.14.2. Baseline before any change: 293 tests passed with 7 skips. |
| Decision history | 2026-08-17: registered as `ready` after the user confirmed the standing Windows, Linux, and macOS support requirement already recorded for line endings, and directed that Windows is the current working platform while the correction must keep the other two systems valid. |
| Next advancement condition | Move to `active` when the portability guard is written as failing coverage. Complete only after the canonical, OpenCode, and Claude suites pass, projections are byte-identical, generated text no longer contradicts the generated shebang, and Engram `work/RH-W-0035` is reconciled. |

### RH-W-0034
| Field | Value |
|---|---|
| Origin | Verified `v3.1-rc.2` consumer transcript and installed-output evidence from a Spanish-only non-Git consumer |
| Priority | P1: the published candidate can generate a checker that rejects its own valid Spanish-only manifest, advertise nonexistent documentation, select report language from wrapper prose, and route a builder defect into irrelevant provider reporting |
| Dependencies | none for implementation; successor release publication remains a separate approval-bound work unit |
| Status | ready |
| Completed work | Confirmed the defects against canonical source and tests, selected `RH-W-0034` as the next immutable ID, resolved every implementation decision, and created the canonical executable plan plus complete Spanish peer. |
| Remaining work | Execute [the implementation plan](rh-w-0034-implementation-plan.md) with strict RED/GREEN coverage; synchronize canonical, OpenCode, and Claude packages; prove owner and generated checker parity in Spanish-only subprocess fixtures; replay the consumer path in a temporary non-Git fixture; and complete lifecycle and release evidence. |
| Additions | Concrete Spanish interview copy; typed report-language evidence with ask-once fallback; exact owner/generated documentation-locale parity; Spanish-only local map and link validation; owner-before-provider failure classification; safe upgrade and fresh regeneration guidance. |
| Exclusions | No direct `E:/code/VPS` mutation, consumer checker patch, Gentle AI source change without separate evidence, Git or RDD mutation, provider or GitHub action, repository publication, external service, tag or release alteration, new locale, translation service, dependency, or changelog entry while planned. |
| Deferrals | Publishing a successor release candidate is deferred until the implementation, all three package suites, temporary consumer replay, explicit-dimension owner check, generated checker, links, lifecycle, and byte audits pass. |
| Rejections | Rejected patching `.repo-harness/check.py` in the consumer, treating English wrapper prose as human language evidence, requiring English first in a Spanish-only manifest, advertising bilingual-only files, classifying builder-generated mismatch as a Gentle AI defect, attempting clone-scope RDD disable in a non-Git directory, and implying GitHub repository publication. |
| Evidence | `normalize_documentation_languages()` accepts `("es",)` while generated `check_script().parse_manifest()` rejects `documentation[0] != "en"`; `docs_outputs()` emits nonexistent English-owner and bilingual-maintenance links in Spanish-only mode; the report-language contract already says wrapper prose and tool output are not conversation evidence; verified consumer evidence shows the resulting English report, checker failure, ownership misclassification, provider-consent detour, non-Git directory, and global RDD already off. |
| Decision history | 2026-08-17: registered as `ready` because the user supplied verified reproduction evidence and explicitly resolved wording, language precedence, preservation behavior, failure ownership, remediation safety, Git/RDD boundaries, release sequencing, exact verification, and non-goals. |
| Next advancement condition | Move to `active` when implementation begins from T-001 RED coverage. Complete only after every acceptance criterion in the canonical plan passes and Engram topic `work/RH-W-0034` is reconciled. |

### RH-W-0033
| Field | Value |
|---|---|
| Origin | Completed language-workflow correction found to be non-trivial during pre-commit documentation reconciliation |
| Priority | P1: the builder could stop after a blocking language answer instead of asking the next required question, and Spanish-only human documentation could be misclassified as a technical artifact and left untranslated |
| Dependencies | none |
| Status | completed |
| Completed work | Made each report, conversation, and documentation language answer resume immediately with the next required blocking question while retaining one question per turn. Distinguished human repository documentation from technical artifacts so declared Spanish documentation receives complete Spanish peers at canonical paths while commands, skills, scripts, tests, identifiers, and machine-significant values remain English. Synchronized canonical, OpenCode, and Claude package projections. |
| Remaining work | none |
| Additions | Added focused behavioral coverage for interview resumption and for the human-documentation versus technical-artifact language boundary. |
| Exclusions | No new locale, natural-language detector, translation service, change to report, conversation, documentation, artifact, or guide-language precedence, localization of technical artifacts, Git mutation, external service, or SDD. |
| Deferrals | none |
| Rejections | Rejected requiring a new invocation after each language answer, asking multiple blocking questions in one turn, and treating every repository artifact as technical merely because agents may read it. |
| Evidence | RED ran 4 tests with 1 failure and 2 errors. Focused GREEN passed 6 tests. Full canonical verification passed 293 tests with 7 skips; OpenCode and Claude each passed 293 tests with 15 skips. The generated checker reports `current` with 0 writes. |
| Decision history | 2026-08-17: completed both defects as one bounded language-contract work unit because the interview flow selects the language dimensions and the artifact boundary determines which selected documentation locales receive human peers. Added this lifecycle record during the required pre-commit documentation reconciliation. |
| Next advancement condition | Reopen only if a blocking language answer fails to present the next required question immediately, more than one blocking question appears in a turn, declared Spanish human documentation is absent or incomplete, or technical artifacts become localized. |

### RH-W-0032
| Field | Value |
|---|---|
| Origin | Approved issue #48 and the user-owned verification-before-assertion principle |
| Priority | P1: generated workflows could qualify status evidence yet still present broader assumptions, stale memory, unrelated proxies, or confidence wording as facts |
| Dependencies | none |
| Status | completed |
| Completed work | Added one universal rule to the generated rulebook defining verified fact, supported inference, unknown or unverifiable, and human decision required; required applicable, trustworthy, proportionate evidence; prohibited false substitutes; and required available verification before asking with minimum-evidence escalation when verification is unavailable or contradictory. Session start, session stop, and documentation inherit the rulebook without duplicating the full principle. Synchronized canonical, OpenCode, and Claude package sources and tests byte for byte. |
| Remaining work | none |
| Additions | Added one behavioral regression proving canonical ownership, all three inheritance paths, proportional verification, organic operation, the four claim classes, forbidden substitutes, minimum escalation, and exactly one full rule across the generated contract. |
| Exclusions | No exhaustive reads or test mandate, state machine, runtime dependency, network, external mutation, SDD, or change to RH-W-0031 language behavior beyond natural rulebook inheritance. |
| Deferrals | none |
| Rejections | Rejected duplicating the full rule in every skill, treating confidence as evidence, asking humans for facts the repository can verify, and using unrelated or stale proxies. |
| Evidence | RED ran 1 focused test with the expected missing-rule assertion failure and no import, fixture, or syntax error. GREEN passed 5 focused canonical tests, then 276 canonical tests with 7 skips and 276 tests with 15 skips in each package projection. Package script and test hashes matched byte for byte. The explicit installed-dimension consumer preview reported digest `576584d67fbc636da790384e0d6b83d451103f89151b0657871b5dd787067a2b`, 0 writes, and no create, append, replace, migrate, or blocked actions. |
| Decision history | 2026-08-15: registered and completed as the smallest universal generated-contract correction after strict RED/GREEN and full package verification. |
| Next advancement condition | Reopen only if generated workflows present unsupported claims as verified facts, omit a claim class, demand disproportionate verification, ask for facts they can verify, fail to name a missing capability, or duplicate the full principle across skills. |

### RH-W-0031
| Field | Value |
|---|---|
| Origin | A conversation-language audit found that generated session and help contracts can treat English artifacts as switch evidence and can emit fixed English approval wording into a Spanish conversation |
| Priority | P1: direct human approval and help replies can use the wrong language at active workflow boundaries, recurring beyond the bounded RH-W-0017 sentence correction |
| Dependencies | none |
| Status | completed |
| Completed work | Added one typed resolver and concise shared runtime principle for session start, session stop, and help; enforced verified-instruction, persisted-preference, verified-session, validated-manifest, and English precedence; preserved persisted preference on flattened hosts; removed fixed-language direct questions; isolated typed machine bytes; decoupled guide locale from reply language; added the deterministic adversarial matrix; synchronized both package projections; and applied the exact approved 13-path consumer plan. |
| Remaining work | none |
| Additions | Persisted explicit preference changes only through a later verified explicit human instruction. Manifest is validated fallback configuration, not conversation evidence. Report, conversation, documentation, artifact, and guide languages remain independent. Machine-significant values remain byte-identical and language resolution is never a security boundary. |
| Exclusions | No natural-language detector, new locale, translation service, approval-scope change, strict builder-token change, live-model completion gate, network, external mutation, or SDD. |
| Deferrals | none |
| Rejections | Rejected another piecemeal literal translation because RH-W-0017 proved that approach does not close the defect class. Rejected letting documentation locale own direct reply language. |
| Evidence | RED ran 12 tests with 39 expected assertion failures and no syntax, import, or fixture errors. Final GREEN passed 275 canonical tests with 7 skips and 275 tests with 15 skips in each package projection. Corrected digest `dff015e12022536ee22060603aece83ce8c5a413aac1c092e4fff98bb086136e` applied exactly 13 paths with all installed dimensions explicit. The explicit second preview had zero actions, `.repo-harness/check.py` reported `current` with 0 writes, package hashes matched, links resolved, and diff, character, and path audits passed. |
| Decision history | 2026-08-15: registered as one unit because start, stop, help, fallback, and tests share one policy boundary. Temporarily returned from completed to blocked after independent verification found omitted owner-render dimensions and missing Spanish chronology. Completed only after the corrected explicit-dimension apply and full independent verification. |
| Next advancement condition | Reopen only if owner renders omit installed dimensions, unverified context replaces persisted preference, guide locale controls reply language, fixed-language questions return, flattened hosts claim reliable switch detection, or machine-significant bytes enter localization. |

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
| Completed work | Located the emission at `skill/repo-harness-builder/scripts/harness_tool.py:1441-1449` and proved the marker is load-bearing for upgrade classification. Re-verified on 2026-08-15 after the file shifted; the earlier `1222-1230` and `208-217` citations are stale and were corrected here and in the Spanish peer. |
| Remaining work | Move wrapper provenance out of the prompt-visible bytes and give `generated_version()` an equivalent evidence source in the same change. The marker and the version share one line, so removing it returns `None` from `generated_version()` rather than `legacy`, which fails the ownership guard at `harness_tool.py:1883` and takes wrappers outside the content-bound replacement executor. `.repo-harness/manifest.json` is the candidate replacement evidence source because the prompt never loads it. `help_wrapper()` at `harness_tool.py:1450-1468` emits the same line and belongs to the same change. |
| Additions | none |
| Exclusions | No change to skill or documentation provenance, which carries real reader value and lives in files the prompt does not load on every invocation. |
| Deferrals | none |
| Rejections | Rejected deleting the comment on its own, because wrappers have no frontmatter and the `source:` line is their only version evidence. |
| Evidence | `wrapper()` emits `<!-- Generated by repo-harness-builder; source: wrapper/<runtime>:<version> -->`; `generated_version()` at `harness_tool.py:398-406` falls back to the `source: [^\n:]+:(\d+\.\d+)` regex when no frontmatter `version:` exists, and returns `None` when `GENERATED_MARKER` is absent. Observed live: a `/session-start` invocation in this repository loaded `<!-- Generated by repo-harness-builder; source: wrapper/claude-code:3.0 -->` as the first prompt bytes. |
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
| Status | completed |
| Completed work | Implemented deterministic bounded `gentle-ai version` PATH execution, exact three-state classification, validation of Engram host MCP or CLI evidence, two-component integrated selection, component-scoped post-installation degradation, selective promotion guidance, generic owner contracts, synchronized package projections, and the six authenticated self-hosted generated skill replacements through RH-W-0008, which were applied and verified. |
| Remaining work | none |
| Additions | The repository owns durable truth, source, tests, policy, backlog, and approvals. Engram owns semantic memory, relations and conflicts, review-age metadata, and session continuity. Gentle AI optionally owns routing, exact-candidate review and receipts, and delivery gates when enabled. The harness composes available capabilities without another state machine. Integrated mode uses Gentle AI and Engram together. Standalone mode applies when neither is available and uses repository evidence plus approved repository continuity only. Generated consumer harnesses stay generic; this self-hosted repository applies that same rule to its own product source. Durable reusable discoveries are promoted selectively to canonical repository owners after human approval, changing external facts are marked current and revalidated, and incidental noise is not persisted. Component failure degrades only that component unless repository policy makes it mandatory. |
| Exclusions | No auto-enabling review, fabricated approval, copied authority, synthetic SDD, duplicate lifecycle, receipt, or conflict stores, loop-until-clean behavior, third state machine, product-specific consumer wording, or implementation in this documentation work unit. |
| Deferrals | none |
| Rejections | Rejected making memory authoritative, treating disabled capabilities as approval or denial, persisting incidental noise, and coupling this work to RH-W-0022 without an implementation need. |
| Evidence | RED ran 11 focused tests with 26 expected assertion failures and no errors. GREEN passed all 11 focused tests and the initial 287-test canonical suite with 7 skips. Gentle AI preflight returned `gentle-ai 2.4.0-rc.3`; Engram was verified through the active MCP project boundary. Fresh explicit-dimension preview digest `a0c09b5c8ff01c831799d7b8f7f78caf62d76b00b14a7b63b8591bd67852937b` applied exactly six authenticated session-start and session-stop owners through RH-W-0008. Final package, checker, parity, link, lifecycle, diff, and idempotence evidence is recorded in the completing work unit. |
| Decision history | 2026-08-14: research completed and the user explicitly approved the conceptual design. Registered as ready because scope, non-goals, implementation surfaces, start condition, and required tests were bounded. 2026-08-14: changed to blocked after verified executor evidence showed self-hosted regeneration requires RH-W-0008. 2026-08-15: returned to ready after RH-W-0008 was applied and the Gentle AI probe was resolved. 2026-08-16: completed through strict TDD, synchronized projections, and authenticated consumer apply without coupling RH-W-0022. |
| Next advancement condition | Reopen only if capability classification gains another state, integrated selection bypasses either verification, owner state is copied, post-installation loss affects the wrong component, promotion bypasses repository verification or human approval, or RH-W-0008 replacement safety regresses. |

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
| Completed work | The four outcome names `fresh`, `stale`, `absent`, and `unverifiable` were recorded. They were only named, never defined; the 2026-08-15 analysis below supplies the definitions and the verified gap. |
| Remaining work | Define package freshness as `manifest.generator.version` compared against the running builder `VERSION`, classify `fresh` when equal, `stale` when a valid earlier version, `absent` when no manifest or no generator field exists, and `unverifiable` when the manifest is unreadable or malformed. Report it as its own field, separate from per-file projection drift, with failing classification tests first. |
| Additions | The classification is local and offline. A consumer repository holds no upstream builder copy, so the manifest generator version compared against the running tool is the only mechanism available without network access. |
| Exclusions | No runtime reload promise. No automatic upgrade: detection only, preserving the RH-W-0008 mutation boundary. |
| Deferrals | none |
| Rejections | none |
| Evidence | Verified 2026-08-15. The builder `check()` at `skill/repo-harness-builder/scripts/harness_tool.py:2386` never reads `manifest.generator.version`. The generated checker template at `harness_tool.py:1560` requires the installed manifest generator to equal the version frozen into that checker, which detects tampering rather than staleness, because a consumer checker is pinned to the builder that emitted it. Per-file `generatorVersion` is emitted only for already-drifted files at `harness_tool.py:2457-2460`, so a fully `current` projection set carries no package identity at all. Live divergence exists and nothing reports it: the installed manifest records `generator.version` `3.0` while `skill/repo-harness-builder/SKILL.md` declares `metadata.version` `3.1`. |
| Decision history | 2026-08-13: retained as a separate concept from runtime projection drift. 2026-08-15: analysis corrected the overstated completed work, recorded the exact detection gap, and established that no human decision is required because the comparison reference is repository-local. |
| Next advancement condition | Begin with failing package freshness classification tests for all four outcomes while preserving the RH-W-0008 mutation boundary. |

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
- The generic capability-aware Gentle AI and Engram coordination contract is `RH-W-0025` at `P2` and is `completed`. RH-W-0008 applied and verified its six authenticated consumer replacements. Repository authority remains primary, optional owners compose without a third state machine, and RH-W-0022 remains related and independent.
- The full harness audit correction is `RH-W-0027`; it completed as documentation and test work while leaving all 17 stale self-hosted generated surfaces unchanged for RH-W-0008.
- English is mandatory as the primary documentation locale, and the reason is now a standing decision rather than a bare `require()`. See [decisions.md](decisions.md).
- A speculative cross-repository name collision was raised against `RH-W-0021` and withdrawn. Generated skills are repository-scoped and there is no global installation, so the risk was a future problem that is not real. Solve the problem in front of you.
- Avoided choosing MCP, CLI, binary, third-party, migration-framework, general README rewrite, bulk upgrade, provider mutation, runtime reload, or universal human-doc cap without evidence.
