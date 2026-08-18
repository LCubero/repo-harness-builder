# RH-W-0034 Consumer Recovery And Spanish-Only Correctness Plan

> **For agentic workers:** REQUIRED SUB-SKILLS: use `systematic-debugging`, `test-driven-development`, and `verification-before-completion`. Track each checkbox. Implement only after moving the lifecycle record to `active`.

**Goal:** Correct the builder defects exposed by the verified `v3.1-rc.2` consumer transcript so Spanish users receive concrete questions, report language is resolved only from valid human evidence, Spanish-only installations pass both checkers with valid local links, and builder-owned failures never enter provider-defect reporting.

**Architecture:** Fix each defect at its canonical builder-owned source, then regenerate package projections and prove behavior in temporary consumer fixtures. Keep report, conversation, documentation, and artifact language independent. Classify ownership before any provider handoff. Treat generated consumer files as outputs, never product-fix surfaces.

**Tech stack:** Python 3.10+ standard library, `unittest`, generated Markdown contracts, JSON manifest, subprocess fixtures, and exact checked-in OpenCode and Claude package projections.

**Lifecycle owner:** [RH-W-0034](roadmap.md#rh-w-0034), status `ready`, priority `P1`. The roadmap owns status; this file owns the executable sequence.

## Root Cause

The consumer failure is one builder-owned defect cluster with four boundaries:

1. The interview contract exposes internal architecture vocabulary instead of the concrete choice a person must make. The same problem affects the no-harness message and existing-document preservation choice.
2. Report-language resolution is described as excluding wrapper prose and tool output, but the orchestration path accepted generated English wrapper text as current conversation evidence. That produced `--report-language english` and English `APPROVE` or `DENY` despite Spanish human conversation.
3. Spanish-only output is accepted by `normalize_documentation_languages()` and emitted by `docs_outputs()`, but `check_script()` independently rejects any manifest whose first documentation locale is not `en`. The Spanish-only map also emits bilingual-only links and an English-owner path that does not exist in monolingual Spanish output.
4. Failure handling classified a mismatch between builder-generated manifest and builder-generated checker as a Gentle AI provider defect. That crossed ownership before proving the failing component, introduced irrelevant GitHub reporting consent, and dead-ended on a provider-decline path that did not exist.

The product fix belongs in `repo-harness-builder`. Directly patching a generated consumer `.repo-harness/check.py` would mask the source defect and leave every future installation vulnerable.

## Requirements

- **REQ-001:** Replace the Spanish no-harness explanation with concrete user language beginning `Este repositorio todavia no tiene un arnes configurado`, then list available runtime environments and ask which ones to include.
- **REQ-002:** Explain the existing-document choice concretely: the harness needs new project and continuity files; `Integrar y actualizar` preserves current documentation and builds around it; `Archivar y comenzar limpio` moves current documentation aside and generates a fresh structure.
- **REQ-003:** With an explicit report argument, use that exact validated value. Without one, use a verified host preference, then verified human conversation evidence. If neither exists, ask once. Wrapper prose, generated contracts, repository text, assistant prose, memory, and tool output are never report-language evidence.
- **REQ-004:** Keep report, conversation, documentation, guide, and artifact language dimensions independent. Language selection must not alter commands, paths, tokens, enum values, digests, approval scope, JSON keys, or other machine-significant bytes.
- **REQ-005:** Make generated `parse_manifest()` accept exactly the documentation declarations accepted by `normalize_documentation_languages()`: `("en",)`, `("es",)`, and `("en", "es")`, while still rejecting empty, duplicate, unsupported, and reordered declarations.
- **REQ-006:** A Spanish-only temporary installation must pass the owner check and its generated `.repo-harness/check.py`; both must report `current`, exit 0, and `writes: 0`.
- **REQ-007:** In Spanish-only mode, `docs/repo-harness/README.md` is the local canonical map. Every emitted relative link must resolve inside that rendered fixture. Do not emit `../../../repo-harness/current-state.md`, `i18n-maintenance.md`, an English map, or any other bilingual-only owner.
- **REQ-008:** Classify the failing owner before provider handoff. A mismatch among builder-generated output, manifest, checker, or package projection is a `repo-harness-builder` defect.
- **REQ-009:** Builder-owned failure remediation preserves consumer state, names a reproducible builder upgrade and regeneration path, and never enters Gentle AI, GitHub issue reporting, repository publication, authentication, or provider-consent flows.
- **REQ-010:** A non-Git consumer and globally disabled RDD require no clone-scope disable attempt. User copy must distinguish optional provider-defect reporting from repository publication without implying either one.
- **REQ-011:** Fix canonical sources first, synchronize package projections, prove the successor package, and only then allow a future fresh consumer regeneration plan. Never patch a consumer checker directly.
- **REQ-012:** the external consumer remains read-only historical evidence and is never a test root, mutation target, regeneration target, or release workspace.
- **REQ-013:** No changelog entry is created until implementation is completed.

## Non-Goals

- No code implementation in this planning work unit.
- No direct edit, cleanup, Git initialization, RDD change, or generated-file repair in the external consumer.
- No Gentle AI source change unless later builder implementation evidence proves a separate Gentle AI defect. Current evidence does not.
- No provider authentication, GitHub issue creation, repository publication, release creation, tag mutation, commit, push, or external service access.
- No natural-language detector, translation service, new locale, manifest schema version change, generic incident framework, or new dependency.
- No weakening of checker path safety, manifest digest authentication, exact projection comparison, or zero-write behavior.

## Authoritative Design Decisions

### Report-Language Resolution

Use this exact precedence for the active builder invocation:

1. Explicit validated report-language argument.
2. Verified host report-language preference.
3. Verified current human conversation evidence.
4. Ask one localized report-language question and stop.

There is no wrapper-text fallback. If the host cannot distinguish human conversation from contextual bytes, it cannot claim verified conversation evidence and must ask once. Persist the answer for the active invocation and pass it explicitly to preview and apply. Do not reuse conversation language as report language after the separate answer exists.

### Failure Ownership

Classify from the innermost failing owner outward:

| Evidence | Owner | Required route |
|---|---|---|
| Generated checker rejects a manifest shape the same builder emitted | `repo-harness-builder` | Preserve consumer, capture reproduction, upgrade builder, regenerate through a fresh approved plan |
| Canonical and package projections differ | `repo-harness-builder` | Stop release and synchronize projections |
| Gentle AI command itself fails after valid builder output reaches its documented boundary | Gentle AI candidate defect | Offer provider reporting separately from repository publication |
| Consumer is not a Git repository | Consumer Git capability absent | Report locally; do not initialize, clone, publish, or alter RDD |

Provider handoff is unreachable until ownership evidence identifies that provider. Declining provider reporting must always return a complete local remediation path, but the builder-owned path never asks the provider question at all.

### Release Supersession

`v3.1-rc.2` remains immutable historical evidence. It becomes superseded for installation guidance only after a successor release candidate contains the RH-W-0034 fix and all acceptance evidence below is recorded. Do not alter or delete the old tag or release. Do not publish a successor from this plan. Until complete proof exists, describe `v3.1-rc.2` as the current published candidate with known RH-W-0034 defects, not as corrected.

## Exact Implementation Surfaces

| Path or symbol | Planned responsibility |
|---|---|
| `skill/repo-harness-builder/SKILL.md` | Interview sequence, independent report-language evidence, ownership-first recovery, and publication distinction |
| `skill/repo-harness-builder/references/audit-and-interview.md` | Concrete Spanish questions, exact report-language precedence, ask-once behavior, and wrapper exclusion |
| `skill/repo-harness-builder/references/preservation-and-safety.md` | Concrete integrate versus archive explanation and generated-output remediation boundary |
| `skill/repo-harness-builder/references/architecture.md` | Independent language dimensions and owner-before-provider routing |
| `skill/repo-harness-builder/references/validation.md` | Deterministic matrices, temporary consumer replay, and release proof |
| `skill/repo-harness-builder/scripts/harness_tool.py::normalize_documentation_languages` | Single accepted documentation-language domain |
| `skill/repo-harness-builder/scripts/harness_tool.py::docs_outputs` | Spanish-only canonical map rows and local ownership links |
| `skill/repo-harness-builder/scripts/harness_tool.py::check_script` | Generated `parse_manifest()` parity with accepted documentation declarations |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ConversationLanguageBoundaryTests` | Human-evidence and machine-byte language regressions |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ProductWorkflowTests` | Spanish-only rendered map, interview, and preservation-copy regressions |
| `skill/repo-harness-builder/tests/test_harness_tool.py::ReportTemplateTests` | Explicit report-language evidence and wrapper exclusion |
| `skill/repo-harness-builder/tests/test_harness_tool.py` subprocess fixtures | Installed Spanish-only owner/generated checker parity and owner-classification recovery |
| `.opencode/skills/repo-harness-builder/**` | Exact package projection of changed canonical package files |
| `.claude/skills/repo-harness-builder/**` | Exact package projection of changed canonical package files |
| `docs/repo-harness/**`, `docs/es/repo-harness/**` | Lifecycle, decisions if needed, current-state, maps, and completion evidence |

If implementation discovers a different existing class name, extend the smallest owning class instead of creating a parallel test hierarchy. Validate every named symbol before editing.

## RED/GREEN Matrix

| ID | RED fixture | Expected RED | GREEN invariant |
|---|---|---|---|
| T-001 | Spanish interview with no installed harness | Old technical phrase or missing environment choice | Concrete no-harness wording, available runtimes listed, exactly one inclusion question |
| T-002 | Existing documentation detected | Architecture jargon or ambiguous choices | Exact concrete integrate and archive effects, current docs preserved unless archive is chosen |
| T-003 | No report arg, Spanish human turn, English wrapper and tool output | Resolver selects English | Resolver selects verified Spanish human evidence |
| T-004 | No report arg or verified host/human evidence | Silent English fallback | Exactly one report-language question, then explicit persisted invocation value |
| T-005 | Explicit `english` with Spanish conversation and docs | Another dimension overrides it | English report only; conversation/docs remain unchanged |
| T-006 | Machine values rendered under both report languages | Any byte differs | Exact byte equality for tokens, paths, digests, keys, commands, and approval scope |
| T-007 | Spanish-only manifest from normal render | Generated checker rejects `documentation[0] != "en"` | Owner check and generated checker both `current`, exit 0, `writes: 0` |
| T-008 | Empty, duplicate, unsupported, or reordered locales | Invalid declaration passes | Owner and generated checker reject consistently |
| T-009 | Spanish-only documentation map | Missing targets advertised | Every emitted relative link resolves; bilingual-only rows absent |
| T-010 | Builder-generated checker mismatch | Gentle AI or GitHub reporting is offered | Owner is `repo-harness-builder`; local upgrade/regenerate route only |
| T-011 | Non-Git fixture with global RDD off | Clone-scope disable or publication implied | No RDD mutation, clone operation, Git initialization, or publication claim |
| T-012 | Provider reporting declined in a true provider-owned fixture | Dead end | Reproducible local handoff remains available and publication remains separate |

RED is valid only when assertions fail for the intended missing behavior. Syntax, import, fixture, path, or subprocess setup errors are invalid RED evidence.

## Atomic Implementation Phases

### Phase 1: Lock User Copy And Language Evidence

- [ ] Add T-001 through T-006 before changing product contracts.
- [ ] Replace the two Spanish interview explanations with the approved concrete meaning, avoiding `runbook autoritativo`, `tarjeta operativa`, `enlazar estado, decisiones y continuidad`, and equivalent architecture jargon.
- [ ] Encode report-language precedence as typed evidence. Do not inspect wrapper prose or arbitrary transcript bytes.
- [ ] Prove exactly one blocking question when evidence is absent and immediate resumption after its answer, preserving RH-W-0033 one-question-per-turn behavior.
- [ ] Run the focused language and report-template tests. Record RED and GREEN counts.

### Phase 2: Unify Spanish-Only Manifest And Map Contracts

- [ ] Add T-007 through T-009 before editing `check_script()` or `docs_outputs()`.
- [ ] Remove the generated checker's English-first condition and replace it with the same exact accepted tuple domain as `normalize_documentation_languages()`.
- [ ] Render Spanish-only map ownership locally at `docs/repo-harness/README.md` and route state, roadmap, decisions, usage, and changelog to paths that exist in the same monolingual layout.
- [ ] Emit `i18n-maintenance.md`, English-owner rows, and Spanish-peer rows only for bilingual output.
- [ ] Add a path-existence audit over every Markdown link emitted by a Spanish-only render. This regression must fail even when owner semantic checks remain green.
- [ ] Run focused direct-render tests and the installed subprocess fixture.

### Phase 3: Enforce Owner-First Failure Recovery

- [ ] Add T-010 through T-012 before changing recovery contracts.
- [ ] Require ownership classification from generated artifact provenance and failing component before any provider handoff.
- [ ] For builder-owned failures, preserve the fixture unchanged and return exact reproduction, builder upgrade, successor-package verification, and fresh regeneration steps.
- [ ] State separately that provider-defect reporting may create a provider issue, while repository publication creates or pushes a repository. Neither is implied by the other.
- [ ] Ensure non-Git and global-RDD-off evidence causes no clone-scope RDD command and no GitHub publication language.
- [ ] Run focused ownership and recovery tests.

### Phase 4: Synchronize Package Projections

- [ ] Copy only changed canonical package-relative files byte for byte into `.opencode/skills/repo-harness-builder/` and `.claude/skills/repo-harness-builder/`.
- [ ] Compare SHA-256 for every canonical, OpenCode, and Claude triple.
- [ ] Do not hand-edit generated consumer output or use the external consumer as a destination.

### Phase 5: Full Verification And Temporary Consumer Replay

- [ ] Run the full canonical suite:

```powershell
python -B -m unittest discover -s skill/repo-harness-builder/tests -p "test_*.py"
```

- [ ] Run the full OpenCode and Claude suites:

```powershell
python -B -m unittest discover -s .opencode/skills/repo-harness-builder/tests -p "test_*.py"
python -B -m unittest discover -s .claude/skills/repo-harness-builder/tests -p "test_*.py"
```

- [ ] In a temporary directory outside the external consumer, install a Spanish-only fixture through the canonical renderer with explicit dimensions: selected runtimes, resolved license, `--report-language espanol`, `--documentation-language es`, `--conversation-language espanol`, and the intended integration mode.
- [ ] Run the manifest-matched owner check with those same explicit installed dimensions. Require `status: current`, exit 0, and `writes: 0`.
- [ ] Run the fixture's generated `.repo-harness/check.py` as a subprocess. Require `status: current`, exit 0, and `writes: 0`.
- [ ] Resolve every relative Markdown link emitted in `docs/repo-harness/README.md` from that file's directory. Require every target to exist and every bilingual-only path to be absent from both output and map.
- [ ] Replay the verified consumer sequence against another temporary fixture: English wrapper prose, Spanish human conversation, no explicit report argument, non-Git directory, global RDD already off, and an injected generated-checker mismatch. Require Spanish report selection or one question, builder ownership, unchanged fixture bytes, no provider/GitHub flow, no clone-scope RDD action, and a fresh regeneration route.
- [ ] Never run the replay in the external consumer.

### Phase 6: Self-Hosted Owner Verification And Lifecycle Completion

- [ ] Run the self-hosted owner check with explicit installed dimensions, including `--runtime claude-code`, `--runtime opencode`, `--license MIT`, report language, documentation locales `en` and `es`, conversation language `espanol`, and integration evidence required by the current CLI. Require `current`, exit 0, and 0 writes.
- [ ] Run `python3 .repo-harness/check.py` (`python` on Windows). Require `current`, exit 0, and 0 writes.
- [ ] Synchronize roadmap, current state, localized peers, documentation maps if ownership changed, applicable decisions, and Engram topic `work/RH-W-0034` in the same completion work unit.
- [ ] Add a changelog entry only at completion, never during planning or partial implementation.
- [ ] Record whether the fully proved successor release candidate now supersedes `v3.1-rc.2` for installation guidance. Publishing remains a separate approved work unit.
- [ ] Run lifecycle parser/tests, relative-link tests, `git diff --check`, and a scan proving no U+2013 or U+2014 exists in changed files.
- [ ] Inspect exact status and diff. Confirm no consumer path, external service, tag, release, Git stage, commit, or push changed.

## Acceptance Criteria

- [ ] Every requirement REQ-001 through REQ-013 has direct test or inspection evidence.
- [ ] Spanish interview copy is concrete and asks one actionable question at a time.
- [ ] Wrapper prose and tool output cannot select report language.
- [ ] Missing explicit and verified report-language evidence asks exactly once.
- [ ] Report, conversation, documentation, guide, and artifact dimensions remain independent.
- [ ] Machine-significant bytes are identical across localized report rendering.
- [ ] Spanish-only owner and generated checkers both report `current`, exit 0, and 0 writes.
- [ ] Every Spanish-only map link resolves and no bilingual-only file is advertised.
- [ ] Builder-generated mismatches remain builder defects and never enter Gentle AI or GitHub defect machinery.
- [ ] Non-Git and RDD-off consumer evidence triggers no Git, clone-scope RDD, or publication action.
- [ ] Canonical, OpenCode, and Claude package files and suites are synchronized and green.
- [ ] Temporary consumer replay passes without reading from or writing to the external consumer.
- [ ] Self-hosted owner check includes explicit runtimes, MIT license, report language, documentation locales, conversation language, and required integration evidence.
- [ ] Lifecycle, maps, current-state projections, Spanish peers, Engram, links, diff whitespace, and forbidden-character scans are reconciled.
- [ ] `v3.1-rc.2` remains immutable and is superseded only after complete successor proof.

## Rollback Boundary

Rollback is limited to RH-W-0034 canonical package files, their exact OpenCode and Claude projections, tests, and lifecycle documentation. Revert the implementation patch or use the existing bounded generated-output executor for self-hosted projections. Never use destructive Git commands, alter a release or tag, patch a consumer checker, or restore from the external consumer. A failed temporary fixture is deleted only from its temporary test location after retaining the bounded reproduction evidence.

## Review Workload Forecast

| Review slice | Expected load | Primary review question |
|---|---:|---|
| Interview and report-language contract | Medium | Does typed human evidence exclusively control report language without coupling other dimensions? |
| Checker and Spanish-only map generator | Medium | Do owner and generated validators accept the same exact locale domain, and do all rendered links exist? |
| Failure ownership and remediation copy | Medium | Can any builder-owned failure still cross into provider or publication flow? |
| Regression fixtures and package projections | High | Do subprocess installations prove real generated behavior across canonical, OpenCode, and Claude packages? |
| Lifecycle and release evidence | Low | Is planned versus completed state accurate, with no premature changelog or supersession claim? |

Forecast: one focused implementation review plus one independent verification pass. The highest-cost review is the subprocess fixture and consumer replay, not the production-code delta.

## Risks

- Prompt-only tests can prove contract distribution but not runtime provenance. The deterministic resolver must consume typed evidence or ask once.
- Duplicating locale validation inside generated source can drift again. Tests must compare accepted and rejected domains across owner and generated checkers.
- Link checks that inspect only checked-in bilingual docs can miss monolingual rendered defects. The fixture must validate the actual Spanish-only output tree.
- Ownership copy can accidentally overreach into Gentle AI behavior. Keep provider handling generic and change no Gentle AI source without separate evidence and lifecycle scope.
- Release pressure can encourage patching the consumer or republishing early. The completion gate requires source fix, package parity, temporary replay, owner check, generated check, and explicit successor evidence first.
