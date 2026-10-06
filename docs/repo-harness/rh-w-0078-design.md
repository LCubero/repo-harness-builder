# RH-W-0078 Thin Entry Contracts — Approved Design

**Verified fact:** on 2026-10-04 the owner explicitly approved this bounded bilingual design and reproducible measurement procedure by answering “dale” to the design-approval question that kept implementation planning, product changes, runtime experiments and delivery separate. This follows the separately authorized design preparation; method approval is not runtime or safety evidence. [RH-W-0078](roadmap.md#rh-w-0078) is now `active`, `P2`: the owner explicitly authorized RH-W-0078 implementation and issue #266 was confirmed. The owner separately authorized preparing the [implementation plan](rh-w-0078-implementation-plan.md) and approved it on 2026-10-05 for documentation recording only; these historical approvals do not constitute runtime evidence. The previous baseline-before-start gate is removed. Runtime evidence remains unknown and is required before claiming measured savings or accepting effectiveness. External tool changes, runtime experiments and delivery remain outside this task's authority. This design is not an implementation plan.

## Recommendation and Boundary

**Supported inference:** prefer thin entry contracts with explicit conditional, mandatory-before-action reference gates, not wholesale relocation. The [approved scope](rh-w-0078-scope.md) and [local research](rh-w-0078-gentle-ai-binary-research.md) support this direction, but do not prove effective savings. Include builder compression and bundled ceiling-scope wording transferred from completed RH-W-0076. Keep documentation as an unchanged preservation control unless measurements prove a shared change necessary.

Keep deliberate activation, never-miss safety, human authority, gate predicates, required read order, and failure behavior in the entry. Move phase-specific detail only behind its named gate; each gate names a repository-local reference and requires loading it before the relevant action. Retain a concise action-point reminder when distance matters. Missing or unsafe required references stop the affected action, never silently waive an obligation. No new store, dependency, binary, runtime, or automatic authority is proposed.

### Approved Design Selections for Proposed Exact Reference Gates

**Verified fact:** the following paths and predicates are approved design selections, not installed behavior. In this table `B` means `skill/repo-harness-builder/references/`, `S` means `.repo-harness/skills/repo-harness-session-start/references/`, and `T` means `.repo-harness/skills/repo-harness-session-stop/references/`; concatenate each prefix with the listed filename. Builder names are existing except `authority-correction.md`; every `S`/`T` reference is **PROPOSED**, not an existing file.

| Canonical reference | Predicate and mandatory-before-action order |
| --- | --- |
| `B` + `audit-and-interview.md` | Active builder inspection: before repository audit and the language/profile interview under current Execution Steps 1–2; candidate check-only exception below is not baseline. |
| `B` + `architecture.md` | Installation/update planning: before capability composition and output selection. |
| `B` + `license-selection.md` | Installation/update license resolution: before plan formation; preserve the existing unresolved-license decision gate. |
| `B` + `preservation-and-safety.md` | Installation/update: before ownership audit, replacement preview or apply. Entry retains containment for every inspection. |
| `B` + `validation.md` | Applied harness verification: before validation and readiness reporting. |
| `B` + `authority-correction.md` (**PROPOSED**) | Opt-in builder authority context: before context construction/preview and again before bound apply; never applies to maintenance authority. |
| `S` + `reconciliation.md` | One of the four named deeper triggers requires reconciliation: before scoped evidence comparison or authoritative routing. |
| `S` + `checkpoint.md` | A directly discovered checkpoint requires processing: before classification/resume proposal; any deeper owner read must satisfy one of the four existing triggers. |
| `S` + `synchronization.md` | Requested synchronization is relevant: before upstream/transport inspection, remembered-grant use or network consent/action. |
| `T` + `checkpoint.md` | Fresh explicit checkpoint request: before guarded write/readback. |
| `T` + `git-commit.md` | Requested staging/commit: before staged-scope inspection, message approval, staging or commit. |
| `T` + `git-transport.md` | Requested push: after local eligibility and before push preflight, consent or transport. Commit-only does not load it. |

Every required gate retains its predicate, ordering and failure reminder in the entry: missing, unowned, unsafe or unreadable required references stop the affected action with evidence; they never waive consent or obligations. Stop retains verification → applicable documentation → native continuity → eligibility unconditionally, with independent optional Engram outcome. Documentation remains unchanged with ownership, parity, finding dispositions and response-contract gates intact.

Start retains unconditional exact-path checkpoint discovery **after native continuity recovery**, including `not_found` for absence, in the entry. Only processing/reference loading is conditional. Its exclusive deeper-owner triggers remain: explicit objective or `RH-W-####`, contradiction, invalid or missing priority evidence, mandatory reconciliation. Checkpoint presence is not a fifth trigger; classify from bounded discovery input, and require an existing trigger before additional owner evidence. Unresolvable required evidence stops the proposal, not the discovery.

For each table path, the exact runtime projection replaces `skill/repo-harness-builder/` or `.repo-harness/skills/<name>/` with `<base>/skills/<name>/`, where `<base>` is `.claude` for `claude-code`, `.opencode` for `opencode`, or `.pi` for `pi`, and the builder name is `repo-harness-builder`. Only selected runtime profiles contribute paths. **Proposed ownership requirement:** the existing renderer derives a finite exact path list from these enumerated filenames, canonical skill names and validated selected profiles; the authenticated manifest declares that list and the checker verifies its schema, exact derived equality, provenance and canonical/projection bytes. Extend `generated_skill_paths()` and replacement/upgrade bindings to those exact files, with current-digest approval, unknown/human bytes protected and no recursive directory ownership. The same derived list feeds projections and one exact LF attribute rule per byte-compared path; legacy manifests never authenticate newly proposed references by proximity. No new manifest store or product change is authorized here.

## Preservation Matrix

**Verified fact:** the following local owners contain the current obligations. **Design requirement:** preserve them before their action in every selected runtime; a body-size win cannot compensate for loss.

| Owner | Required preservation |
| --- | --- |
| [Builder](../../skill/repo-harness-builder/SKILL.md), [safety](../../skill/repo-harness-builder/references/preservation-and-safety.md), [architecture](../../skill/repo-harness-builder/references/architecture.md) | Deliberate activation and strict current digest approval versus conversational maintenance consent; independent interview language/runtime/license flags; stateless authority context, target/plan/generation bindings and fresh-preview refusal; guarded root/ancestors and honest containment limits; human/unknown bytes protected. |
| [Start](../../.repo-harness/skills/repo-harness-session-start/SKILL.md) | Charter, language/preferences, newest valid backup resume-first order; exactly four deeper-read triggers; bounded evidence/chronology/dependents; provenance-aware language resolution; independent optional capabilities; no automatic work, mutation or consent reuse. |
| [Stop](../../.repo-harness/skills/repo-harness-session-stop/SKILL.md) | Successful verification before applicable documentation, continuity and eligibility; local-first privacy-filtered backup with independent Engram outcome; checkpoint requires fresh explicit authority; exact staged scope and pre-existing index separation; no AI attribution and no silent approved-message rewrite; commit-only never approves push. |
| [Runtime architecture](../../skill/repo-harness-builder/references/architecture.md) | Canonical product source versus generated neutral skills and projections; disk parity versus active-process cache freshness; thin registered-name command/prompt entry surfaces; no invented reload or mandatory live restart. |
| [Renderer/checker owner](../../skill/repo-harness-builder/scripts/harness_tool.py), [LF policy](../../.gitattributes) | Authenticated exact generated-reference ownership, checker schema/owned-path coverage, replace/upgrade transaction and provenance, all selected runtime targets and explicit LF rules must evolve together under later authority. Current `generated_skill_paths()` owns operational `SKILL.md` files only: never trust arbitrary recursive directories or infer ownership from a link/name/prefix. Human references remain protected. |
| [Documentation control](../../.repo-harness/skills/repo-harness-documentation/SKILL.md), [rulebook](../../REPO-HARNESS.md) | One canonical owner, complete localized peers, factual chronology, evidence classes, human approval and immediate-before-response obligation remain reachable and timely. |

## Four Measurements, Not One Token Claim

| Quantity | Definition and limitation |
| --- | --- |
| Entry body proxy | Existing estimate `round(len(text.split()) * 1.33)` after the frontmatter rule below; not a tokenizer or observed runtime cost. |
| Static local reachability | Unique safe files reachable from explicit local references and instruction-named paths, whether mandatory or conditional. A link alone proves neither a required read nor a runtime read. |
| Ordered mandatory/phase closure | Required read events in contract order for a pinned scenario, with gate evidence; report phase totals, cumulative unique volume and repeated-read volume separately. Unconditional references count fully, so relocation earns no savings. |
| Actual runtime consumption | Unknown without actual runtime/tool traces: injected wrappers/metadata, whole or partial tool results, implicit resources, repeat reads and truncation may differ from static closure. Cache effects and tokenizer totals need explicit runtime evidence. |

Count UTF-8 source bytes, whitespace words and proxy separately. For body measurement only, exclude frontmatter iff the first line is exactly `---` and a subsequent line is exactly `---`; remove both delimiters and all intervening lines, otherwise exclude nothing and flag malformed opening frontmatter. No YAML parsing or heading heuristic. Closure counts whole files including frontmatter, comments and code by default. A heading link never implies section-only reads. Section-only runtime evidence must specify exact returned bytes and must not change the static whole-file baseline.

Historical proxies retained in the [roadmap](roadmap.md#rh-w-0078): builder 3042, start 2707, stop 2822, documentation 878. These are historical evidence, not measurements of this draft or current runtime consumption.

## Pinned Scenario Matrix

Run the same rows separately for `claude-code`, `opencode`, and `pi`, and for each declared locale/configuration being compared. Pin integration per row; standalone/integrated are not interchangeable.

| Family | Scenarios and discriminating gates |
| --- | --- |
| Builder | Explicit inspect/check intent selects a read-only active-builder branch using only the existing check and validated declared configuration; no inferred runtime, profiles, language, or capabilities. Necessary missing/untrusted evidence reports the limitation and stops. Direct CLI check remains a separate read-only rendered comparison; install/update and authority correction retain their existing gates. |
| Start | Resume-only with valid priority and no deeper trigger; named objective (only its owner/evidence); checkpoint (discovery after native recovery, current evidence then fresh resume authority); sync (current exact upstream and separate network consent, invalidation on material change). |
| Stop | Local handoff (verification/docs/native continuity/eligibility, no Git); requested checkpoint (fresh explicit request, guarded write/readback); commit-only (scope/message consent and staged check, no push); commit+push (same bounded consent, refreshed preflight and transport failures preserved). |
| Documentation | Preservation-only control with applicable canonical/localized owners and response gate; no savings target imposed on an unchanged compliant body. |

**Verified fact:** builder Product Identity says no operation checks read-only, but active Execution Steps 1–4 still require audit/interview, language/profile questions and installation references. **Human decision (2026-10-06):** explicit inspect/check intent selects a read-only active-builder branch using the existing check and validated declared configuration only; it infers no runtime, profiles, language, or capabilities. If necessary evidence is missing or untrusted, report the limitation and stop. This branch authorizes no apply, repair, synchronization, or Git action. Installation, update, and correction gates remain unchanged. This approval is design-only: it grants no implementation, runtime experiment, native verdict, savings claim, commit, or delivery authority. The existing CLI `check()` remains a separate read-only rendered comparison, not a substitute for this branch.

For dynamic owners, backups, checkpoint and Git evidence, use fixed non-sensitive in-memory fixtures with declared path, bytes, SHA256 and gate outcome; or declare an excluded-variable bucket separately. Never silently treat missing dynamic content as zero, read private local state for sizing, or claim excluded variables are part of total context. Pin optional capability outcomes and repository shape as scenario inputs.

## Reproducible Static Baseline Procedure

This explicit stdlib/read-only pseudocode is a procedure, not a new script/tool or permission to run experiments. The checked-in baseline is commit `d30f8b442288eeb180257a46ccc4bb1b64688f5c`; record the local documentation delta separately. A future authorized renderer candidate is a separate in-memory output mapping, never a generated apply or replacement of this baseline.

```text
pin = {commit, runtime, documentation_locales, conversation_language,
       report_language, integration_mode, license, project_identity,
       scenario, capability_outcomes, repository_shape, count_rule_version}
assert baseline HEAD == pin.commit; record any scoped diff separately
select exact skill and wrapper paths from authenticated manifest/config:
  builder source skill/repo-harness-builder/SKILL.md; its runtime package peer
  daily canonical .repo-harness/skills/<name>/SKILL.md; runtime skill peer
  Claude/OpenCode command families; Pi prompt family; required root pointer
pin renderer, manifest, checker, entry, wrapper and each read-source SHA256
classify raw Markdown destinations before deriving read locators:
  split fragment from path; same-file anchors are section edges, not new files;
  validate anchors against the target, never treat them as section-only reads
  queries are unsupported/unresolved local edges, never stripped silently;
  external http/https links are non-read edges (no network); other schemes
  are unsupported; reject drive-qualified, UNC and root-relative paths
  normalize decoded relative path against containing file's repository-relative
  parent (instruction-named paths use their declared base); allow ../../ only
  when lexical normalization remains inside the validated repository root;
  reject malformed/ambiguous encoding and encoded separators; no alias following
safe_read(normalized_locator): require contained repository-relative components
  with no remaining parent segments, drive, UNC or absolute prefix;
  guard root and every ancestor before reading, rejecting symlink/reparse,
  indirect, non-directory ancestors and non-regular/escaping final targets;
  read UTF-8 strictly; return bytes and hashlib.sha256(bytes).hexdigest()
  never recurse a directory; cooperative filesystem checks retain race limits,
  not a hostile-filesystem security subsystem
inventory explicit Markdown links plus instruction-named local paths;
  classify examples and placeholders as non-read edges unless required;
  flag unresolved edges; inventory membership itself creates no read event
visit reachable safe local files once (visited set); record owner for each;
  this graph is reachability, not automatic mandatory traversal
for each scenario: manually derive ordered read events from exact clauses:
  (phase, ordinal, path, source_clause, gate_predicate, gate_input, gate_outcome)
  include wrapper, entry, root instructions, unconditional reads and triggered
  dependencies only where exact scenario clauses require them; canonical and
  projection copies count together only when those clauses require both
  expand recursive obligations to a finite ordered sequence with clause-defined
  stopping conditions; retain repeated events, never silently deduplicate reads
  ambiguity or unbounded cycles makes that scenario unmeasured, not a broken cycle
  reject ambiguous order/gates rather than guessing; record skipped gates too
for each phase: volume = sum(full_file_words(event.path) for event in phase)
  cumulative_unique = sum(full_file_words(path) for path read so far once)
  cumulative_repeats = cumulative_event_words - cumulative_unique
  report bytes/words/proxy per file and phase; sum per-event rounded proxies
  separately from round(total_words * 1.33), since rounding may differ
compare baseline and future candidate using identical pin scenarios/count rules;
  attach exact required-read sequence, gate evidence, ownership/link failures
```

Use `pathlib`, `hashlib` and ordinary stdlib text counting in memory; output evidence to the session, with no new persistent measurement store. Source hashes identify inputs, not human authority. A checked-in integrated installation does not supply fresh standalone outputs: mark unavailable configurations unmeasured until a separately authorized in-memory renderer baseline exists. No numerical result is invented here.

## Acceptance and Remaining Gate

- Nominate resume-only start and local-handoff stop as common paths; candidate static mandatory closure must be strictly lower than the same pinned baseline for those rows in every runtime/configuration claimed. Also require a lower active builder inspection/check-only closure once its proposed branch is resolved; keep direct CLI check separate, and report installation/correction alternate paths without hiding increases.
- Builder and generated entry bodies must be at most the existing 1000 proxy maximum; rulebook budget remains 700 with its existing assertion semantics. Budgets are development assertions, never render/check refusals. Bundled wording must explicitly bind builder and every generated skill without raising either ceiling.
- Every preservation obligation is present before its action; no dangling, escaping, unowned reference or changed human consent/safety. Missing required resources prevent the affected action. All canonical/runtime projections must be equivalent, with authenticated exact-path and LF coverage rather than directory trust.
- Static acceptance is necessary, not sufficient: runtime pass requires an actual measured consumption decrease on the named claimed common paths (resume-only start, local-handoff stop and resolved active builder inspection/check-only), comparing identical baseline/candidate scenarios in each runtime/configuration claimed. Pin version/config, fresh-versus-cached process state, exact prompt/tool-read traces and ordered gate compliance; compare actual returned/injected bytes and whitespace-word proxies, or tokenizer counts only with explicit tokenizer evidence. No arbitrary percentage, static substitute or savings inferred from body size. Comparable observations with no decrease fail the claimed path; unavailable, ambiguous or incomparable observations are unmeasured, never pass. This requirement remains unmet; no runtime observations or savings are claimed and live sessions are not required merely to draft this design.

**Tradeoff:** conditional retrieval can lower mandatory context but risks missed/distant obligations; explicit before-action gates and future negative/missing-reference tests protect the contract. Pure relocation is simpler but earns no savings when all references remain mandatory. Model-facing mechanical checks are candidates only for a later authorized product change, not implemented safeguards.

**Verified authority:** RH-W-0078 implementation is explicitly owner-authorized; issue #266 was confirmed. The former current-runtime baseline-before-start gate no longer applies. Runtime observations remain unknown and are required before any claim of measured savings or acceptance of effectiveness; this documentation update claims neither. External tool changes, runtime experiments and delivery are not authorized by this task.

**Current authorization update:** the user's explicit “si dale” authorizes the bounded I1 implementation of the approved read-only active-builder inspect/check branch, its focused canonical tests, and minimum bilingual authority/status reconciliation. It does not authorize the later generated-reference ownership/projection work, projection synchronization, full-suite claim, runtime experiment, native verdict, savings claim, Git action, commit, or delivery. The branch must use the existing installed checker and authenticated declared configuration, fail-stop on missing or untrusted evidence, and leave installation/update/correction flows unchanged.
