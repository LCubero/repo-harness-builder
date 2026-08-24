# RH-W-0056 Findings: External Harness Review

<!-- doc-role:research -->

Canonical findings owner for [RH-W-0056](roadmap.md#rh-w-0056). The Spanish peer is [`../es/repo-harness/rh-w-0056-findings.md`](../es/repo-harness/rh-w-0056-findings.md).

## Scope And Method

A read-only inspection of one external harness the owner built and owns, performed on 2026-08-22. Nothing in the reviewed repository was modified, executed, or copied here; its working tree and `HEAD` were verified unchanged after the review.

The reviewed harness is called **the external harness** throughout. Its project identity, absolute paths, hosts, credentials, team names, customer data, and domain numbers are deliberately absent, because a portable pattern does not need them and this repository does not own them.

### What was read

The complete harness surface: the root rulebook and all three root pointer files; the six rule files it gates to; all six skills in the runtime pack, including every reference and banner asset; the control index document; every control script docstring plus the two largest control implementations in full; the ignore and attribute files; the pack installation guide; the generated skill registry; and the product README's conventions and session-command sections.

### What was not read, and why

The deployment target references, the audit and backup workflow bodies beyond their rules and hazards, the product README's data-platform sections, and the business documentation, notebooks, and application code. These are domain operations detail, not harness patterns. Where a lesson from them reached a rule, a hazard file, or the control index, it was read there. **No claim in this document rests on those files, and no absence claim covers them.**

### How to read a finding

Every finding is a **pattern**, restated in this repository's vocabulary. None is a copy of the reviewed implementation, and none is adopted by this document. Adoption is a separate owner decision, and each adopted pattern needs its own work unit with its own design, tests, and delivery evidence.

**Classification**: `integrate` means this repository should own the pattern; `inspire` means the insight is valuable but the mechanism here would differ; `already present` marks a pattern this repository independently arrived at, recorded because convergence is itself evidence; `reject` means it does not belong; `out of scope` means it addresses a problem this product does not have.

## The Three Structural Insights

**1. One source, N copies that nobody synchronizes.** The external harness states that almost every control it owns catches this one shape, and it reached that conclusion independently. It is the same shape this repository's generated checker owns. Independent convergence raises it from a design preference to a supported inference about the problem class.

**2. A note is not a control; a control lives in the executable path.** Several of its controls exist because a rule written in prose failed silently for weeks while everyone believed it was being followed. One case is worth stating exactly: a fix was written up in memory as the answer, and then the same failure happened again in a later session, because the write-up lived only in memory and never reached the file that runs. This repository defends several obligations with prose, and `RH-W-0038` already recorded that a prose rule cannot be mechanically enforced. The external harness's answer is not to weaken the rule but to find the mechanizable half and mechanize that half.

**3. A hazard that can recur needs a guard, and the second occurrence is the trigger.** A documented risk is read by whoever already suspects it, never by the person about to repeat it. On the second occurrence, stop fixing the instance and find what lets the system allow it.

## Group A: Controls And Verification

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| A1 | **Discovery-based control runner.** One runner walks the tools tree and executes every `test_*.py` it finds. Adding a control costs one place instead of five. Measured origin: eleven test files existed and nothing ran any of them, no CI, no runner config, no script. Ten of the eleven passed by hand. | Verified fact from the runner source | `integrate` |
| A2 | **Zero discovered is not a pass.** A runner that finds no tests exits non-zero: an empty result answers for the probe, not for the world. | Verified fact from the runner source | `integrate` |
| A3 | **Self-testing detector.** A content checker runs its own detector against a string it knows is present and refuses to report clean if the detector fails. Origin: a shell verification that returned zero on a file containing two, because the shell did not expand the escape being searched for. | Verified fact from the checker docstring | `integrate` |
| A4 | **Tolerant verdict that is not a blindfold.** A failing control separates a missing third-party library, which is a machine difference, from a missing module the repository owns, which is a real break. Without the split, moving an internal file reports as healthy. | Verified fact from the runner source | `inspire` |
| A5 | **Cadence controls, reported and never enforced.** A control whose trigger is time rather than a file change carries a tracked stamp file, prints only when overdue or never run, and never changes the exit code. The stamp is tracked on purpose because the cadence belongs to the team, not to one laptop. Only a clean run is stamped: recording a run that found something would restart the clock over unaddressed findings. | Verified fact from the runner source and control index | `inspire` |
| A6 | **Failing output is printed in full, never summarized.** A verdict without its reason sends the reader back to reproduce it by hand. | Verified fact from the runner source | `integrate` |
| A7 | **Controls live in the repository, not in the distributed pack.** A pack installed elsewhere must report the script is absent and fall back to a manual check. | Verified fact from the control index | `already present` |
| A8 | **An exit code is not proof a run succeeded.** A wrapper reported exit code 0 while its log held the traceback of a run that died before writing anything. | Verified fact from the backup hazards file | `integrate` |
| A9 | **An absent control is reported and skipped, never failed.** The pack ships to projects that have no control directory at all. | Verified fact from the session-stop skill | `already present` |
| A10 | **A control that did not run is never reported as passed.** Stated as a correctness rule, not a formatting preference: the exact confusion let a documentation check report clean for months while checking nothing. Three of its controls can fail to *run*, which is different from failing. | Verified fact from the session-stop skill and control index | `integrate` |
| A11 | **A clean sweep reports one line.** Name an individual control only when it found something, fixed something, or could not run. A wall of green lines is noise, and noise is what teaches people to skim the one line that mattered. | Verified fact from the session-stop skill | `integrate` |
| A12 | **A map of anchors carries its own self-check.** The documentation map opens with a runnable snippet that lists any anchor that no longer resolves, run before using the map, with the instruction to fix the map as part of the change already underway. | Verified fact from the doc map | `integrate` |

## Group B: Projection And Packs

Bears directly on the open `RH-W-0057`.

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| B1 | **The projection check covers commands, not only skills.** Both the skills directory and the command directory of every runtime pack are compared against the runtime copy. This is exactly the gap `RH-W-0057` records here. | Verified fact from the pack checker | `integrate` |
| B2 | **Five verdicts, not two.** `MISSING`, `STALE`, `EXTRA`, `PACK` for packs disagreeing with each other, and `HARNESS` for a root pointer that stopped pointing at the rulebook. `EXTRA` is reported and never touched: it is where local experiments live. | Verified fact from the pack checker | `integrate` |
| B3 | **Direction is always source to runtime, and apply never deletes.** A differing runtime file cannot say by content alone whether the source moved ahead or somebody edited the runtime without porting. The rule decides; the losing side is preserved. | Verified fact from the pack checker | `already present` |
| B4 | **Harness-owned root files are bootstrapped once and never overwritten.** The runtime rewrites its own pointer file, so replacing it destroys what accumulated there. The only obligation is that it still points at the rulebook. | Verified fact from the pack checker | `already present` |
| B5 | **Per-runtime vocabulary lives in data, not in branches.** One runtime names its command folder `prompts/`; the checker carries that as a per-pack tuple. | Verified fact from the pack checker | `already present` |
| B6 | **Runtime folders are gitignored; only the source pack travels through git.** Stated as a consequence in the rulebook: a skill edited only at the runtime is not versioned and is lost on a fresh clone. | Verified fact from the ignore file and rulebook | `inspire` |
| B7 | **The measured failure that created the control.** The three packs agreed on a skill while two of the three runtimes still held an earlier version. Nobody noticed because the person editing sees their own runtime working. | Verified fact from the pack checker | `integrate` |
| B8 | **A harness file is compared by heading, and the repair only ever adds.** The pack copy is the *minimum* the runtime must contain: copied whole when absent, then compared only by `## heading`, so a missing required section is appended. The runtime may carry extra sections. Nothing replaces. | Verified fact from the start workflow | `integrate` |
| B9 | **Only the runtimes actually installed are checked, and the verdict names them.** A runtime this machine does not have is omitted, not an error. | Verified fact from the start workflow | `integrate` |
| B10 | **Drift is reported and the repair is offered, never applied unasked.** The losing side may hold unported work, so the human decides. | Verified fact from the session-start skill | `already present` |
| B11 | **When no canonical source is designated, parity is proved by hash and stated as such.** One skill's reference carries an explicit hash-parity snippet across its three pack copies, plus the instruction not to introduce a fourth source, label a pack as generated, or claim automatic synchronization that does not exist. | Verified fact from the audit workflow | `inspire` |

## Group C: Transport, SSH, And Cross-Platform

Bears directly on the open `RH-W-0046` and on the generated session gates.

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| C1 | **Transport auto-detection with an ordered fallback, for fetch and for push.** Resolve in order and stop at the first that works: a provider CLI credential helper when authenticated and the host matches, then native Git, then fail loudly with a banner naming the exact command the human must run. A push that completed over any transport is success. | Verified fact from both session workflows | `integrate` |
| C2 | **The provider path must pass an explicit HTTPS URL.** Overriding the remote URL through configuration does not reliably beat an existing SSH origin and silently falls back to SSH. | Verified fact from both session workflows | `integrate` |
| C3 | **Every network step is non-interactive, because an agent has no TTY.** A passphrase-protected key or an uncached credential opens a prompt nobody can answer, and the session hangs with no error, no banner, and no timeout. Batch mode for SSH, a terminal-prompt kill switch for Git, and a bounded connect timeout. On the stop path this is worse: the memory is already saved and the commit may already exist, so the user sees a frozen session instead of a banner. | Verified fact from both session workflows | `integrate` |
| C4 | **Never write an environment assignment as a command prefix.** POSIX-only; fails on PowerShell before the command runs, on the exact platform the guard protects. | Verified fact from both session workflows | `already present` |
| C5 | **The upstream shorthand must be quoted.** Unquoted, PowerShell can parse it as a hashtable expression. This repository uses the shorthand in prose without stating the hazard. | Verified fact from both session workflows | `integrate` |
| C6 | **Up to date is a success, never a failure.** Level with the upstream, or ahead with unpushed commits, needs no integration. | Verified fact from both session workflows | `already present` |
| C7 | **Known limits of the mechanism are documented next to it.** The SSH command override is ignored when the corresponding environment variable is set, and the terminal-prompt kill switch does not silence a credential helper's graphical prompt. | Verified fact from both session workflows | `integrate` |
| C8 | **Integration is a local merge of the already-fetched upstream, never a re-fetching pull.** A plain pull re-fetches over the default transport and fails for users who only have the provider path. | Verified fact from the stop workflow | `integrate` |
| C9 | **Cross-platform hazards are recorded as measured incidents, not warnings.** A checksum tool whose output format differs between two shells and already produced a false positive; one operating system that normalizes filenames differently, named as the untested environment rather than assumed safe; a cloud CLI that resolves to a batch shim on Windows so a bare subprocess call raises before the first request; and a shell alias on Windows that opens a different subsystem instead of the expected interpreter. | Verified fact from the control index and backup hazards | `inspire` |
| C10 | **A timezone prefix is not portable and fails silently.** Taking the date with a `TZ=` prefix is ignored without error by one Windows shell and returns UTC, which after 18:00 local is tomorrow, so everything is dated one day forward and nobody notices by reading. The rule is a bare local `date` plus an explicit conversion helper. | Verified fact from the working agreements | `integrate` |
| C11 | **Line-ending configuration can fight the attributes file and fake a content change.** With autocrlf on, an exporter that normalizes line endings emits one spurious update per affected file; verify against the committed blob before recording a content change. | Verified fact from the backup hazards | `integrate` |
| C12 | **Reviewing repository state includes the branches, not only whether the tree is clean.** Measured the same day: an audit covering documentation, code, harness, controls, and deployment never ran a branch listing, and a fifteen-day-dead merged branch kept appearing and raising doubt. What settles deletion is an ancestry check against both the local and the remote tip, never the name or the date. | Verified fact from the working agreements | `integrate` |

## Group D: Documentation Lifecycle

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| D1 | **One canonical document per decision; others may mention it but must link.** Pointing is allowed; restating without pointing is what rots. Measured origin: one decision's status was rewritten in fifteen files in each one's own words, and three told a reader to do work that was no longer needed. | Verified fact from the decision-status checker | `integrate` |
| D2 | **An ambiguous status fails instead of being guessed.** Guessing is how a still-open item passed as closed. | Verified fact from the backlog checker | `already present` |
| D3 | **An open item missing from the summary table people actually read is a failure.** State kept in two places inside one document is the same one-source shape at document scale. | Verified fact from the backlog checker | `already present` |
| D4 | **A claim that cannot be checked fails rather than warns.** An item asserting a state without citing the evidence that would settle it is not wrong, it is uncheckable. The checkable half is settled by a machine ancestry comparison, not by prose. | Verified fact from the backlog checker | `integrate` |
| D5 | **A generated copy announces in its own first line that it is generated and where its source is.** | Verified fact from the control index | `already present` |
| D6 | **A synchronization check must cover the drift shape that reads perfectly.** Three shapes: a copy edited by hand, a copy missing, and a copy that survives after its source was withdrawn. The third is the worst: no visible contradiction, it reads correctly, and it states something the team stopped standing behind. | Verified fact from the control index | `integrate` |
| D7 | **A large index is grepped, never read whole, and the rulebook states its token cost.** | Verified fact from the external rulebook | `inspire` |
| D8 | **Never create a file whose only job is to point at documents living elsewhere.** A signpost looks like navigation and is actually a second index nobody updates. Theirs outlived several moves while describing an older layout. Move the documents and delete the signpost. | Verified fact from the docs hazards | `integrate` |
| D9 | **A move breaks links in both directions, and the visible label is the third failure.** The moved file's own relative links break although the file was never edited. Links must be resolved against their own source and recomputed, never text-substituted. A correct link under a lying label is worse than a broken one, because nothing reports it. | Verified fact from the docs hazards | `integrate` |
| D10 | **The decision registry is a machine structure, not prose.** Registering a new decision means adding one row to a dictionary inside the checker. | Verified fact from the docs skill | `integrate` |
| D11 | **Sweep again after building, not only after investigating.** When something is built that the documents called blocked, every document stating the old status is now wrong. Prefixing a superseded notice is not enough: the index row repeated the old plan in the same cell as the notice. | Verified fact from the docs hazards | `integrate` |
| D12 | **A deliberate duplication is marked as deliberate so a future cleanup does not delete it as drift.** Exactly one rule is duplicated into the runtime pointer files on purpose, because an agent can act on it before reading the rulebook. The exception names itself and its bound. | Verified fact from the docs hazards | `integrate` |
| D13 | **A count that appears in more than one document is grepped and updated everywhere, in both languages, in the same change.** The document map names the specific counts that have already bitten. | Verified fact from the doc map | `already present` |
| D14 | **"No text change" is a valid, explicit outcome of a congruence review.** The gate table names when a review is required and permits concluding that the pages remain accurate. | Verified fact from the docs skill | `integrate` |
| D15 | **The document map holds only anchors that do not move, and says why each one is safe to hardcode.** Everything else resolves through the canonical index. A path written into a map is one more thing that goes stale, and a map pointing at an old name is worse than no map. | Verified fact from the doc map | `integrate` |

## Group E: Session Lifecycle

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| E1 | **Sensitive paths never auto-stage, whatever the user said.** An explicit pattern list forces a pause and a per-path question. | Verified fact from the stop workflow | `already present` |
| E2 | **Append-only files conflict by union, never by hand.** Dropping an entry orphans real records and breaks the import for the whole team. On any doubt, rebuild the index from what is on disk. | Verified fact from the stop workflow | `integrate` |
| E3 | **Only the repository-root state directory is valid.** Never a nested copy under packs, private folders, temp directories, or copied bundles. | Verified fact from both session workflows | `integrate` |
| E4 | **Author identity is derived, with a documented fallback chain and a warning when unusable.** Identity comes from the version-control configuration, falls back through three per-operating-system sources, and warns when the value looks like a placeholder. Never fabricate one: an unattributable thread is honest and recoverable, a made-up address is neither. | Verified fact from the start workflow | `inspire` |
| E5 | **Three fixed output shapes, and the vocabulary is enforced.** Blocked is for safety gates, failed is only for real command or tool errors, and the word "error" is reserved for the second. Never hide the real output. | Verified fact from both session skills | `already present` |
| E6 | **A success message is not evidence that anything was saved.** Their memory call accepts exactly two parameter names; a body passed under any other name deserializes to empty, skips validation, and still answers "saved". The rule is to pass it explicitly, read the stored body back, and confirm it is not empty before continuing. This failure happened, was written up, and then happened again, because the write-up lived only in memory. | Verified fact from the stop workflow | `integrate` |
| E7 | **The documentation sweep runs after the memory save and before the commit.** A failing check must never cost a session its memory. | Verified fact from the session-stop skill | `integrate` |
| E8 | **The closing sweep closes what it can and asks only about what it cannot.** Drift with one obvious fix is fixed and reported; a pause is reserved for a judgment the human owns. A stop that interrogates on every run is a stop people learn to rush through, and then it catches nothing. | Verified fact from the session-stop skill | `integrate` |
| E9 | **Every path the summary references is validated against the worktree.** Missing ones are surfaced and never block. It catches a summary claiming a file that never reached disk. | Verified fact from the stop workflow | `integrate` |
| E10 | **A block before the network says what stayed invisible.** Blocking happens before the fetch, so new skills and commands were never checked, and the banner says so rather than leaving the reader to assume the check passed. | Verified fact from the session-start skill | `integrate` |
| E11 | **Session start reads the rulebook and nothing else.** The gated rule files are never loaded at start; one is opened when its gate fires during the work. That is what keeps session-start cost flat as the rules grow. | Verified fact from the start workflow | `integrate` |
| E12 | **Search ranking is not date ranking.** Their memory search sorts by relevance, so the newest record is often absent from the top results; when a result looks implausibly old, cross-check with a second interface before concluding. | Verified fact from the start workflow | `integrate` |
| E13 | **Thread matching anchors on a literal line, never on "the body mentions it".** An incidental mention of a teammate in someone else's record leaks through a substring match. | Verified fact from the start workflow | `inspire` |

## Group F: Evidence And Claim Discipline

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| F1 | **Notices, pending actions, and questions go in a block that survives skimming.** A banner, box, bold line, or list, never inside a paragraph. Stated to hold for the skills and for chat alike. A question buried in prose is a question that does not exist. | Verified fact from the working agreements | `integrate` |
| F2 | **Every rule traces to a measured incident, and the incidents live in a separate document.** The rules file carries the imperative; the evidence file carries what each one cost, with dates and numbers, and the rules file says to read it before arguing with a rule. Each skill repeats the split: the skill states the imperative, a hazards file holds the failure behind it, to be read before weakening a rule. | Verified fact from the working agreements and hazards files | `integrate` |
| F3 | **A measurement is not a conclusion. Check the cause too.** | Verified fact from the working agreements | `integrate` |
| F4 | **Ship the scope with the number.** Name the grain and the filter, or a correct measurement of one thing becomes a false claim about everything. | Verified fact from the working agreements | `integrate` |
| F5 | **A tool can be wrong.** When one contradicts a belief, measure with a second before believing either. | Verified fact from the working agreements | `integrate` |
| F6 | **A document is verified against the machine, never against itself. N copies agreeing prove nothing.** The deployment rules restate it as a hard consequence: six copies of one command in a single operations document agreed perfectly and all six were missing a volume. Copies agreeing with each other is not evidence. | Verified fact from the working agreements and deploy skill | `integrate` |
| F7 | **A finding of the form "X and Y do not overlap" must name what would make it stop being true.** These are the dangerous findings: they carry no verb telling anyone to act, they grant permission, and a reader treats them as permanent. One was still granting permission after a data change had reversed it, through three review passes. | Verified fact from the docs hazards | `integrate` |
| F8 | **Put a rule where it is actually read.** A rule filed only behind a gate that never fires never runs. | Verified fact from the working agreements | `integrate` |
| F9 | **A resume guide for finished work is the worst stale document there is, because it is written to be obeyed.** Retire it the day the work lands. | Verified fact from the working agreements | `integrate` |
| F10 | **The status taxonomy separates "the check failed" from "the check could not run".** Four statuses: verified, partial, failed, and unverifiable, where collection, query, and authentication failures are unverifiable with a named blocker and never failed. A checked box means verified only. | Verified fact from the audit workflow | `integrate` |
| F11 | **A bundle may summarize counts but never carries one global status.** Each conclusion declares its own. | Verified fact from the audit workflow | `integrate` |
| F12 | **Publishing evidence passes a sanitization gate: a runnable secret and identity scan, then a manual review, and if neither can prove it safe the evidence is not published.** The scan names what it looks for: bearer tokens, key and secret assignments, connection strings, identifiers, home directory paths, and email addresses. | Verified fact from the audit workflow | `integrate` |
| F13 | **Every live conclusion records how to reproduce it.** Sanitized command, parameters, target fingerprint, execution time, tool version, status or exit code, output hash, source and retrieval date, and the source revision when local evidence is used. Cached or prior-session evidence is never current. | Verified fact from the audit workflow | `inspire` |
| F14 | **A rulebook budget states its measurement method, a target, a ceiling, and that passing the target is not a finding.** Measured as a word count times a constant, never by eye, because a character estimate reported a number 17 percent high on the day it mattered. Growing past the ceiling moves a section down; the ceiling is never raised. Their own record: the note explaining the budget is what broke the budget. | Verified fact from the rulebook maintenance rule | `already present` |
| F15 | **The "last updated" line is a date and a hint, never a second inventory.** It had grown into a control inventory already written in two canonical documents, which is the duplicated-status failure their own checker exists to catch. | Verified fact from the rulebook maintenance rule | `integrate` |
| F16 | **Name which of several environments produced a number.** Three environments that are never interchangeable, and every report says which one it came from. | Verified fact from the working agreements | `inspire` |
| F17 | **A resolver, not a name, decides project identity; anything under another name is a stale alias, ignored with one line saying so.** | Verified fact from the memory rule | `integrate` |
| F18 | **An explicit table says what goes where**: a decision and its reasoning to memory, what shipped to the changelog, document status to the index, how to work here to the rulebook. | Verified fact from the memory rule | `already present` |

## Group G: Authority And Artifact Identity

Historically informed completed `RH-W-0054`. Any future adoption of G1 through G3 belongs to a new unit that explicitly inherits that scope.

| # | Pattern | Evidence class | Recommendation |
|---|---|---|---|
| G1 | **The authority tuple travels as explicit command arguments: no state file, no inherited variable.** Stated as the reason, not the mechanism: the authority to change the machine has to be visible in the command that changes it. Five fields, each named, each with its role. | Verified fact from the deploy skill | `integrate` |
| G2 | **Identity comes from the bytes, not from a name.** A content hash of a normalized manifest is the identity; the source revision is an ancestry anchor and explicitly not an identity. A dirty tree may produce a candidate precisely because identity does not depend on the tree being clean. | Verified fact from the deploy skill | `integrate` |
| G3 | **A matching name never grants authority.** Inspect the artifact by its immutable identifier and require it to declare its own digest and provenance. A tag that matches is not authority. | Verified fact from the deploy skill | `integrate` |
| G4 | **List what an operation will publish before publishing it, and say it out loud.** An operation ships everything committed, not the change in the operator's head. Doing this afterwards only reports what already happened. | Verified fact from the deploy skill | `integrate` |
| G5 | **Stamp the artifact with the revision it was built from, and read that stamp to answer the previous rule next time.** Without it the only way to know what is running is to guess from dates, which has already produced a wrong answer. | Verified fact from the deploy skill | `inspire` |
| G6 | **Prove the change arrived: a string new in this change is absent before and present after, inside the running artifact.** A green build is not proof the new source arrived. Pick a probe that survives transformation, and when a probe comes back empty, rule out the transformation before concluding the code did not ship. | Verified fact from the deploy skill | `inspire` |
| G7 | **Shipping adds files, it never deletes them.** List what exists on the target and not in the source before building, and move the leftovers aside. A renamed file leaves a stale twin that either breaks the build or silently wins resolution over the file that replaced it. | Verified fact from the deploy skill | `inspire` |
| G8 | **A generated bundle is never overwritten.** A colliding name takes a suffix or returns blocked. | Verified fact from the audit workflow | `integrate` |
| G9 | **Sort before writing a manifest.** Measured: the same unchanged records produced a 544-line diff unsorted and a 153-line diff sorted, where only the sorted one shows what moved. A backup whose diff is unreadable is not doing its job. | Verified fact from the backup hazards | `integrate` |
| G10 | **A generated artifact carries a restore instruction, not just a copy.** Each backed-up file is classified as authored or generated, by what the generators actually write and never by filename, because restoring a generated file from a backup reintroduces the exact drift its generator exists to kill. | Verified fact from the backup skill and hazards | `integrate` |
| G11 | **Never normalize an artifact whose bytes are the contract.** Normalization rules apply to structured payloads only; reformatting a prompt changes what the model receives, which is the one thing a backup must never do. | Verified fact from the backup skill | `inspire` |
| G12 | **Report the two halves separately.** A merged total hides which source was actually reached, and the two fail independently. | Verified fact from the backup hazards | `integrate` |
| G13 | **A green run can still leave the repository wrong, and the defect is the claim, not the copy.** A clean export with zero failures was reported as bringing the repository back in sync while an older copy still declared itself the source of truth with stale content. What is never acceptable is a file whose header tells the reader it is authoritative while a fresher export disagrees. | Verified fact from the backup hazards | `integrate` |

## What This Review Explicitly Did Not Find

Stated so absence is not later mistaken for evidence. Each claim is bounded by the surface named in **What was read**.

- **No packaging or distribution model to borrow.** The external harness is installed by copying a directory by hand: no renderer, no manifest of generated outputs, no digest-bound plan, no upgrade path, and one skill's reference states outright that no canonical source is designated among its three pack copies. This repository is ahead on that axis and nothing here should be traded for it.
- **No evidence-class vocabulary spanning the whole harness.** Its audit skill has a sharp four-status taxonomy, but it is scoped to that skill; there is no equivalent of a repository-wide verified fact, supported inference, unknown, and human-decision classification.
- **No approval or consent model for repository operations.** Work happens directly on the main branch with commits at session close. That is a different risk posture and is not portable here.
- **No bilingual document contract.** Documents mix languages per file rather than maintaining localized peers, so there is nothing to learn for the localization contract this repository owns.
- **No mechanism for a consumer to add a control that the harness will run.** The discovery runner is a repository control, and the pack that ships elsewhere explicitly cannot bring it.

## Reconciliation Summary

Ninety-four findings.

| Recommendation | Count |
|---|---|
| `integrate` | 64 |
| `inspire` | 14 |
| `already present` | 16 |
| `reject` | 0 |
| `out of scope` | 0 |

`integrate`: A1, A2, A3, A6, A8, A10, A11, A12, B1, B2, B7, B8, B9, C1, C2, C3, C5, C7, C8, C10, C11, C12, D1, D4, D6, D8, D9, D10, D11, D12, D14, D15, E2, E3, E6, E7, E8, E9, E10, E11, E12, F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12, F15, F17, G1, G2, G3, G4, G8, G9, G10, G12, G13.

`inspire`: A4, A5, B6, B11, C9, D7, E4, E13, F13, F16, G5, G6, G7, G11.

`already present`: A7, A9, B3, B4, B5, B10, C4, C6, D2, D3, D5, D13, E1, E5, F14, F18.

Counts and membership are derived from the tables above by parsing them, not by hand. An earlier draft of this document claimed a total the tables did not support; the count is now checked against the rows.

**Human decision required.** This table is a recommendation, not an adoption. Proposed groupings, each its own work unit:

1. **Transport hardening**: C1, C2, C3, C5, C7, C8, C10, C11, C12. Highest urgency, because the generated session gate here promises a remediation banner it cannot reach when a passphrase-protected key is present.
2. **Command projection**: B1, B2, B7, B8, B9. Folds into the already-open `RH-W-0057` and corroborates it.
3. **Control discipline**: A1, A2, A3, A6, A8, A10, A11, A12.
4. **Documentation invariants**: D1, D4, D6, D8, D9, D10, D11, D12, D14, D15.
5. **Evidence and claim discipline**: F1 through F12, F15, F17.
6. **Authority and artifact identity**: G1, G2, G3, G4, G8, G9, G10, G12, G13. G1 through G3 historically informed completed `RH-W-0054`; direct any future adoption to a new unit that explicitly inherits that scope.
7. **Session lifecycle**: E2, E3, E6, E7, E8, E9, E10, E11, E12.

No adoption, implementation, or generated behavior change is authorized by this document.
