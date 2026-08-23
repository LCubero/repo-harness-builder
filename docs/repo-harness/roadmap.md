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

## Proposed Audit Execution Order

The owner approved this revised execution order on 2026-08-21 after Judgment Day found zero severe defects. Approval of the order changes no unit status or advancement condition. Completed `RH-W-0050` is baseline context for every scheduled unit below, not future work:

1. Deliver `RH-W-0054` because it is `P1` and its only declared unit dependency, `RH-W-0050`, is completed. No storage or recovery unit blocks it. If its design proves it needs persisted evidence, it joins the local-state architecture design in step 3 instead.
2. Run `RH-W-0011` as the first real application of the completed classification baseline. It inventories this repository's own historical findings with the identical `integrate`, `close`, `defer`, `reject`, or `noise` vocabulary, so it validates `RH-W-0050` against a real backlog at low marginal cost.
3. Jointly design the local-state architecture for `RH-W-0051`, `RH-W-0048`, and `RH-W-0052` because they share `.repo-harness` local namespaces, recoverable inventory, ownership evidence, ignore and override semantics, reports, and safety boundaries. The same design reserves namespace for the other declared consumers of that contract: the `RH-W-0053` checkpoint bytes, the durable repository-scoped consent or preference store that both `RH-W-0046` and `RH-W-0018` require, and the `RH-W-0040` violation record location. Reserving a namespace neither advances nor implements those units; it prevents a second breaking change to the storage contract.
4. Implement `RH-W-0051` first within that architecture so `.repo-harness/tmp/` and `.repo-harness/archive/` establish the storage semantics consumed by later units.
5. Implement `RH-W-0053` next because it is `P1` and both declared dependencies, `RH-W-0050` and `RH-W-0051`, are satisfied at this point.
6. Implement `RH-W-0048` to prove additive, non-overwriting runtime recovery on those storage primitives.
7. Implement `RH-W-0052` after that because archive, uninstall, and reinstall are the highest-risk operations and must consume proven storage and recovery primitives.
8. Design `RH-W-0046` and `RH-W-0018` together because both require the same missing primitive, a durable repository-scoped consent or preference that stays visible and revocable. Implementation stays separate, and `RH-W-0018` remains `deferred` until its own advancement condition is met.
9. Continue with `RH-W-0044`, whose `RH-W-0043` prerequisite is already completed, so only its written scope design remains.
10. Continue with `RH-W-0022`. Reversing it with `RH-W-0044` was an unsupported optimization, not a dependency-backed correction.
11. Continue with `RH-W-0055`.
12. Continue with `RH-W-0040` last because it addresses only the unmechanizable residue, and which rules belong to that residue is unknown until the earlier checks exist.

Joint design does not merge implementation units. Each implementation retains separate tests, commit, issue, pull request, lifecycle evidence, rollback boundary, and its own authoritative advancement condition. Completed `RH-W-0050` remains the independent baseline for the architecture bundle.

Historical owner-approved order: the 2026-08-20 sequence placed the `RH-W-0051`, `RH-W-0048`, and `RH-W-0052` bundle immediately after `RH-W-0050`, then `RH-W-0011`, `RH-W-0046`, `RH-W-0044`, `RH-W-0022`, and `RH-W-0040`, and reviewed `RH-W-0053`, `RH-W-0054`, and `RH-W-0055` last. The re-audit found two `P1` units sitting behind eight units, seven `P2` and one `P3`, with no declared dependency requiring it, and found three further declared consumers of the same local-state contract. The owner-approved revised order addresses both findings without changing any record status or advancement condition.

## Consolidated Portfolio Audit

The seven owner notes were audited against the existing portfolio rather than copied into a second backlog. Verified repository behavior, supported inferences from that behavior, owner decisions, and unresolved product choices remain distinct:

- Human decisions: preserve the first critical path above; register interrupted-session recovery, wrong-answer recovery, and installation onboarding as separate proposed units; integrate existing-memory reconciliation into `RH-W-0050`; and extend the shared non-Git architecture without merging implementation boundaries.
- Verified facts: `RH-W-0025` is completed; plain checker execution is read-only; the installed harness has no supported interrupted-checkpoint, answer-invalidation, or copy/paste onboarding workflow today.
- Supported inference: release asset naming belongs with installation onboarding because the install command must identify a stable package artifact. A written release contract and consumer replay would raise this to verified design evidence.
- Unknown or human decision required: an optional ATL-like capability inventory remains deferred until its product boundary and owner are decided. It must not duplicate Gentle AI-owned routing, review, receipt, or delivery state, and it does not reopen `RH-W-0025`.

A second pass on 2026-08-21 re-audited the whole non-terminal portfolio rather than only the owner notes. It counted 18 non-terminal records, 12 `proposed` and 6 `deferred`, and produced the revised execution order above, which the owner approved on 2026-08-21 after Judgment Day:

- Verified facts: `RH-W-0054` declares `RH-W-0050` as its only unit dependency and `RH-W-0053` declares `RH-W-0050` and `RH-W-0051`, while both were ordered behind eight units, seven `P2` and one `P3`. `RH-W-0011` and `RH-W-0050` use the identical `integrate`, `close`, `defer`, `reject`, or `noise` classification vocabulary. `RH-W-0044` still named the completed `RH-W-0043` as a pending prerequisite. `RH-W-0055` named a future release contract as a dependency while also declaring that same contract among its own additions.
- Supported inference: `RH-W-0051` is a storage hub rather than one of three peers. `RH-W-0048`, `RH-W-0052`, and `RH-W-0053` declare its storage semantics as a dependency, and `RH-W-0046`, `RH-W-0018`, and `RH-W-0040` each name a storage location the same contract would own, so designing only `tmp` and `archive` risks a second breaking change.
- Verified non-defect: the current-state runtime set `claude-code` and `opencode` is this repository's installed manifest, while `RH-W-0055` names the product's supported runtimes, which `skill/repo-harness-builder/scripts/harness_tool.py:26` defines as `opencode`, `claude-code`, and `pi`. The two statements have different scopes and do not conflict.

## Active Records

<!-- doc-role:active-records -->

### RH-W-0060
| Field | Value |
|---|---|
| Origin | Extracted from the `RH-W-0059` design review on 2026-08-22. Two findings of the `RH-W-0056` external harness review are real here but are not transport, and they were travelling on a `P1` unit's urgency. |
| Priority | P3: neither loses work nor produces a wrong result. Both make a repository state report incomplete or misleading in a way a reader cannot detect from the report itself. |
| Dependencies | none |
| Status | proposed |
| Completed work | Verified both against this repository rather than importing them. `core.autocrlf` is `true` in this clone while `.gitattributes` pins `eol=lf` for every byte-compared generated path, so the configuration that fakes a content change is present and active here, not hypothetical. The generated session-start execution step 3 inspects branch, worktree, index, upstream, remotes, and each upstream remote URL, host, and scheme, and lists no branches. |
| Remaining work | Design and then implement test-first. Finding `C12`: a repository state review lists branches, and what settles whether a merged branch can be deleted is an ancestry check against both the local and the remote tip, never the branch name or its date. Finding `C11`: before a staging classification reports a path as content-modified, compare it against the committed blob, because a line-ending normalization under this configuration reports one spurious modification per affected file. Decide for each whether it belongs in the generated session skills or only in this repository's own working agreements. |
| Additions | Branch coverage in the generated repository state review, and a committed-blob comparison before reporting a content change in the staging classification. |
| Exclusions | No branch deletion, no automatic cleanup, no change to the transport contract that `RH-W-0059` owns, no change to the `.gitattributes` derivation that `RH-W-0029` owns, and no change to the conflict detection that `RH-W-0042` owns. |
| Deferrals | none |
| Rejections | Rejected keeping these inside `RH-W-0059`. A `P1` transport defect and two `P3` reporting gaps do not share a delivery boundary, and bundling them would have let the slower work set the pace of the urgent fix. |
| Evidence | Verified: `core.autocrlf` is `true` locally and `.gitattributes` pins `eol=lf`, so the `C11` configuration is present. Verified: generated session-start step 3 enumerates six inspection targets and branches are not among them. Verified: `RH-W-0029` derives the attribute rules and `RH-W-0042` fixed the conflict-detection half, so only the reporting half of `C11` remains. Human decision: whether either clause belongs in the generated contract or only in this repository's own agreements. |
| Decision history | 2026-08-22: registered as proposed, extracted from the `RH-W-0059` design review that cut that unit from eight clauses to four. The same review withdrew finding `C10`, the bare local date, as a verified non-defect here and it is deliberately not carried into this unit. Registered outside the owner-approved 2026-08-21 execution order; its scheduling remains an owner decision. |
| Next advancement condition | Advance after a written design decides, for each of the two clauses, where the obligation lives, what a state review must list, and what evidence settles a content change, with focused tests that fail before implementation. |

### RH-W-0059
| Field | Value |
|---|---|
| Origin | Findings `C1` through `C3`, `C5`, `C7`, `C8`, and `C10` through `C12` of the `RH-W-0056` external harness review, extracted into their own unit on 2026-08-22 because the review left a measured defect owned by a document instead of by the roadmap. |
| Priority | P1: the generated session contracts promise a remediation banner naming an alternate credential path, and against a passphrase-protected key or an uncached HTTPS credential they cannot reach it. The session hangs with no error, no banner, and no timeout, which is worse on the close path where memory may already be saved. |
| Dependencies | none. `RH-W-0046` owns whether a read-only fetch is separable from the network-access threshold; this unit owns how the fetch is invoked once it is authorized, and the two do not block each other. |
| Status | completed |
| Completed work | Implemented the approved four-clause contract test-first. Seven focused `NonInteractiveTransportTests` failed with 16 assertions before implementation and pass afterward; the canonical suite passes 367 tests with 8 skips; the generated checker reports `current` with zero writes; and the canonical self-hosted session skills plus both runtime projections are synchronized, with each projection keeping its own provenance pointer. The owner approved the design and both recommendations: name the exact flags, and keep `C1` and `C2` excluded. Wrote the [design](rh-w-0059-design.md) and reviewed it against repository evidence, which cut it from eight clauses to four. Three clauses left as scope creep and are now `RH-W-0060` or withdrawn, one imported justification was retracted, and two findings were reclassified as already satisfied. Pending owner approval, with three open human decisions and no implementation authorized. Measured the gap in this repository's own generated contract rather than assuming it. `.repo-harness/skills/repo-harness-session-start/SKILL.md` requires one approval before network access, names `git fetch` and fast-forward-only integration, and requires transport-failure remediation to name an available alternate credential path, but nothing in it specifies a non-interactive invocation. The strings `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY`, and `passphrase` appear nowhere in the generated skills or in the roadmap. The reviewed external harness states the same obligation, states that exactly this configuration exists on one of its two machines, and states that it has not fired only because its provider path keeps winning first. |
| Remaining work | none |
| Additions | A non-interactive transport contract in the generated session skills, with the shell-portability hazards stated where the command is given. |
| Exclusions | No provider tooling requirement, no change to the approval threshold that `RH-W-0046` owns, no new dependency, no credential storage, no automatic integration on divergence, and no change to the existing outcome model. |
| Deferrals | none |
| Rejections | Rejected leaving these findings inside the `RH-W-0056` document. A defect owned by a document and not by the roadmap is the exact shape that review named as its second structural insight: a note is not a control. |
| Evidence | Verified: the generated session-start skill requires approval before network access and names the alternate-credential-path obligation, and no generated skill or roadmap record contains `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY`, or `passphrase`. Verified: this session's own commit and push used the non-interactive form and completed in seconds, which demonstrates the mechanism but not the failure it prevents. Supported inference: an agent session against a passphrase-protected key would hang rather than fail into the banner, resting on the absence of any non-interactive requirement plus the reviewed harness's statement of the same failure; reproducing it against such a key would raise it to verified. Human decision: whether the contract names specific flags or states the obligation and leaves the flags to the implementation. |
| Decision history | 2026-08-22: registered as proposed, extracted from the `RH-W-0056` review after the owner asked whether the problem had actually been recorded and it had not. 2026-08-22: the owner approved the reviewed design and both recommendations, implementation ran test-first, and the unit advanced to `completed` in the same session. Registered outside the owner-approved 2026-08-21 execution order; the owner recommended it as the next unit because it is the only reviewed finding with an active defect rather than an improvement, and its scheduling remains an owner decision. |
| Next advancement condition | none. The unit is complete. A future change to Git's prompt-suppression flags would be new work, and this unit's clause 4 documents the two limits that bound it. |

### RH-W-0058
| Field | Value |
|---|---|
| Origin | Owner correction on 2026-08-22 after a session buried its one pending question at the end of a long reply, where it could not be found. Corroborated the same day by finding `F1` of the RH-W-0056 review, which states the identical rule in the reviewed harness. |
| Priority | P2: no state is at risk and no output is wrong, but a question the human cannot find is a question that was never asked, and the session then either stalls or proceeds on an assumption the human never made. |
| Dependencies | none |
| Status | proposed |
| Completed work | Measured the defect in this repository's own contracts rather than assuming it. The generated session-start Output Contract requires that a safe pending approval be the single concrete question on the final user-facing line, and the session-stop and documentation contracts carry their own output shapes, but no generated contract requires a notice, pending action, or question to be visually separated from surrounding prose. "Final line" is a position rule; it is not a visibility rule, and a final line at the end of a long reply is not findable. |
| Remaining work | Design the visibility obligation and where it belongs: a rulebook hard rule, a shared clause in all three daily Output Contracts, or both. Decide whether it binds only the generated contracts or also the agent's own replies, since the reviewed harness states its version binds skills and chat alike. Decide the permitted forms so the rule stays mechanical enough to follow and does not become a second formatting style guide. Then implement test-first and synchronize the projections. |
| Additions | One visibility obligation covering notices, pending actions, and questions across the generated daily contracts. |
| Exclusions | No visual style system, no banner templates copied from the reviewed harness, no change to what the contracts require to be reported, and no change to the existing single-final-question rule, which this strengthens rather than replaces. |
| Deferrals | none |
| Rejections | Rejected treating this as a presentation preference. The reviewed harness registers it as a working agreement with the reason attached, and the failure it prevents is a blocked session that looks like a delivered one. |
| Evidence | Verified: the generated session-start contract requires a final-line question and no generated contract requires visual separation. Verified: the reviewed external harness states the rule as "a warning, a pending action or a question goes in a block that survives skimming: banner, box, bold, list, never inside a paragraph", and scopes it to skills and chat alike. Verified: the owner reported the failure from a real reply in this repository on 2026-08-22. Human decision: whether the obligation binds the agent's own replies as well as the generated contracts. |
| Decision history | 2026-08-22: registered as proposed at owner instruction, after the owner named the defect in a live session and asked for it to be recorded. Extracted from finding `F1` of `RH-W-0056` rather than left inside that document, because the reviewing session violated the rule in the same turn that found it, which is evidence that a finding recorded in a document is not yet a contract. Registered outside the owner-approved 2026-08-21 execution order; its scheduling remains an owner decision. |
| Next advancement condition | Advance after a written design names the obligation's exact wording, the contracts it lands in, its scope over generated output and agent replies, and the permitted forms, with focused tests that fail before implementation. |

### RH-W-0057
| Field | Value |
|---|---|
| Origin | Owner question on 2026-08-22 about recovering a deleted `repo-harness-help` command, which exposed that no in-repo control detects the deletion at all. |
| Priority | P2: the file is recoverable from Git or a builder re-run, but a consumer repository has no local detection, so a missing generated command stays silent until a human happens to notice. |
| Dependencies | none |
| Status | proposed |
| Completed work | Measured the gap instead of inferring it. `.repo-harness/check.py` defines `SKILL_SURFACES` with exactly two entries, both `skills/{name}/SKILL.md`, and `check_projections()` iterates only `manifest["canonicalSkills"]`, so no command or prompt path is ever compared. The 2026-08-22 run reported six files and `current` with zero writes. `harness_tool.py:2321-2325` proves the builder does own `{base}/commands/repo-harness-help.md` and the per-runtime command wrappers, and `check_script()` states that the builder is not installed in the repositories it builds. Found the root cause the same day: `.repo-harness/` contains `check.py`, `manifest.json`, `rules/`, and `skills/` and no `commands/`, so the three wrappers named by `COMMAND_TO_SKILL` plus `repo-harness-help` have no canonical source inside the harness at all. |
| Remaining work | Design the canonical command source layout under `.repo-harness/` first, then the compared wrapper set and the reported states, then implement test-first. The owner set the direction on 2026-08-22: canonical commands belong in `.repo-harness/` beside the skills, which gives every wrapper a canonical peer and lets the checker compare content the same way it already compares skill projections, instead of adding a wrapper digest to the manifest. The design must decide the manifest field that declares canonical commands, whether the Pi prompt surface is covered, and how an installed harness without that field is treated. |
| Additions | Canonical command sources under `.repo-harness/` and command wrapper coverage in the generated read-only checker, so a consumer repository can detect a generated command that no longer exists or no longer matches its canonical source. |
| Exclusions | No writes from the checker, no repair or regeneration mode, no change to skill projection comparison, and no wrapper digest field in the manifest if a canonical peer file makes it unnecessary. |
| Deferrals | none |
| Rejections | Rejected treating Git tracking as sufficient detection, because the harness must also report the case where the deletion was committed, and rejected treating builder availability as detection, because the builder is absent from every repository it builds. |
| Evidence | Verified: the two `SKILL_SURFACES` entries, the `canonicalSkills`-only loop, the six-file `current` result, the builder-owned wrapper paths at `harness_tool.py:2321-2325`, a `.repo-harness/` tree with no `commands/` directory, and a manifest whose fields are `canonicalSkills`, `generator`, `integrationMode`, `language`, `project`, `runtimes`, `schemaVersion`, and `writeSemantics` with no wrapper digest. Supported inference: re-running the builder recreates a deleted wrapper as a create action, resting on that same generated-path set; a read-only render preview against a repository with the file removed would raise it to verified. Human decision: whether wrapper content drift is in scope or only absence. |
| Decision history | 2026-08-22: registered as proposed after the owner asked whether a deleted `repo-harness-help` command is recoverable. The premise that the command lives inside the builder was disproved by reading both wrappers, which reference only the usage guide, but the question surfaced this separate real gap. 2026-08-22: the owner identified the root cause and set the direction, that `.repo-harness/` currently holds only canonical skills and must also hold the canonical commands, and scheduled this unit next after the active `RH-W-0054` without changing the rest of the approved 2026-08-21 order. |
| Next advancement condition | Advance after a written design defines the canonical command layout under `.repo-harness/`, its manifest declaration, the compared wrapper set, the reported states, and the treatment of installs that predate the field, with focused tests that fail before implementation. |

### RH-W-0056
| Field | Value |
|---|---|
| Origin | Owner request on 2026-08-22 to review his own external harness, referred to here as `external harness 1`, and extract generically reusable ideas for any repository. The alias follows the `RH-W-0046` precedent: the reviewed repository belongs to the owner, and its identity is not this repository's to record. |
| Priority | P2: it is an idea source rather than a defect, but this repository has no comparative evidence base for cross-OS, transport, or runtime-surface decisions and currently designs each one from first principles. |
| Dependencies | none |
| Status | active |
| Completed work | Confirmed the owner owns the reviewed repository, that the review is read-only, and that no existing roadmap record covers a comparative external harness analysis. Performed the bounded read-only inspection on 2026-08-22 and delivered the bilingual [findings document](rh-w-0056-findings.md) with 94 patterns across seven groups, each carrying its evidence class and a recommendation: 64 `integrate`, 14 `inspire`, 16 `already present`, 0 `reject`, and 0 `out of scope`. The first pass covered roughly a third of the harness and stopped at a bound this work unit invented rather than one the owner set; the owner rejected it, and the completed review then read the whole harness surface: the root rulebook and all three root pointer files, the six gated rule files, all six pack skills with every reference and banner asset, the control index, every control docstring plus the two largest control implementations in full, the ignore and attribute files, the pack installation guide, the generated skill registry, and the product README's conventions and session-command sections. Deployment target references, audit and backup workflow bodies beyond their rules and hazards, and business documentation, notebooks, and application code remain unread as domain operations detail, and the document states that no claim and no absence claim rests on them. No project identity, absolute path, host, credential, or domain datum was carried into this repository. |
| Remaining work | Obtain the owner decision on the 64 `integrate` recommendations and register the accepted ones as their own work units. The document proposes seven natural groupings: transport hardening, command projection folding into `RH-W-0057`, control discipline, documentation invariants, evidence and claim discipline, authority and artifact identity, and session lifecycle. `G1` through `G3` bear on the active `RH-W-0054` and should be read before its Task 2. `F1` was extracted into its own registered unit `RH-W-0058` because the reviewing session violated it in the same turn that found it. No adoption is authorized by the document itself. |
| Additions | One findings document plus a reconciliation that classifies every finding as integrate, inspire, reject, or out of scope, using the vocabulary the completed `RH-W-0050` already established. |
| Exclusions | No mutation of the reviewed repository, no secrets, credentials, absolute paths, host names, or machine-specific values copied into this repository, no adoption of any pattern without its own separate unit, and no second backlog competing with this roadmap. |
| Deferrals | none |
| Rejections | Rejected adopting any reviewed pattern directly, because an adoption needs its own design, tests, and delivery evidence, and rejected treating the reviewed repository as authority over this product's boundaries. |
| Evidence | Verified: `docs/repo-harness/` already owns `integration-research.md`, `i18n-research.md`, and `contribution-workflow-research.md`, so a findings document is an established artifact type here. Verified: no roadmap record covers external harness comparison, and the nearest units `RH-W-0022`, `RH-W-0055`, and `RH-W-0046` plus the deferred ATL-like inventory address different scopes. Verified: the owner stated the reviewed repository is his own and authorized a read-only review. Human decision: which findings are adopted, inspired, or rejected. |
| Decision history | 2026-08-22: registered as proposed at owner request after confirming no existing owner for the scope. Registered outside the owner-approved 2026-08-21 execution order; its scheduling remains an owner decision. 2026-08-22: the owner authorized execution the same day; the bounded read-only inspection ran and the bilingual findings document was delivered, advancing the unit to `active` with the adoption decision still pending. |
| Next advancement condition | Advance after the read-only review produces the findings document and every finding carries an explicit integrate, inspire, reject, or out-of-scope classification with its evidence class. |

### RH-W-0055
| Field | Value |
|---|---|
| Origin | Owner note requesting copy/paste installation guidance for Claude Code, OpenCode, and Pi, connected to builder invocation and daily session use. |
| Priority | P2: installation is possible from a release package, but the repository does not provide one coherent consumer path from artifact selection through daily operation. |
| Dependencies | RH-W-0050 for evidence classification in onboarding claims. The published package already provides stable contents, so the release asset naming contract is an addition this unit owns rather than a dependency it waits on. |
| Status | proposed |
| Completed work | Grouped copy/paste installation, future release asset naming, and install-to-builder-to-session guidance into one bounded lifecycle unit. |
| Remaining work | Design per-runtime copy/paste commands, supported prerequisites, future release asset naming, verification, upgrade references, and links from installation to builder execution and daily session-start or session-stop usage. |
| Additions | One concise onboarding path for each supported runtime and one stable future release asset naming contract. |
| Exclusions | No release publication, network action, package rename, generated behavior, or claim that a future asset already exists. |
| Deferrals | Actual asset naming and URLs remain unresolved until a release design is approved. |
| Rejections | Rejected scattering installation snippets across unrelated documents or documenting only package extraction without the operational next steps. |
| Evidence | Verified: the supported runtime set includes Claude Code, OpenCode, and Pi. Supported inference: asset naming and installation belong together because copy/paste commands need a stable artifact reference. Unknown: exact future asset name and URL pattern. |
| Decision history | 2026-08-21: registered as proposed from the consolidated portfolio audit; no release or implementation was approved. |
| Next advancement condition | Advance after a written onboarding design names supported installation sources, exact per-runtime commands, future asset naming, offline and failure boundaries, verification, and links through builder invocation to daily session usage. |

### RH-W-0054
| Field | Value |
|---|---|
| Origin | Owner note requiring recovery when the builder gives a wrong answer or the human corrects a material premise after preview. |
| Priority | P1: a stale or incorrect answer can preserve an invalid plan or checkpoint unless the correction explicitly invalidates derived authority. |
| Dependencies | RH-W-0050 evidence classification and the existing digest-bound preview or apply safety model. |
| Status | active |
| Completed work | Separated answer correction from interrupted-session recovery and recorded the owner-approved [stateless correction authority design](rh-w-0054-design.md). Task 1 delivered canonical authority-context parsing and host-JSON validation. Task 2 delivered the deterministic configuration projection and the two pure digest helpers: `expected_authority_builder_config()` derives what the builder would actually run so a context describing a different build cannot authorize this one, `target_binding_sha256()` hashes the normalized destination and discards it so a preview can name its target without disclosing where it lives, and `authority_binding()` moves context, target, and plan identity together so a correction cannot inherit an approval. Task 3 delivered typed repository evidence resolution: a declared repository file is resolved only through the containment guard, required to be a regular non-indirect file, re-read, and compared byte for byte, and the returned binding carries a repository-relative locator and never the bytes or the resolved root. Task 4 delivered the stateless builder preview: three opt-in CLI arguments, a projection that emits stable IDs, kinds, safe locators or sources, content digests, explicit redaction markers, and the four binding digests, and nothing else. The plan contradicted itself here and the correction is recorded in it: requiring a context for every builder activation would have removed the `RH-W-0030` strict-domain boundary and broke four existing tests, so the transport is opt-in. The canonical suite passes 399 tests with 9 skips, one skip environmental because the indirect-path refusal needs a symlink this account cannot create; both plan-compatibility tests still pass; the checker reports `current` with zero writes. |
| Remaining work | Tasks 5 through 8 and delivery remain pending. Runtime projections intentionally remain pending until Task 7. The owner raised a proportionality concern on 2026-08-22, that this is a documentation harness and Tasks 5 through 8 are the expensive half; Task 5 is the one that makes the binding do work, because nothing validates a generation before writing until it lands, and Tasks 6 through 8 are to be reassessed after it. Task 5 is the next candidate boundary but is not authorized by this reconciliation. Preview and apply must eventually independently resolve and re-read supported evidence, enforce exact no-newline canonical bytes with non-finite values rejected, recompute target, outputs, plan, and generation, reject every apply transport or binding mismatch before writes, expose only safe metadata on every output path, preserve maintenance digest-only compatibility, and retain existing final guards. |
| Additions | Exact ASCII-safe canonical authority JSON, typed evidence bindings with SHA-256, separate human decisions, authority and generation digests, opaque target binding, complete configuration binding, host correction-refresh contract, separate-process enforcement, and pre-write fresh-preview routing. |
| Exclusions | No automatic repository mutation, silent correction, reused generation or approval, persistent store, in-memory cross-command session, receipt, ledger, cache, random unverifiable token, hidden authority, absolute target path disclosure, silent rollback, false recovery claim, or transfer of RH-W-0053 or RH-W-0051 scope. Post-apply remediation ownership remains a separate future design decision. |
| Deferrals | none |
| Rejections | Rejected continuing from an invalidated plan or treating a corrected answer as sufficient evidence without re-verification. |
| Evidence | Verified: preview and apply are separate CLI processes and current digest-bound apply rejects changed plans. Owner-approved design: exact canonical context carries authority across those processes; safe typed identifiers remain reviewable; repository-file bytes and canonical host JSON are digest-bound and re-read; target, configuration, plan identity, and material revision bind the generation; corrected identical plans require fresh approval; unsupported or unverifiable evidence fails closed; the CLI cannot detect a correction omitted by the host or verify truth beyond supported bindings. |
| Decision history | 2026-08-21: registered as a separate proposed unit in the later review order. 2026-08-21: the same-day portfolio re-audit superseded that review order and placed this unit first after completed `RH-W-0050`. 2026-08-21: the owner approved the written behavior design, advancing the unit to `ready` with implementation and delivery still pending. 2026-08-21: the owner explicitly authorized Task 1 implementation against the corrected design and focused tests, advancing the unit to `active`. Task 1 then completed locally and passed focused, canonical, spec-compliance, and ordinary quality review. 2026-08-22: the owner authorized Tasks 2, 3, and 4, which completed test-first in the same session, and then raised a proportionality concern about the remaining half; Tasks 5 through 8 and delivery remain pending and Tasks 6 through 8 are to be reassessed after Task 5. |
| Next advancement condition | Advance to `completed` only after implementation of all authorized tasks, required RED/GREEN evidence, projection synchronization, and delivery evidence are verified. |

### RH-W-0053
| Field | Value |
|---|---|
| Origin | Owner note requiring safe recovery from a partial or interrupted session. |
| Priority | P1: unfinished work can be mistaken for completed continuity or resumed from stale assumptions if the interruption is not represented explicitly. |
| Dependencies | RH-W-0050 evidence classification and RH-W-0051 storage semantics for any future harness-owned local checkpoint bytes. |
| Status | proposed |
| Completed work | Bounded the capability to a typed unfinished checkpoint, no automatic mutation, and session-start resumption that re-verifies repository evidence. |
| Remaining work | Design checkpoint schema, authority and storage, creation and expiry triggers, sensitive-data limits, interrupted-step representation, session-start discovery, stale or conflicting checkpoint handling, and explicit resumption behavior. |
| Additions | A typed unfinished checkpoint and safe session-start route that distinguishes completed facts, unfinished intent, supported inference, unknown state, and decisions still requiring a human. |
| Exclusions | No automatic continuation, mutation, approval reuse, claim that an interrupted action completed, duplicate backlog, or storage in user-owned personal paths. |
| Deferrals | none |
| Rejections | Rejected treating a generic session summary as proof of an executable checkpoint or resuming an interrupted mutation without fresh evidence and authority. |
| Evidence | Verified: generated session-start reconciles Engram with repository evidence, but no repository contract defines a typed unfinished checkpoint. Human decision: resumption must be non-mutating until explicit current authority exists. Unknown: final checkpoint owner and persistence shape. |
| Decision history | 2026-08-21: registered as proposed from the consolidated audit and placed first in the later review order. 2026-08-21: the same-day portfolio re-audit superseded that review order and placed this unit sixth overall, immediately after `RH-W-0051`, because it is `P1` and both declared dependencies are satisfied at that point. The status stays `proposed` and the advancement condition is unchanged. |
| Next advancement condition | Advance after a written design defines checkpoint owner, typed fields, evidence classes, storage and retention, sensitive-data handling, stale and conflict outcomes, no-mutation startup behavior, and explicit resumption authority. |

### RH-W-0052
| Field | Value |
|---|---|
| Origin | Approved VPS findings reconciliation: an existing non-Git information folder without a harness exposed a need for explicit uninstall, archive, and reinstall behavior. |
| Priority | P2: recovery and replacement can destroy useful local information if ownership and manifests are not proved first. |
| Dependencies | RH-W-0051 storage semantics and proven RH-W-0048 additive recovery primitives, plus an approved manifest-ownership design. |
| Status | proposed |
| Completed work | Classified the failed VPS prototype as evidence for a separate capability, not as an implementation, and fixed the fixture boundary. |
| Remaining work | Design manifest-aware inventory, archive, uninstall, reinstall, rollback, non-Git behavior, and explicit approval boundaries. |
| Additions | Identify harness-owned bytes, archive recoverable state, uninstall only proved owned bytes, and reinstall from authoritative material. |
| Exclusions | No promotion of the failed prototype, automatic deletion, assumption of Git, fixture mutation, or merged implementation with RH-W-0048. |
| Deferrals | none |
| Rejections | Rejected treating the failed prototype as shipped behavior or proof that destructive cleanup is safe. |
| Evidence | Owner-approved reconciliation decision; the current repository implements no uninstall, archive, or reinstall command. |
| Decision history | 2026-08-20: registered as a separate proposed capability. RH-W-0048 remains proposed and separate. |
| Next advancement condition | Advance only after RH-W-0051 storage semantics and RH-W-0048 recovery primitives are proven, and a written design proves ownership from a manifest, defines archive retention and explicit deletion approval, covers non-Git folders and workspaces, and specifies rollback and reinstall verification. |

### RH-W-0051
| Field | Value |
|---|---|
| Origin | Approved owner decision separating user-owned personal storage from harness-owned local state. |
| Priority | P2: local state needs predictable ownership and clean-worktree semantics before recovery or archival behavior can depend on it. |
| Dependencies | none |
| Status | proposed |
| Completed work | Documented the target architecture under `.repo-harness/`: `tmp` is disposable and `archive` is recoverable, both non-authoritative and ignored by default, with explicit repository overrides. |
| Remaining work | Jointly design the shared storage and recovery architecture with RH-W-0048 and RH-W-0052, including non-Git folders and workspaces, then implement render, ignore, checker, manifest, override, migration, and verification behavior as this separate unit. The same design reserves namespace for the other declared consumers of this contract, the RH-W-0053 checkpoint bytes, the durable repository-scoped consent or preference store required by RH-W-0046 and RH-W-0018, and the RH-W-0040 violation record location, without implementing or advancing any of them. |
| Additions | Default local namespaces with explicit ownership, retention, clean-worktree, and non-Git workspace rules. |
| Exclusions | No claim that either directory currently exists or is supported; no ignore, render, checker, manifest, or migration behavior in this documentation work. |
| Deferrals | none |
| Rejections | Rejected using user-owned personal storage for harness state and silently applying repository overrides. |
| Evidence | The canonical generator currently defines no supported `tmp` or `archive` namespace behavior; the standing decision records the approved target policy. |
| Decision history | 2026-08-20: architecture approved and implementation retained as proposed work. |
| Next advancement condition | Advance after a written design defines exact generated paths, ignore integration, checker treatment, manifest representation, explicit override semantics, migration, and tests. |

### RH-W-0050
| Field | Value |
|---|---|
| Origin | Completed lifecycle review after PR #82 found unresolved findings, stale prerequisites, missing delivery evidence, duplicate localized continuity, and no post-delivery reconciliation step. |
| Priority | P1: unreconciled findings and stale projections can route the next session from false repository state. |
| Dependencies | none |
| Status | completed |
| Completed work | Approved the responsibility, classification vocabulary, and Engram design corrections; completed direct external review; and obtained owner approval of the corrected [bounded reconciliation design](rh-w-0050-design.md) at commit `38cbedc`. Implemented the generated start, stop, documentation, integrated-memory, and standalone contracts test-first; synchronized canonical, self-hosted, OpenCode, and Claude projections; documented architecture and validation obligations; passed 341 tests with 8 skips; and delivered the result through approved issue #89 and merged PR #90 at merge commit `1bebc027ba61b95088be1acc4966b120d9cd9b5e`. Post-delivery reconciliation found no unresolved retained finding or projection drift. |
| Remaining work | none |
| Additions | Inventory findings; classify integrate, close, defer, reject, or noise; discover relevant pre-existing Engram observations under explicit project scope; report project resolution, local read, local write, prompt association, conflict discovery, lifecycle review, plugin capture, and sync outcomes independently when applicable; preserve a successful observation save when best-effort prompt capture or post-save candidate discovery fails; treat conflict candidates and `judgment_required` as advisory discovery rather than proof of contradiction; interpret `active` and `needs_review` only as computed review-age labels based on `review_after`; require evidence before claiming success from OpenCode plugin capture, auto prompt capture, compaction handoff, or passive capture; reconcile repository and memory evidence without automatic mutation; promote only with approval; clean stale or duplicate continuity; and reconcile lifecycle after delivery before recommending the next ready unit. |
| Exclusions | Engram does not own backlog, approval, delivery, classification, reconciliation, or canonical truth. No aggregate available/degraded/absent Engram verdict, automatic promotion, deletion of historical evidence without approval, duplicate backlog, cumulative reconciliation document, new framework, store, ledger, SDD requirement, or implementation of RH-W-0048, RH-W-0051, or RH-W-0052. The verified current SQLite local-first implementation is not a required product dependency. |
| Deferrals | none |
| Rejections | Rejected treating continuity as only remembering or naming one next item, and routing work before findings are classified. |
| Evidence | The 2026-08-20 review found unresolved findings and stale projections. The corrected design was approved at `38cbedc`; five focused regressions failed before implementation and passed afterward; the canonical suite passed 341 tests with 8 skips; the checker reported `current` with zero writes; issue #89 carried approval; and PR #90 merged as `1bebc027ba61b95088be1acc4966b120d9cd9b5e`. |
| Decision history | 2026-08-20: owner approved finding and post-delivery reconciliation as a harness responsibility. 2026-08-21: owner approved the Engram authority, project-scope, operation-outcome, conflict-candidate, review-age, capture-evidence, and implementation-detail corrections. 2026-08-21: direct final review completed and the owner approved the corrected design at `38cbedc`, advancing the unit to `ready`. 2026-08-21: owner authorized implementation through issue #89; test-first implementation advanced the unit to `active`; merged PR #90 and convergent post-delivery reconciliation advanced it to `completed`. |
| Next advancement condition | Reopen only if bounded reconciliation, finding disposition, operation-level continuity outcomes, standalone degradation, projection parity, or post-delivery convergence regresses. |

### RH-W-0049
| Field | Value |
|---|---|
| Origin | Observed in this repository's generated session-start workflow on 2026-08-20. After an approved fetch established `main` at 0 ahead and 0 behind, the assistant reported synchronization and one recommended unit, then stopped. When the owner asked to continue, the assistant treated that conversational prompt as authority to prepare `RH-W-0047` implementation and design rather than discuss current state, options, priorities, and tradeoffs. The owner corrected the boundary explicitly: talk first, propose what follows, and do not launch work. |
| Priority | P2: no repository state was changed and the owner could correct the turn immediately, but the generated workflow ends a successful synchronization at a terminal status plus one isolated recommendation, which withholds the context needed for a human choice and makes an ordinary continuation prompt easy to misread as authorization for unrequested work. |
| Dependencies | none |
| Status | completed |
| Completed work | Added a bounded generated session-start continuation contract. After status and synchronization it recovers current evidence, explains only the one or two material tensions, reviews relevant pending work, state, and priorities, recommends one reasoned direction, and mentions a second path only when materially relevant. Ordinary continuation remains discussion and steering; preparation, design, delegation, implementation, mutation, or delivery begins only when explicit human intent identifies both the action and target. The contract judges intent semantically and rejects backlog dumps, fixed menus, multi-question rounds, exhaustive interviews, phrase allowlists, automatic planning, and automatic document writes. A focused semantic regression failed on eight missing obligations before implementation and passed after the minimal source change. The repository-owned maintenance renderer synchronized the canonical self-hosted skill and both runtime projections in one digest-bound apply. |
| Remaining work | none |
| Additions | A conversation-first post-synchronization continuation that helps the human understand current state, relevant options, priorities, tradeoffs, and the recommended direction before asking what should follow; explicit protection that conversational continuation does not authorize task, design, implementation, or delegation launch. |
| Exclusions | No automatic task launch, no mandatory implementation proposal, no fixed multi-option menu, no removal of the concise startup status, no change to Git synchronization or approval semantics, and no requirement that session-start itself complete later work. No external or consumer repository is modified. |
| Deferrals | none |
| Rejections | Rejected treating the terminal report as sufficient merely because it names one next step: one recommendation without comparative context does not help the human choose. Rejected interpreting `continue`, `let us continue`, or equivalent conversational wording as launch authority. Rejected auto-launching the recommended unit, which would violate the rulebook prohibition on continuing into unrequested tasks. |
| Evidence | The generated session-start contract requires identifying completed work, remaining work, constraints, and one next recommended step at `skill/repo-harness-builder/scripts/harness_tool.py:760`, while its equal-state Git row says only to report synchronization healthy and continue at line 775. Its Output Contract at line 787 ends with exactly one next recommended step and defines no post-sync conversational handoff, comparative option framing, or distinction between continuing discussion and authorizing work. The live sequence supplied by the owner is corroborated by Engram observations `#9963` and `#9964`: after synchronization the report stopped; the owner's `si ok sigamos, xq te quedas ahi.?` was overread as authority to prepare `RH-W-0047`; the owner then corrected `lanzar el trabajo no, hablar conmigo, ver que sigue, proponer`. `REPO-HARNESS.md:20` already prohibits automatic continuation into unrequested tasks, so the missing rule belongs in the generated session-start behavior rather than as another root rule. |
| Decision history | 2026-08-20: registered `proposed` at P2 as delegated direct documentation work. The owner defined the desired boundary and later authorized review and implementation. Completed through direct delegated work with strict RED/GREEN evidence and projection synchronization. Git-local delivery evidence: implementation parent `40eb1533369fcb2de71ea45f8739a90ced14e307` and merge commit `017df90af0a6fc1bb38678e55dd23adefdc3e6c8`; the local merge subject identifies pull request #76. Engram observation #9965 corroborates issue #75 and PR #76, but no Judgment Day artifact is claimed. |
| Next advancement condition | none |

### RH-W-0048
| Field | Value |
|---|---|
| Origin | Owner decision clarified on 2026-08-20: a fresh clone may have no `.claude/`, `.opencode/`, or `.pi/` folders, while another clone may have those folders but lack required skills, commands, wrappers, or other generated runtime surfaces. Recovery must work from the installed `.repo-harness` without locating or installing the external repo-harness-builder skill again. Runtime trees are local, minimally generated, and Git-ignored by default, while a repository may explicitly request tracking them. |
| Priority | P2: nothing is destroyed and the local checker is green for the user who rendered, but a supported configuration produces provably inert output and an unexplained non-zero exit for anyone who clones. |
| Dependencies | RH-W-0051 storage semantics. |
| Status | proposed |
| Completed work | Established that the builder has no way to express this configuration, and measured the consequences instead of arguing them. `MANIFEST_FIELDS` at `skill/repo-harness-builder/scripts/harness_tool.py:1670` is exactly `schemaVersion`, `generator`, `project`, `runtimes`, `canonicalSkills`, `writeSemantics`, `integrationMode`, `language`, and `project` is validated at line 1734 as exactly `id`, `ignoreCodegraph`, `rulebook`. No field records how runtime projections are tracked. `--standalone` is not this switch. The owner has now fixed the product boundary: the installed `.repo-harness` must carry enough authoritative local material or inventory to detect and restore the minimum runtime projections without the external builder skill. The generated checker is the preferred owner for detection and recovery UX. Plain `python .repo-harness/check.py` remains read-only and must continue reporting `writes: 0`; mutation requires a future explicit repair or bootstrap invocation whose final command name remains undecided. That invocation handles both entirely absent runtime folders and partially missing generated contents. |
| Remaining work | Jointly design the shared architecture with RH-W-0051 and RH-W-0052, including non-Git folders and workspaces, then separately decide the bootstrap command name, ownership boundaries, exact trigger, authoritative inventory or templates stored inside `.repo-harness`, minimum file set for each declared runtime, tracked-signal detection, handling of stale versus missing files, and result or report schema. Decide separately whether `.gitattributes` omits rules for local-only paths. |
| Additions | Local, minimally generated, ignored projection trees by default; an explicit repository choice to track them; authoritative in-repository recovery material; checker-owned detection and explicit recovery UX in Git and non-Git workspaces; and bounded additive creation of only missing required skills, commands, wrappers, and other declared-runtime files. Repair preserves unknown or human-evolved bytes, never stages files, and keeps runtime trees Git-ignored by default. |
| Exclusions | No mutation from plain checker execution and no requirement to reinstall or locate the external builder skill for recovery. No mandatory tracked/local manifest mode unless implementation evidence proves it necessary. No overwrite of stale, unknown, or human-evolved bytes under the missing-file repair contract. No silent reclassification of a projection explicitly tracked in Git but missing: that remains drift or error and must not become local-only. No change to `.repo-harness/` ownership, path safety, or exact-byte validation after bootstrap. No consumer repository is modified. |
| Deferrals | none |
| Rejections | Rejected proposing that the consumer commit its runtime projections. Rejected treating appended LF rules as a defect in isolation. Rejected inventing a mandatory tracked/local manifest field without implementation evidence. Rejected treating a tracked but missing projection as local-only, because explicit tracking makes its absence real drift. |
| Evidence | `references/architecture.md:37` states the generated checker compares each canonical skill against its `.claude/` and `.opencode/` projections and exits non-zero on a missing copy. Line 39 states root `.gitattributes` carries one `text eol=lf` rule per byte-compared generated path, explicitly including runtime projections and wrappers, with the stated rationale that without a rule a CRLF checkout reports drift. That rationale cannot apply to a path Git never checks out. Measured on the real consumer, read-only: `git check-ignore -v` resolves `.claude/skills/repo-harness-session-start/SKILL.md` to `.gitignore:31` and `.opencode/commands/session-start.md` to `.gitignore:30`, and `git ls-files` returns zero tracked paths under either directory, so 14 of the 19 appended rules can never apply. Proved by execution rather than inference: a simulated fresh clone built from only the tracked harness paths ran the installed checker and returned `status: drift` with exit 1 and all six projections `missing`. Recovery has no in-repo path, because the installed checker has no argparse, no options, and no repair mode, and the builder skill itself sits under the same ignored `.claude/` tree. |
| Decision history | 2026-08-20: found while analyzing the applied `consumer 2` harness at the owner's request. The owner later clarified the local-by-default model, explicit tracked exception, both whole-folder and partial-content recovery cases, installed-harness self-sufficiency, checker ownership preference, read-only plain-check contract, explicit additive repair, and tracked-missing drift boundary. The unit remains `proposed` because the operational recovery design is not complete. |
| Next advancement condition | Advance only after RH-W-0051 defines storage semantics and a written scope design decides the bootstrap command name, ownership boundaries, exact trigger, authoritative `.repo-harness` inventory or templates, minimum file set per runtime, Git and non-Git behavior, tracked-signal detection, stale-versus-missing handling, and result or report schema while preserving read-only plain checks and tracked-missing drift. |

### RH-W-0047
| Field | Value |
|---|---|
| Origin | Read-only audit of the real prior-era consumer on 2026-08-20, authorized by the owner while he reviewed the builder plan before approving it |
| Priority | P2: nothing is destroyed and no apply fails, but the builder appends a line to a human-owned file to satisfy an intent the repository already satisfies more precisely, and it reports that file as `directive-missing` while saying so. |
| Dependencies | none |
| Status | completed |
| Completed work | Added `has_codegraph_ignore_line()` as a bounded wrapper over exact-line matching and routed only `.gitignore` planning and checking through it. It accepts exactly `.codegraph/` and `/.codegraph/`; shared `has_pointer_line()` remains unchanged for `CLAUDE.md`, `AGENTS.md`, and `.gitattributes`. Focused regressions cover both accepted forms, comments, negations, leading and trailing whitespace, missing trailing slash, unrelated near matches, byte preservation, checker classification, and unchanged exact pointer behavior. RED failed only because `/.codegraph/` still planned an append; GREEN passed after the minimal helper. Canonical source and both package projections were synchronized without changing dependencies, manifest schema, default policy, opt-out, or append behavior. |
| Remaining work | none |
| Additions | An ignore-specific directive-satisfaction rule that accepts `.codegraph/` and `/.codegraph/`, plus positive regressions for both forms and negative regressions for comments, negations, and unrelated near matches. |
| Exclusions | No change to the byte-preserving append itself, to the root pointer directives in `CLAUDE.md` and `AGENTS.md`, or to `.gitattributes` conflict detection, which `RH-W-0042` already made combination-aware. No consumer repository is modified. No general-purpose gitignore pattern engine; the scope is the one directive this builder owns. |
| Deferrals | none |
| Rejections | Rejected loosening `has_pointer_line()` for every caller at once. Rejected comments, negations, trailing-slash-less forms, and unrelated near matches as satisfying the initial equivalence set. Rejected redesigning `ignore_codegraph=True`, manifest `project.ignoreCodegraph`, or explicit `--no-codegraph-ignore`, which already implement the intended local-state default and opt-out. |
| Evidence | `CODEGRAPH_IGNORE_DIRECTIVE = ".codegraph/"` at `harness_tool.py:36`. The classification path at `harness_tool.py:2035-2037` selects that directive for `GITIGNORE_PATH` and routes on `has_pointer_line(current, directive)`, sending the file to `append` when it returns false. Verified on the real consumer, which was clean with 0 dirty files and had not applied the plan: `grep -n codegraph .gitignore` returns `29:/.codegraph/`, and the plan nonetheless listed `.gitignore` under both `directive-missing` and `append`. This is the same defect class as `RH-W-0042`, a literal comparison blind to an existing form that is equivalent or stronger; there the sibling `eol=lf` was ignored, here the leading `/` is. The direction differs in the consumer's favor: a leading slash anchors the pattern to the repository root while the builder's bare form matches at any depth, so the existing rule is the more precise one. Delivery evidence: closed [issue #77](https://github.com/LCubero/repo-harness-builder/issues/77), merged [PR #78](https://github.com/LCubero/repo-harness-builder/pull/78), implementation commit `22b0fb01d05bc4d4f3d2cdcb26b5c88564275516`, and merge commit `34b1ac65d3a6523e03575240b6a073034322969e`. Judgment Day ended `APPROVED` with 0 severe findings and 0 corrections. Both judges corroborated one informational `WARNING`: `bytes.splitlines()` accepts control-character separators that Git does not treat as line endings. Final verification reported checker `current` with 0 writes and local `main` synchronized and clean. |
| Decision history | 2026-08-20: found while auditing the consumer at the owner's request. Registered `proposed` because the helper was shared and the equivalence set was undesigned. The owner later fixed the initial set at `.codegraph/` and `/.codegraph/`, excluded comments, negations, and near matches, preserved exact pointer behavior, and confirmed the existing default and opt-out surfaces. Advanced to `ready`, then completed through delegated direct work with strict RED/GREEN evidence and no consumer, network, schema, dependency, or policy change. Delivered through issue #77 and PR #78 after the terminal Judgment Day approval. |
| Next advancement condition | Reopen only if either accepted form plans a redundant append, a rejected form is treated as present, or shared pointer and attribute matching stops requiring an exact line. |

### RH-W-0046
| Field | Value |
|---|---|
| Origin | Owner observation on 2026-08-20 that a `/session-start` run in this repository ends on the same `git fetch` approval question in every session |
| Priority | P2: no behavior is wrong and no state is at risk, but the contract spends its one mandated final question, on every clean session, on a read-only operation the owner has never declined. |
| Dependencies | none |
| Status | proposed |
| Completed work | Confirmed the recurrence is structural rather than an execution slip, and bounded what is not in question. The obligation is unconditional and the generated session contract carries no consent-persistence surface anywhere, so every clean session with a usable upstream must ask again. The dirty, detached, missing-upstream, ambiguous-remote, diverged, and transport-failure rows were read and are correct as written; none of them is the subject of this unit. |
| Remaining work | Design the scope boundary before any implementation. Decide whether a read-only `git fetch` is separable from the network-access threshold at all, where a durable per-repository consent would live given that the harness stores no session state today, and how such consent stays visible and revocable. Reconcile the two thresholds the contract already carries in the same design: line 740 requires approval before an external mutation while line 742 requires it before network access, and `git fetch` satisfies only the second because it writes nothing on the remote. Design the durable consent store jointly with RH-W-0018, which requires the same repository-scoped preference primitive, and consume the namespace reserved by RH-W-0051 rather than inventing a second location. Implementation stays separate and RH-W-0018 keeps its own status and advancement condition. |
| Additions | Either a narrower threshold that classifies read-only fetch as non-mutating network access, or a durable and revocable per-repository consent recorded where the human can see and withdraw it. |
| Exclusions | No change to any integration approval. Fast-forward-only integration, push, commit, publication, and every destructive path keep their own separate approval. No weakening of the dirty, detached, missing-upstream, ambiguous-remote, diverged, or transport-failure rows, and no change to the two-attempt transport bound. |
| Deferrals | none |
| Rejections | Rejected deleting the approval outright. The generated contract installs into every consumer repository, so a harness that fetches silently on session start would take network access in repositories whose owner never granted it, which is a larger defect than the friction it removes. |
| Evidence | The obligation is at `skill/repo-harness-builder/scripts/harness_tool.py:742`, `A clean worktree and index with a usable upstream requires one conversational approval before network access`, with no condition and no persistence. The clean-with-upstream row at line 774 restates it as `keep network approval mandatory`. The Output Contract at line 787 then requires a safe pending approval to be the single final user-facing line, so the question is not merely permitted but mandated into the most prominent position of the report. Observed live on 2026-08-20: this repository was clean, `main` tracked `origin/main`, the session ended on the fetch question, and the approved fetch reported 0 ahead and 0 behind. The recurrence across earlier sessions is owner-reported and is not recorded by any repository artifact; an Engram search for prior fetch-approval observations returned none. The unconditional rule with no persistence surface is what explains the report. |
| Decision history | 2026-08-20: the owner reported the repeated question, approved the fetch for that session, and asked explicitly that the observation be documented for later rather than acted on immediately. Registered `proposed` rather than `ready` because the change moves the harness's network-consent boundary in every repository it installs into, which is the whole risk. |
| Next advancement condition | Advance once a written scope design decides whether read-only fetch leaves the network-access threshold or instead gains durable consent, and names where that consent is stored, how it is shown, and how it is withdrawn. |

### RH-W-0043
| Field | Value |
|---|---|
| Origin | Measured in this repository on 2026-08-19 while reviewing the owner-run `v3.1-rc.4` plan, then reproduced in isolation on a synthetic install so the finding depends on no consumer |
| Priority | P0: a builder version bump freezes every installed harness. Planning against a bumped `VERSION` offers zero replaceable paths, and the freeze includes the checker and manifest that would repair it, so no installation can ever receive a newer harness. |
| Dependencies | none. `RH-W-0010` detects the condition this unit makes actionable, and either may land first. |
| Status | completed |
| Completed work | Separated two regimes by experiment rather than by reading the gates and reasoning about them. A clean install rendered at builder `VERSION` 3.0 was planned three times. Unchanged: 0 replacements and 33 identical, which is correct. Checker template changed with `VERSION` unchanged: the first pass offers exactly `.repo-harness/check.py`, and after writing only that file the second pass sees the full set of 19 owned paths. `VERSION` bumped to 3.1 with nothing else changed: `replace` is empty and 28 paths fall to `existing`, including the checker and the manifest. Implemented on 2026-08-19. Before writing any fix, one more experiment found the trap in the obvious repair: relaxing the version gate alone makes the checker replaceable while the manifest stays outside the narrowed owned set, so the new checker embeds the digest of a manifest nobody wrote and the installed checker dies with `ValueError: manifest digest mismatch`. That is worse than freezing, and it is why the fix has two halves rather than one. |
| Remaining work | none |
| Additions | Two conditions and their regressions. `plan_changes` gates replacement on builder provenance rather than exact version equality, and `manifest_owned_generated_paths()` lets an authentic installed checker authorize the whole manifest-owned set instead of only itself, so the checker and its manifest move together. Five behavioral tests: a version bump offering a complete one-pass update, the installed checker still working after that update, a generated path stripped of provenance still refused, a forged checker digest still authorizing nothing, and the same-version case not regressing. The product contract prose was corrected in the same unit: `SKILL.md`, `references/architecture.md`, `references/preservation-and-safety.md`, and `references/validation.md` all declared the superseded current-version rule, and the last of them now also states that an update completes in one pass and that a two-pass repair is never presented as an update. |
| Exclusions | No change to path safety, digest binding, apply atomicity, rollback, or which paths an authenticated manifest owns. No change to human-owned classification. No upgrade daemon, no new abstraction, and no consumer repository is touched. The generated consumer checker is unchanged, so `packageFreshness` stays a builder-side field. |
| Deferrals | none |
| Rejections | Rejected accepting the two-pass repair as the answer. It was proposed in conversation, the owner refused it, and it is not even a workaround for the version-bump case, which offers zero passes. Rejected patching any consumer to unblock the builder, and rejected bumping `VERSION` before this lands, because that would freeze every existing installation. |
| Evidence | The first gate is in `plan_changes` at `skill/repo-harness-builder/scripts/harness_tool.py:2077`, which requires `generated_version(target) == VERSION` before a generated path may be replaced. That condition demands the installed file already carry the current version, which an installed file cannot do during an upgrade, so the builder can repair same-generation drift but can never update across versions. The second gate is `manifest_owned_generated_paths()` at line 1976, which narrows the owned set to `.repo-harness/check.py` alone whenever the installed checker is not byte-equal to the current template. `check_script()` embeds the current version in its own header at line 1657, so a version bump changes the checker bytes and trips both gates at once. Measured on a synthetic install: unchanged plan 0 replace and 33 identical; template drift at the same version 1 replace, then 19 owned paths on the second pass; `VERSION` 3.0 to 3.1 with `replace` empty, 28 `existing`, 0 `blocked`, and `manifest_owned_generated_paths()` returning exactly `.repo-harness/check.py`, which then fails the version condition itself. The legacy `migrate-generated-skills` action bypasses both gates by replacing checker and manifest inside its own atomic transaction, which is why a legacy skill-name migration still advances and why this freeze was not visible before. Completion evidence: the two decisive regressions failed RED with `'.repo-harness/manifest.json' not found in set()`, proving nothing at all was offered before the fix. After it, a bumped-version plan offers exactly 19 replacements covering the manifest, the checker, all three canonical skills, their six projections and all eight command wrappers, with 0 blocked; applying that single plan leaves the installed checker exiting 0 and reporting `current` with 0 writes, which is the trap closed by execution rather than by argument. The `RH-W-0036` narrow contract test was updated rather than deleted, because this unit supersedes it deliberately. Canonical, Claude and OpenCode suites each pass 323 tests, the canonical run with 8 skips and each projection with 20. The manifest-matched owner check reports `current` with 21 current, 18 human-evolved, 4 present and 0 stale, and the generated checker reports `current` with 0 writes. Projection parity across the whole package: 0 mismatches. A review before merge probed one risk and cleared it with evidence: a prior-era legacy install now yields a full owned set, so the migration and the ordinary replacement could have handled the checker and manifest twice. They do not, and the reason is deliberate rather than accidental, because `plan_changes` already removes migration-handled replacements at `harness_tool.py:2132`. The same probe found an unpredicted gain: the six command wrappers that a prior-era migration used to leave pointing at renamed skills are now regenerated in the same pass, which closes the dangling-command window that the `v3.1-rc.4` consumer run exposed. A regression pins both facts. Canonical, Claude and OpenCode suites each pass 324 tests. |
| Decision history | 2026-08-19: registered `ready` at P0 after the owner refused a two-pass update and stated the goal directly, that a new skill version copied over a repository and run once should offer an update. An earlier answer in the same session presented the two-pass behavior as a deliberate security lock, which was wrong for the owner's actual case; the experiment corrected it. 2026-08-19: implemented the same day. The security boundary was widened deliberately and stated in issue #71 rather than slipped in: byte equality with the current template proves freshness, not authenticity, and cannot hold across versions by definition, while provenance plus the embedded manifest digest is what lets a manifest name its own outputs. Every replacement still requires provenance on the file being replaced, so a human-owned path stays unreachable. |
| Next advancement condition | Reopen only if an installed harness carrying builder provenance is again refused an update it owns, if a path without builder provenance is ever replaced, or if the checker and its manifest are ever offered separately. |

### RH-W-0044
| Field | Value |
|---|---|
| Origin | Owner statement of the goal on 2026-08-19: a new skill version is copied over the repository, run once, and the update reviews the core, key decisions, and tools against what the builder would produce now, then suggests the changes without damaging anything |
| Priority | P2: regeneration already repairs everything the builder owns, so the update is useful without this unit, but the human-owned half of a harness is where drift actually accumulates and today it is reported only as a missing section or a missing directive. |
| Dependencies | RH-W-0043, because a suggestion the builder cannot then apply is advice rather than an update. |
| Status | proposed |
| Completed work | Bounded what already exists so this unit does not rebuild it. Congruence checking already reports `stale`, `missing`, `human-evolved`, `sections-missing`, and `directive-missing` per path, and human-owned documents already stay proposal-only. The gap is content-level comparison and a concrete proposed change, not detecting that a human-owned document diverged. |
| Remaining work | Design the scope boundary before any implementation: which human-owned documents are compared, what one suggestion contains, how suggestions are presented and accepted individually, and how the builder avoids proposing to undo legitimate divergence the repository intended. |
| Additions | A compare-and-suggest pass over human-owned core documents that presents per-document proposed changes for individual acceptance. |
| Exclusions | No overwriting of a human-owned document, no bulk accept, no claim of semantic equivalence, and no builder ownership of the roadmap, decisions, or changelog content this repository owns. |
| Deferrals | none |
| Rejections | Rejected registering this `ready`. Its scope boundary is the entire risk, which is the same reason `RH-W-0022` stays `proposed`, and an undesigned boundary here would propose edits to the documents holding this project's durable truth. |
| Evidence | The classification surface already exists: `plan_changes` routes a human-owned path to `existing` with a proposal-only reason at `skill/repo-harness-builder/scripts/harness_tool.py:2085`. The owner-run consumer report listed `REPO-HARNESS.md` and `CHANGELOG.md` as human-evolved plus four documents with missing sections, which is exactly the detection half without the suggestion half. |
| Decision history | 2026-08-19: registered `proposed` at P2 in the same work unit as `RH-W-0043`, from the owner's description of the update he wants. Recorded as `proposed` rather than `ready` because the compared document set and the acceptance interaction are undesigned. |
| Next advancement condition | `RH-W-0043` is completed and the prerequisite is satisfied. Retain `proposed` until a written scope design names the compared document set, the suggestion shape, and the one-at-a-time acceptance interaction. |

### RH-W-0045
| Field | Value |
|---|---|
| Origin | Owner-run `/repo-harness-builder` preview against the real prior-era consumer on 2026-08-19, reviewed and deliberately not approved. The plan was correct in every byte it proposed to write and silent about what it would leave broken. |
| Priority | P1: the migration deletes the legacy generated skill paths, and the rulebook naming them is read at the start of every session, so an upgraded repository immediately follows a decision gate pointing at a file the same apply deleted. No apply fails and no content is lost, which is why this is P1 rather than P0. |
| Dependencies | `RH-W-0041` and `RH-W-0043`, both completed. A prior-era migration had to be reachable before what survives it could matter. |
| Status | completed |
| Completed work | Measured against the real consumer rather than reasoned about from the plan. The preview proposed 9 moves, 9 removals, 2 replacements and 6 command wrapper replacements, and classified `REPO-HARNESS.md` as `existing`, which is correct: human-owned bytes are never rewritten. A repository-wide read-only scan then showed what that leaves behind. Twenty files name a legacy skill. Fifteen are builder-owned and handled by the same apply, one is a historical session record that is correct as history, one is untracked tool state, and three are tracked human documents the apply preserves unchanged while deleting what they name. Implemented on 2026-08-19. |
| Remaining work | none |
| Additions | `tracked_markdown_paths()` bounds one `git ls-files` call through the injectable executor pattern already used for capability detection, `resolved_by_apply()` names every path this apply overwrites or deletes, and `surviving_legacy_reference_report()` reports the exact file, line, and skill for each surviving reference. The preview emits it as `survivingLegacyReferences`, outside the plan dictionary, so it never enters `plan_digest`. Seven behavioral tests, including one that ages a fixture rulebook into the prior-era gate rows the current renderer cannot produce on its own. |
| Exclusions | No human-owned byte is rewritten and no suggestion is generated; suggesting edits is `RH-W-0044` and stays there. No change to migration behavior, path safety, apply atomicity, rollback, or the plan digest. No consumer repository is modified. Untracked and ignored tool state stays out of scope. |
| Deferrals | none |
| Rejections | Rejected rewriting the surviving documents automatically, which would break the preservation rule the whole product rests on. Rejected binding the report into `plan_digest`, because it describes files the apply never writes and would let an unrelated documentation edit invalidate an approved plan. Rejected scanning every text file: measured on the real consumer, that adds a historical session summary whose legacy names are correct history, and tool-owned state its own tool regenerates, so the wider scope reports noise as defects. |
| Evidence | Absence was proved before the fix: all seven tests failed with `AttributeError: module 'harness_tool' has no attribute 'surviving_legacy_reference_report'`, and no scanner or link validator existed in the product surface, only link assertions inside the self-hosted suite. The fixture exposed a second fact worth recording: `render_with_skill_mapping()` remaps generated skill paths but still writes the fixed names into the rulebook prose, so a fixture rendered today cannot reproduce a real prior-era rulebook, which is the same class of gap `RH-W-0041` hit. The test ages it deliberately. Verified against the real consumer through a read-only preview with 0 writes: 11 references across `CONTRIBUTING.md` at lines 22, 23, 99, 113 and 144, `REPO-HARNESS.md` at lines 28 and 29, and `docs/repo-harness/README.md` at lines 11, 12 and 13, with `status` `checked` and `scope` `tracked-markdown`. Six of those eleven were invisible to the manual review that opened this unit. The canonical suite passes 331 tests with 8 skips. |
| Decision history | 2026-08-19: the owner ran the builder against the prior-era consumer, reviewed the plan, declined it, and asked for the defect to be fixed in the builder rather than worked around in the consumer. Registered and completed the same day. The scope boundary was decided from measurement rather than preference: tracked markdown was chosen after the wider scope was shown to report a historical record and tool-owned state as defects, and the known cost is stated rather than hidden, since a gitignored registry that names the removed skills is not reported. |
| Next advancement condition | Reopen if a migration removes a generated skill while a surviving tracked document still names it unreported, if the report ever rewrites a human-owned byte, or if the excluded untracked scope produces a failure the human could not have anticipated from the reported scope. |

### RH-W-0041
| Field | Value |
|---|---|
| Origin | Owner-run `v3.1-rc.3` upgrade against a real generator-3.0 consumer, reported back and then re-verified against canonical source in this repository |
| Priority | P0: the legacy skill-name migration that `RH-W-0021` and `RH-W-0024` built is unreachable against every real legacy install, so no consumer carrying project-prefixed generated skills can be upgraded at all |
| Dependencies | none |
| Status | completed |
| Completed work | Confirmed the reported first wall at `skill/repo-harness-builder/scripts/harness_tool.py:2239`, `require(isinstance(language, dict), "manifest ownership is unverifiable")`, and proved it is a defect rather than a policy by locating the contradicting contract in the same file: `declared_languages()` at line 2530 tolerates the absent field and infers `["en"]` plus `"es"` when `docs/es/repo-harness` exists, which is the `RH-W-0003` behavior. Then measured past the reported wall instead of stopping at it. An isolated probe over a copy of a real consumer, with the repository left untouched, injected the inferred `language` and found the migration still blocked one wall later. |
| Remaining work | none |
| Additions | A legacy language fallback reusing `declared_languages()`, one `prior_era_installed_outputs()` helper that reads the gated paths from disk only when the current renderer cannot reproduce the manifest, `authentic_installed_checker()` accepted alongside template equality, and two regressions on a fixture that reproduces a genuine pre-`language` install. |
| Exclusions | No change to any consumer repository, no hand-patched consumer manifest, no weakening of path safety, and no change to what byte equality authorizes for non-manifest generated paths. |
| Deferrals | none |
| Rejections | Rejected patching the consumer's `.repo-harness/manifest.json` to add `language`, which unblocks in seconds and masks the origin defect while leaving every future install exposed. Rejected repairing the consumer's own `.claude/skills/repo-harness-builder/` copy, which is a runtime projection of the distributed package and not canonical source. |
| Evidence | The consumer manifest carries exactly `canonicalSkills`, `generator`, `integrationMode`, `project`, `runtimes`, `schemaVersion`, `writeSemantics`, with no `language`. Probe result over a temporary copy: wall 1 raises `ContractError manifest ownership is unverifiable`; with the inferred `language` injected, `legacy_migration_inputs()` succeeds and reconstructs 37 outputs with 9 replacements; `plan_generated_output_migration()` then raises `ContractError manifest ownership or bytes are unverifiable`. The cause of wall 3 is measured, not inferred: installed `writeSemantics` is `additive-create-with-runtime-pointer-append` while the current renderer emits `additive-create-with-bounded-root-append`, so byte equality cannot hold for a prior-era manifest. Measured once more before committing to the `RH-W-0036` remedy, which proved that remedy insufficient on its own. `authentic_installed_checker()` returns `True` for the consumer, so provenance plus the embedded digest does establish authenticity. But byte equality is required at eleven further gates, and every one of them fails: all 9 legacy `SKILL.md` files fail `generated source bytes changed`, and the installed checker fails its own reconstruction comparison. The cause is structural rather than incidental: the migration is designed to reproduce the installed generated tree from the current renderer, which only holds for a same-era install carrying legacy names, and a genuinely older install has different template bytes by definition. Fixing the manifest gate alone therefore moves the failure one step later without unblocking anything. Completion evidence: the RED test builds a genuine pre-`language` install and failed at wall 1; after the fix, planning against a copy of the real consumer yields 9 moves, 9 removals and 2 replacements, mapping every project-prefixed legacy skill onto its fixed `repo-harness-*` name. A third wall appeared during implementation and was the same conflation one level up, the checker gate comparing against the current template instead of proving authenticity, so `authentic_installed_checker()` is now accepted alongside template equality. Canonical, Claude and OpenCode suites each pass 306 tests and both checkers report `current` with 0 writes and 0 stale. Both generator versions read `3.0` and the tool `VERSION` is `3.0`, so a generator-version comparison would not have caught this. The existing `migration_fixture` in `tests/test_harness_tool.py` renders its installed manifest through the current renderer, so it always carries `language` and never reproduces a real legacy install; that is why the defect shipped inside `v3.1-rc.3`. |
| Decision history | 2026-08-18: registered `ready` at P0 after the owner ran the upgrade himself and the two reported defects were re-verified against canonical source, then completed the same day. The chosen route was the smaller one: prove ownership by what a prior-era install can still show, rather than teach the renderer to reproduce prior-era bytes. Same-era installs keep strict byte equality untouched, so the relaxation reaches only the case where the strict proof is impossible. The consumer stays untouched and serves as the verification fixture. |
| Next advancement condition | Reopen only if a prior-era install is again refused a migration it owns, if byte equality returns as the only accepted proof, or if a generated path without builder provenance is ever migrated or removed. |

### RH-W-0042
| Field | Value |
|---|---|
| Origin | Same owner-run consumer upgrade: the apply stayed blocked by `.gitattributes` even after the migration question |
| Priority | P1: a correctly written LF-safe repository is refused, and the only remedy offered today is editing a human-owned file that is already correct |
| Dependencies | none; independent of `RH-W-0041` |
| Status | completed |
| Completed work | Confirmed the reported cause at `conflicting_attribute_paths()` line 498, which flags any assignment starting with `text=` while evaluating each assignment independently of its siblings on the same line. Narrowed the report rather than accepting it: blocking a bare `* text=auto` is correct and deliberate, because `text=auto` alone checks out CRLF on Windows, and it is covered at `tests/test_harness_tool.py:3593` inside `test_matching_non_lf_attributes_block_without_writes`. Only the `text=auto` plus `eol=lf` combination is a false positive. Read the consumer's real file to confirm which rule actually fires. Implemented behaviour-first: extended the existing compatible-attributes test into a case table, watched it fail on `* text=auto eol=lf` with the exact conflict list, then made the rule combination-aware by pinning `text=auto` to a sibling `eol=lf`. Synchronized both runtime projections and verified against the consumer's real file. |
| Remaining work | none |
| Additions | One combination-aware compatibility rule and its regression, keeping the bare `text=auto` block intact. |
| Exclusions | No change to any consumer `.gitattributes`, no relaxation of `binary`, `-text`, `!text`, `eol=crlf`, or bare `eol`, and no change to macro expansion. |
| Deferrals | none |
| Rejections | Rejected asking the consumer to edit its `.gitattributes`, because the file is correct and documented and the builder is the side that is wrong. |
| Evidence | The consumer's `.gitattributes` line 4 is `* text=auto eol=lf`, and that single wildcard reaches all 24 byte-compared protected paths. Its remaining rules use bare `text` and pass cleanly; `*.bat` and `*.cmd` carry `text eol=crlf` but match no protected path. `text=auto eol=lf` normalizes text files and pins the working tree to LF, so it is compatible with the exact `text eol=lf` rule the builder requires. No test covered the combination before this unit. Completion evidence: the RED run failed on `assertFalse` with the blocked entry naming 5 conflicting paths; after the change the canonical, Claude and OpenCode suites each pass 304 tests, the bare `* text=auto` block stays green in `test_matching_non_lf_attributes_block_without_writes`, and `conflicting_attribute_paths()` returns 0 conflicts over 31 protected paths against the consumer's real `.gitattributes`. Both checkers report `current` with 0 writes and 0 stale. |
| Decision history | 2026-08-18: registered `ready` at P1 alongside `RH-W-0041` from the same consumer run, and completed the same day. The owner's instruction to keep the work small and organic applied directly: the fix is one added condition, not a new abstraction. |
| Next advancement condition | Reopen only if a combination that genuinely forces non-LF is accepted, or if bare `text=auto` stops blocking. |

### RH-W-0040
| Field | Value |
|---|---|
| Origin | Owner proposal after three rule violations in one session: surface the rules that are actually forgotten instead of the whole rulebook |
| Priority | P3: it addresses only the residue left after RH-W-0039, and its evidence source is human-fed, so it cannot be trusted as a detector |
| Dependencies | RH-W-0039; only rules proven unmechanizable belong here, and that set is unknown until the checks exist |
| Status | proposed |
| Completed work | Established the counter-evidence that bounds this unit. `REPO-HARNESS.md:22` is injected into agent context every turn and was present throughout the session in which it was violated three times, so surfacing rules earlier is proven insufficient on its own and this unit must not be scoped as a rule reader. |
| Remaining work | Design what a violation record contains, where it lives, who writes it, and how session start consumes it without becoming another ignored wall of text. |
| Additions | A bounded record of repeatedly violated unmechanizable rules and a session-start path that surfaces only the top offenders. |
| Exclusions | No re-reading of the full rulebook at session start, no duplication of rules already enforced by a check, no automatic violation detection claim, and no new state machine or third authority. |
| Deferrals | none |
| Rejections | Rejected framing this as a pre-session rule reader, because the violated rule was already in context. Rejected presenting it as automatic detection: it can only record violations a human or a later check noticed, so an empty ledger never proves compliance. |
| Evidence | In one session the claim-classification rule was violated three times while loaded: an inference about test coverage stated as fact, a corrected restatement that was also wrong, and a stale suite count introduced while correcting stale suite counts. A fourth instance was measured on 2026-08-20 and has a sharper shape than the first three. Reviewing the consumer plan, a narrow grep for five exact phrases returned nothing, and that null result was reported to the owner as the verified fact that the builder contract carries no evidence obligation at all. A broader grep then found the obligation in the same files, at `references/audit-and-interview.md:7`, `Report uncertainty and the exact missing evidence instead of guessing`. The rule was loaded throughout and was not skipped; it was misapplied, because an unvalidated probe was labeled `verified fact` rather than the weak single-source inference it was. The named discipline that would have caught it is a positive control: a null search result supports a negative claim only after the same search is shown to return a positive on a string known to be present. Recorded here because it is the same failure the run under review had committed about a root `LICENSE`, which makes it the clearest evidence yet that this residue is about how a claim is classified and not about which rules are visible. |
| Decision history | 2026-08-18: registered `proposed` under issue #65 from an owner proposal, deliberately narrowed to the unmechanizable residue after the counter-evidence showed a rule reader would have prevented none of the observed failures. |
| Next advancement condition | RH-W-0039 is completed and the prerequisite is satisfied. Retain `proposed` until a written design decides the violation schema, authority, storage location, writer, ranking rule, and bounded session-start presentation for the measured unmechanizable residue. |

### RH-W-0039
| Field | Value |
|---|---|
| Origin | Measured during RH-W-0038 delivery: rules that a check could enforce were being defended by prose and were violated anyway, including by the change that fixed them |
| Priority | P2: a documented fact that drifts silently is the defect class this repository keeps repeating, and each instance costs a session to find while a check would cost one test run |
| Dependencies | none |
| Status | completed |
| Completed work | Identified the mechanizable set from observed violations and confirmed the gap is real: no test compares the suite count documented in `current-state.md` against the actual count, which is why `294` and then `300` both went stale while the suite ran `300` and then `301`. Added all three checks. Each was proved to fail on a real defect before being trusted: the count check failed immediately at `301 != 304`, the line-ending check found pre-existing mixed endings in `README.md` and `README.es.md` on its first run, and the projection check was proved by injecting a `completed` claim against a `ready` record and observing `'ready' != 'completed'`. The line-ending check is working-copy scoped: Git normalizes to LF in the blob, so a mixed file is a local hygiene defect rather than a repository defect, and normalizing the two READMEs produced no diff. It still earns its place because a wrong newline written by an edit is exactly what corrupts the byte-exact digest contract on generated paths before any commit happens. |
| Remaining work | none |
| Additions | Behavioral checks that convert three prose obligations into failures with exit codes. |
| Exclusions | No attempt to mechanize prose judgment such as evidence class or tone, no repository-wide line-ending policy, no new dependency, and no change to the roadmap as lifecycle owner. |
| Deferrals | none |
| Rejections | Rejected relying on reminders for this set. The claim-classification rule was in context every turn and was still violated three times, so a loaded rule is not an enforced rule and a check is the only difference. |
| Evidence | `grep` for the documented count phrase across `skill/repo-harness-builder/tests` returns nothing, while `current-state.md` asserted `294`, then `300`, against real suite sizes of `300` and `301`. Two stray LF bytes also survived in `decisions.md` working copies from an earlier edit. |
| Decision history | 2026-08-18: registered `ready` under issue #64 after the owner asked for a control that runs before the work, and the mechanizable half of that request proved cheaper and stronger than the memory half. |
| Next advancement condition | Reopen only if a documented fact drifts again in a way one of these checks was meant to catch. |

### RH-W-0038
| Field | Value |
|---|---|
| Origin | Observed live: a session-start report stated an inference about test coverage in assertive form, and the corrected restatement about the same subject was also wrong while sounding equally certain |
| Priority | P1: the rulebook already forbids confidence wording as a substitute for evidence, but nothing obliges the emitted report to show the class, so an unlabeled inference violates no output contract and can be written into durable documentation as fact |
| Dependencies | none |
| Status | completed |
| Completed work | Confirmed the claim-classification rule exists at `REPO-HARNESS.md:22` and that the generated session-start contract echoes it as a single line, then confirmed all three daily Output Contracts enumerate report topics without requiring any claim to carry its evidence class. Added the requirement to the session-start, session-stop, and documentation Output Contracts in `skill/repo-harness-builder/scripts/harness_tool.py`, proved it RED first with a new rendered-contract regression, and regenerated the nine stale projections through one digest-bound apply. |
| Remaining work | none |
| Additions | One Output Contract sentence per daily skill requiring the evidence class, and one behavioral regression asserting it in all three rendered contracts. |
| Exclusions | No new rulebook rule, no change to the RH-W-0032 classification principle itself, no checker gate, no localization of the clause, no Git or GitHub action, and no release. |
| Deferrals | none |
| Rejections | Rejected adding the requirement to session-start alone, because the session-stop handoff is what writes durable current-state text. Rejected promising a mechanical gate, because a prose rule about prose output cannot be enforced by `check.py`; this unit strengthens an obligation and does not create a gate. |
| Evidence | Digest-bound plan `9d7b979cb9a2936f496eaf286d6c8478ecbd571ffe35a36a6ba9730594eb7d76` offered exactly the nine stale `SKILL.md` paths with zero create, append, migrate, or blocked actions. After apply, the manifest-matched owner check and the generated checker both report `current` with 0 writes, and the canonical suite passes 301 tests with 7 skips. |
| Decision history | 2026-08-18: registered and completed as one unit after the user identified the pattern directly, because the diagnosis showed a missing output obligation rather than a missing rule. |
| Next advancement condition | Reopen only if a daily report presents an inference in assertive form again, or if a rendered Output Contract stops requiring the evidence class. |

### RH-W-0037
| Field | Value |
|---|---|
| Origin | Injection experiment run during RH-W-0034 reconciliation: the same completed-with-outstanding-work contradiction failed the suite in the English roadmap and passed it in the Spanish peer |
| Priority | P1: every lifecycle invariant is enforced against the English roadmap only, so the Spanish peer can carry a self-contradicting record indefinitely and a Spanish-reading maintainer receives false lifecycle state |
| Dependencies | none |
| Status | completed |
| Completed work | Proved the gap by experiment rather than inspection. Injecting `Trabajo restante: quedan las fases 3, 5 y 6 sin entregar.` into the completed Spanish `RH-W-0034` record kept the suite green at 300 tests, while the equivalent English injection failed `test_repository_roadmap_records_have_valid_lifecycle_fields` with `'phases 3, 5 and 6 are still outstanding' != 'none'`. Both files were restored and verified by SHA-256. Measured the structural cause behind it. |
| Remaining work | none |
| Additions | Spanish-side lifecycle field completeness and one regression enforcing status-to-remaining-work consistency in both roadmaps. |
| Exclusions | No change to the English roadmap format, no new locale, no generated product behavior change, and no Git or GitHub action. The exclusion that forbade converting the Spanish peer into a table is retired: that separate decision was taken on 2026-08-18 and recorded on issue #63, so the conversion is now the chosen route rather than a blocked one. |
| Deferrals | none |
| Rejections | Rejected a gate that skips records missing the label, because partial enforcement is the same defect class this unit exists to remove. |
| Evidence | The English table parses 504 fields across 36 records while the Spanish prose parses 0 under the same expression, and `test_repository_roadmap_records_have_valid_lifecycle_fields` never opens the Spanish file at all. Only 6 of 36 Spanish records carry all twelve labelled fields: `Rechazos` is absent from 22, `Adiciones` from 21, `Exclusiones` from 15, and `Dependencias`, `Trabajo completado`, and `Trabajo restante` from 13 each. The lossy-peer pattern is not confined to the roadmap. On 2026-08-18 the same loss was found in two more surfaces: the `README.es.md` skill row omitted the remote-check-question clause its English owner carried, and `docs/es/repo-harness/usage.md` carried a bare sentence where the English entry carried three further clauses. Both were levelled in that commit, but the pattern suggests this unit's scope may be too narrow and should be re-measured across every Spanish peer before implementation. Completion evidence: re-measuring first is what kept this small. Of 42 Spanish records, 32 were missing at least one label, but only `Estado` and `Trabajo restante` carry the lifecycle lie this unit exists to remove, and `Estado` was already present everywhere. Backfilling the 13 records missing `Trabajo restante` and enforcing the invariant was therefore the complete fix, not a 32-record rewrite. The new `test_spanish_roadmap_records_hold_the_same_lifecycle_invariant` immediately caught a real one: Spanish `RH-W-0008` declared `completed` while claiming 17 self-hosted paths still needed separate digest approval, where its English owner said `none`. It was corrected to match the owner. The gate was then proved by the same injection method that opened this unit, and the file was restored byte for byte. Canonical, Claude and OpenCode suites each pass 307 tests. The remaining label gaps are completeness rather than lifecycle correctness and are not tracked here. |
| Decision history | 2026-08-18: registered `ready` under issue #63 after the user challenged an unverified claim about this gap, and the injection experiment converted it from inference into measured fact. |
| Next advancement condition | Reopen only if a Spanish record can again declare a status its remaining work contradicts, or if the Spanish peer stops being read by the invariant. |

### RH-W-0036
| Field | Value |
|---|---|
| Origin | Discovered while verifying RH-W-0034 phase 6: the owner check reported `.repo-harness/check.py` stale while the render plan offered zero actions to correct it |
| Priority | P0: a stale generated checker disables replacement for every manifest-owned generated output, so any future change to `check_script()` permanently freezes every already-installed consumer |
| Dependencies | none; RH-W-0034 phase 6 cannot close until this is resolved |
| Status | completed |
| Completed work | Located the exact gate at `manifest_owned_generated_paths()` in `skill/repo-harness-builder/scripts/harness_tool.py:1964` and confirmed the circular block against live evidence. |
| Remaining work | none |
| Additions | A bounded, digest-bound checker refresh path and regression coverage proving a stale checker no longer empties the replaceable set. |
| Exclusions | No weakening of manifest digest authentication, path safety, exact projection comparison, or zero-write checking. No direct patching of a consumer checker. No change to the external consumer. |
| Deferrals | none |
| Rejections | Rejected treating this as part of RH-W-0034; the language work is complete and independent, while this is a lifecycle defect in the update mechanism itself. |
| Evidence | `manifest_owned_generated_paths()` returns an empty set when `checker_target.read_text(...) != check_script(digest_bytes(manifest_bytes))`, so the plan reports 0 replace, create, append, migrate, and blocked actions while the owner check reports `drift` with exactly 1 stale path. Contrast evidence: the same command produced 3 `replace-generated-output` actions earlier in the same session, before `check_script()` changed. |
| Decision history | 2026-08-17: registered and completed the same day. New `authentic_installed_checker()` separates authenticity from freshness: builder provenance plus the embedded `MANIFEST_SHA256` naming these exact manifest bytes now gates the replaceable set, while byte equality with the current template only decides whether the checker itself needs replacing. RED proved an authentic prior-version checker returned an empty set; after the fix the self-hosted plan offered exactly 1 `replace-generated-output` action, applying digest `fe5fb7041bcff1887a859a15cfc377399bc277d221c75cf1b4f933e8ae14c915` regenerated the checker, and both checkers report `current` with 0 writes and 0 stale. Three suites pass 300 tests. Self-review then found that provenance plus embedded digest is a weaker proof than byte equality, because an editor who changes both the manifest and the checker's digest line would satisfy it. The weaker proof therefore authorizes exactly one repair, replacing the checker itself; every other generated path still requires byte equality with the current template. 2026-08-19: superseded in part by `RH-W-0043`. The weaker proof authorized exactly one repair, the checker itself, and that narrowing turned out to freeze every installation on a version bump and to leave the checker naming manifest bytes nobody wrote. It now authorizes the whole manifest-owned set. The authenticity requirement this unit established is unchanged and still gates everything. |
| Next advancement condition | Reopen only if an authentic installation is again denied a corrected checker, or if provenance and embedded-digest proof stop gating the replaceable set. |

### RH-W-0035
| Field | Value |
|---|---|
| Origin | Measured interpreter-invocation audit of this repository, prompted by a confirmed `python` versus `python3` failure observed on a macOS collaborator machine in an unrelated repository |
| Priority | P1: the canonical test suite cannot start on a machine without a bare `python` on PATH, so macOS and PEP 394 Linux contributors cannot verify this builder at all, and every generated harness instructs its consumers to run an interpreter name the same builder does not guarantee |
| Dependencies | none; independent of `RH-W-0034` and of any release |
| Status | completed |
| Completed work | Audited every interpreter invocation in the repository, measured the interpreters actually present on the Windows development machine, classified findings into generated consumer text, test-suite invocations, contributor documentation, and out-of-scope imported material, and confirmed byte parity of the three `harness_tool.py` copies at `f344fca692249d91b7c3c67ec81c54e11a348a3e63a1740b4e41c9b55c60ab12`. |
| Remaining work | none |
| Additions | Portable interpreter resolution in tests, one guard covering bare interpreter literals in subprocess argument lists, and generated documentation copy that names the Windows and POSIX cases explicitly instead of assuming either one. |
| Exclusions | No change to any other repository, no platform-conditional rendering of generated documentation, no interpreter version bump, no new dependency, no `py` launcher requirement, no Git mutation, no external service, no release, and no changelog entry while planned. |
| Deferrals | Real execution on macOS and Linux hardware is deferred until a collaborator machine is available; Windows evidence is authoritative for this unit and the correction must not depend on a single platform to be valid. |
| Rejections | Rejected a blanket `python` to `python3` substitution, because `python3` is absent from a default python.org Windows installation and this machine only resolves it through Python Manager. Rejected rendering the generated command from the installing machine's platform, because the generated file is committed into a repository that contributors on the other two systems clone. Rejected touching unrelated repositories, user-owned personal storage, and the historical `docs/superpowers/plans/` record. |
| Evidence | `skill/repo-harness-builder/tests/test_harness_tool.py` invokes `subprocess.run(["python", ...])` at lines 83, 91, 433, 451, 902, 912, 1289, 1325, 1764, 1786, 1807, 1831, and 2787 while the same file already uses `sys.executable` at lines 1110, 3555, 3961, 3972, 4053, 4070, 4135, 4156, and 4178. `skill/repo-harness-builder/scripts/harness_tool.py` emits `python .repo-harness/check.py` into consumer text at lines 756, 1011, 1209, 1306, 1330, 1426, and 1519, while line 1642 gives the generated checker a `#!/usr/bin/env python3` shebang, so generated output contradicts itself. Measured on the Windows development machine on 2026-08-17: `python` resolves to `C:\Python314\python.exe` reporting 3.14.3, `python3` resolves through Python Manager reporting 3.14.2, and `py -3` reports 3.14.2. Baseline before any change: 293 tests passed with 7 skips. Completion evidence: the guard RED listed exactly those 13 offenders with zero false positives, and after the correction the canonical, OpenCode, and Claude suites each passed 294 tests. Digest-bound maintenance plan `02ec2033010c0ade1e7530399204c880aaaadaa12aa69acc576de65eeff41a17` carried exactly 3 `replace-generated-output` actions and no create, append, migrate, or blocked action; applying it changed 3 files by one line each and returned the owner check to `current` with 0 stale, 21 current, 18 human-evolved, and 4 present. |
| Decision history | 2026-08-17: registered as `ready` after the user confirmed the standing Windows, Linux, and macOS support requirement already recorded for line endings, and directed that Windows is the current working platform while the correction must keep the other two systems valid. |
| Next advancement condition | Reopen only if a bare interpreter name returns to a subprocess invocation, generated command copy stops naming both the Windows and POSIX cases, or real macOS or Linux execution contradicts the Windows evidence. |

### RH-W-0034
| Field | Value |
|---|---|
| Origin | Verified `v3.1-rc.2` consumer transcript and installed-output evidence from a Spanish-only non-Git consumer |
| Priority | P1: the published candidate can generate a checker that rejects its own valid Spanish-only manifest, advertise nonexistent documentation, select report language from wrapper prose, and route a builder defect into irrelevant provider reporting |
| Dependencies | none for implementation; successor release publication remains a separate approval-bound work unit |
| Status | completed |
| Completed work | Confirmed the defects against canonical source and tests, selected `RH-W-0034` as the next immutable ID, resolved every implementation decision, and created the canonical executable plan plus complete Spanish peer. Delivered phase 2 on branch `fix/rh-w-0034-spanish-only-parity` as commit `a4ba876`: the generated `parse_manifest()` now accepts exactly the domain `normalize_documentation_languages()` accepts, and the Spanish documentation map advertises only destinations its own render creates. Delivered phase 4 as `5c89470`, synchronizing both runtime projections. Delivered phase 1 as `f3e2b27`: report-language step 3 now requires evidence the host can attribute to the human, a new step 4 asks one localized question and stops instead of falling back silently, and generated contracts, repository text, assistant prose, memory, and tool results join wrapper prose as things that are never evidence. Canonical, OpenCode, and Claude suites each pass 298 tests. Delivered phase 3 as `cfadb19`: a Failure Ownership contract classifies the failing owner from generated-artifact provenance before any provider handoff, so builder-owned output never reaches provider reporting, issue creation, or repository publication. Phase 5 verified a temporary Spanish-only fixture of 33 files with both checkers `current` at 0 writes and 0 stale, 10 resolving links, and no bilingual-only path, and never touched the external consumer; its T-009 map check also exposed two defects the plan had not predicted, both in bilingual mode, where the Spanish map pointed one level too high. Phase 6 was blocked on `RH-W-0036` and closed as `9084918` once the stale-checker gate was corrected; the branch merged into `main` through pull request #57 as `461f625`. |
| Remaining work | none |
| Additions | Concrete Spanish interview copy; typed report-language evidence with ask-once fallback; exact owner/generated documentation-locale parity; Spanish-only local map and link validation; owner-before-provider failure classification; safe upgrade and fresh regeneration guidance. |
| Exclusions | No direct mutation of the external consumer, consumer checker patch, Gentle AI source change without separate evidence, Git or RDD mutation, provider or GitHub action, repository publication, external service, tag or release alteration, new locale, translation service, dependency, or changelog entry while planned. |
| Deferrals | Publishing a successor release candidate is deferred until the implementation, all three package suites, temporary consumer replay, explicit-dimension owner check, generated checker, links, lifecycle, and byte audits pass. |
| Rejections | Rejected patching `.repo-harness/check.py` in the consumer, treating English wrapper prose as human language evidence, requiring English first in a Spanish-only manifest, advertising bilingual-only files, classifying builder-generated mismatch as a Gentle AI defect, attempting clone-scope RDD disable in a non-Git directory, and implying GitHub repository publication. |
| Evidence | `normalize_documentation_languages()` accepts `("es",)` while generated `check_script().parse_manifest()` rejects `documentation[0] != "en"`; `docs_outputs()` emits nonexistent English-owner and bilingual-maintenance links in Spanish-only mode; the report-language contract already says wrapper prose and tool output are not conversation evidence; verified consumer evidence shows the resulting English report, checker failure, ownership misclassification, provider-consent detour, non-Git directory, and global RDD already off. |
| Decision history | 2026-08-17: registered as `ready` because the user supplied verified reproduction evidence and explicitly resolved wording, language precedence, preservation behavior, failure ownership, remediation safety, Git/RDD boundaries, release sequencing, exact verification, and non-goals. |
| Next advancement condition | Reopen only if a Spanish-only installation is rejected by either checker, a documentation map advertises a destination its render does not create, report language is selected without evidence attributable to the human, or a builder-generated mismatch reaches provider or publication flow. Publishing a successor to `v3.1-rc.2` remains a separate approval-bound work unit. |

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
| Dependencies | Session-start consent design, repository-scoped preference storage design, and focused safety tests. The RH-W-0046 consent design is the expected route to that storage design, and the namespace reserved by RH-W-0051 is its expected location; neither advances this unit by itself. |
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
| Status | completed |
| Completed work | Removed prompt-visible provenance from normal and help runtime wrappers while preserving their thin registered-skill behavior. Marker-free wrappers receive version evidence only when the installed checker authenticates the exact manifest bytes and the wrapper bytes exactly match the manifest-derived runtime, command mapping, language, and locale render. Arbitrary wrapper bytes and forged checker digests fail closed and remain proposal-only. Skills and documentation retain their provenance. Strict RED first excluded both wrapper kinds from replacement and found the missing evidence API; focused GREEN passed 9 tests. |
| Remaining work | none |
| Additions | none |
| Exclusions | No change to skill or documentation provenance, which carries real reader value and lives in files the prompt does not load on every invocation. |
| Deferrals | none |
| Rejections | Rejected deleting the comment on its own, because wrappers have no frontmatter and the `source:` line is their only version evidence. |
| Evidence | Current completed-state evidence: `wrapper()` and `help_wrapper()` emit only thin registered-skill instructions. Authenticated replacement evidence requires exact manifest and wrapper bytes. Focused tests cover both wrapper kinds, exact-byte binding, forged checker digests, modifications, and legacy fallbacks. Git-local delivery evidence: implementation commit `e6218cada5c3056694e30d04d5fff6b04a764bd5` and merge commit `61c35585d6bc2fb411f2bb970ebba40b1b79a41c`, whose local subject identifies pull request #82. No issue or Judgment Day artifact is claimed. |
| Decision history | 2026-08-14: user questioned the token cost; verification confirmed the cost is real and small, and that a blind deletion would silently degrade every wrapper to `legacy`. 2026-08-20: completed through delegated direct work with authenticated manifest/checker evidence, exact wrapper-byte binding, and strict TDD. |
| Next advancement condition | Reopen only if a marker-free generated wrapper becomes unreplaceable, unauthenticated or modified wrapper bytes become replaceable, or wrapper provenance returns to prompt-visible output. |

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
| Status | completed |
| Completed work | The four outcome names were recorded first and only named, never defined; the 2026-08-15 analysis supplied the definitions and the verified gap. Implemented on 2026-08-19 through strict TDD. Eight behavioral tests were written first and all eight failed on `KeyError: 'packageFreshness'`, which proved the field did not exist rather than merely being unread. `package_freshness()` then classified every outcome and `check()` gained one separate report field. A fifth outcome was required and is not scope creep: the registered four leave an installed version later than the running builder undefined, which happens whenever an older skill copy is run against a newer harness, and the owner decided it must warn that an earlier builder is running and that continuing is at his own risk. A review pass before delivery then found three real defects in this unit's own work and each was proved before being fixed: a `generator` field present but not an object was classified `absent` when it is malformed and therefore `unverifiable`, a manifest reached through a link or reparse point was followed and trusted, and the reference contract that documents `runtimeSet` for the agent said nothing about the new field, so no agent reading the skill would know to surface staleness at all. |
| Remaining work | none |
| Additions | `package_freshness()` returning `state`, `installed`, and `running`, reported by `check()` as its own `packageFreshness` field, plus the reference contract in `references/preservation-and-safety.md` and `references/audit-and-interview.md` that tells the agent to report it on its own line and to relay the `ahead` warning. Five outcomes: `fresh` when equal, `stale` when a valid earlier version, `ahead` with a warning when a valid later version, `absent` when no manifest or no generator version exists, and `unverifiable` when the manifest is unreadable, not an object, or carries a version that is not a version. The classification is local and offline, because a consumer repository holds no upstream builder copy. |
| Exclusions | No runtime reload promise. No automatic upgrade: detection only, preserving the RH-W-0008 mutation boundary. `upgrade_plan()` deliberately does not forward the field in this unit; surfacing freshness inside the upgrade offer belongs with `RH-W-0043`, which is what makes an update applicable. |
| Deferrals | none |
| Rejections | none |
| Evidence | Verified 2026-08-15. The builder `check()` at `skill/repo-harness-builder/scripts/harness_tool.py:2386` never reads `manifest.generator.version`. The generated checker template at `harness_tool.py:1560` requires the installed manifest generator to equal the version frozen into that checker, which detects tampering rather than staleness, because a consumer checker is pinned to the builder that emitted it. Per-file `generatorVersion` is emitted only for already-drifted files at `harness_tool.py:2457-2460`, so a fully `current` projection set carries no package identity at all. Live divergence exists and nothing reports it: the installed manifest records `generator.version` `3.0` while `skill/repo-harness-builder/SKILL.md` declares `metadata.version` `3.1`. Confirmed again on 2026-08-18 with a live instance rather than a static reading: `RH-W-0038` changed generated bytes in all three daily Output Contracts and regenerated nine projections, while `VERSION` stayed `3.0` and every generated header still reads `source: core/session-start:3.0`. An installed consumer therefore cannot learn its harness is behind, which is exactly the detection this unit defines. Bumping `VERSION` rewrites every generated header and marks every consumer as behind, so it remains a release decision rather than a maintenance edit. Completion evidence: the RED run reported 8 tests and 8 errors, every one `KeyError: 'packageFreshness'`, so the field was proven absent before it was written. After the change the canonical, Claude, and OpenCode suites each pass 318 tests, the canonical run with 8 skips and each projection with 20, and the generated checker reports `current` with 0 writes. The suite-count guard from `RH-W-0039` caught the documented count immediately at `307 != 315`, which is the third time it has paid for itself. The review round added two regressions: the malformed-generator case failed RED with `'absent' != 'unverifiable'`, and the indirect-manifest case is written and passing but SKIPS on this Windows machine because it cannot create a symlink without elevation, so that branch is verified by reading and by the existing `is_indirect()` contract rather than by execution here. A POSIX or elevated run would raise it to executed fact. A second review, requested before merge, found a third defect in this unit's own guard: `is_indirect()` on the manifest checks only the final component, so an indirect ancestor such as a linked `.repo-harness/` was still followed, and the code comment promised containment the code did not deliver. It now reuses `guarded_target()`, which the repository already built to reject indirect ancestors, and answers `unverifiable` rather than raising. That branch is executed evidence rather than reading: a Windows directory junction needs no elevation, so the regression runs here and proved RED first by raising `ContractError: indirect output ancestor` out of `check()` itself. Projection parity was compared byte for byte across the whole package: 0 mismatches. |
| Decision history | 2026-08-13: retained as a separate concept from runtime projection drift. 2026-08-15: analysis corrected the overstated completed work, recorded the exact detection gap, and established that no human decision is required because the comparison reference is repository-local. 2026-08-19: confirmed as the detection piece of the update the owner described, so it runs first and `RH-W-0043` makes its result actionable. The same day the owner decided the undefined later-version case: it warns that an earlier builder is running and that continuing is at his own risk, rather than passing as fresh or hiding inside `unverifiable`. |
| Next advancement condition | Reopen only if a classification stops matching its definition, if an installed package ahead of the running builder stops warning, or if freshness ever mutates anything. |

### RH-W-0011
| Field | Value |
|---|---|
| Origin | Roadmap unit 11; `skills-pack-ld9` historical review |
| Priority | P2: still-relevant review findings need public evidence or explicit closure |
| Dependencies | RH-W-0003 through RH-W-0010 outcomes may close or reshape findings |
| Status | proposed |
| Completed work | The prerequisite range RH-W-0003 through RH-W-0010 is completed. Reconciliation must use current repository evidence and must not depend on external personal report paths or claim unavailable report contents. |
| Remaining work | Inventory only findings supported by repository or verified continuity evidence, then classify each as integrate, close, defer, reject, or noise. |
| Additions | Include the incomplete Judgment Day round-2 receipt in the evidence review. |
| Exclusions | No blind import of private or stale notes. |
| Deferrals | none |
| Rejections | none |
| Evidence | Repository lifecycle records show RH-W-0003 through RH-W-0010 completed. This repository does not contain the historical report contents, and Judgment Day round 2 still lacks judge B output, so those contents remain unverifiable and are not claimed. |
| Decision history | 2026-08-13: deferred with an observable evidence threshold. 2026-08-20: advanced to `proposed` because the prerequisite range completed; evidence limitations replace the former external-report dependency. |
| Next advancement condition | Advance after a written reconciliation scope identifies repository-local evidence sources, classification output, handling of the incomplete round-2 receipt, and explicit promotion approval without requiring unavailable report contents. |

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
- The 2026-08-21 portfolio re-audit revised the execution order and changed no status or advancement condition. `RH-W-0011` moves to second as the first application of `RH-W-0050`; the `P1` units `RH-W-0054` and `RH-W-0053` move ahead of the `P2` units that no declared dependency required them to follow; `RH-W-0051` is treated as a storage hub whose one design reserves namespace for every declared consumer; and `RH-W-0046` and `RH-W-0018` share one consent design while keeping separate implementations. Two field defects were corrected in the same unit: `RH-W-0044` no longer names the completed `RH-W-0043` as pending, and `RH-W-0055` no longer depends on the release asset naming contract it delivers.
