# RH-W-0059 Design: Non-Interactive Transport Contract

<!-- doc-role:design -->

Completed owner-approved design for [RH-W-0059](roadmap.md#rh-w-0059), delivered test-first. The body retains the pre-delivery draft and review narrative as history. The Spanish peer is [`../es/repo-harness/rh-w-0059-design.md`](../es/repo-harness/rh-w-0059-design.md).

## The Defect

The generated session contracts promise a remediation banner they cannot always reach.

**Verified fact.** `.repo-harness/skills/repo-harness-session-start/SKILL.md` requires one approval before network access, names `git fetch` and fast-forward-only integration, and closes its synchronization gate with an obligation to name an available alternate credential path after a transport failure. `repo-harness-session-stop/SKILL.md` step 12 carries the same obligation for the push path. Neither specifies how the network command is invoked. The strings `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY`, and `passphrase` appear in no generated skill and in no roadmap record.

**Supported inference.** Against a passphrase-protected SSH key or an uncached HTTPS credential, `git fetch` opens a prompt. An agent has no TTY, so nothing answers it. The session hangs with no error, no exit status, no banner, and no timeout, which means the transport-failure row of the gate never executes and the alternate-credential-path obligation never fires. The evidence is the absence of any non-interactive requirement in the contract plus the reviewed external harness stating the identical failure and stating that exactly this configuration exists on one of its two machines. Reproducing it against a passphrase-protected key would raise this to verified fact.

**Why it is worse on the close path.** `repo-harness-session-stop` reaches the network after the memory save and after the commit. A hang there leaves the user with a frozen session, a commit that may already exist, and no output saying so.

## Scope: What The Nine Findings Actually Leave To Do

The unit was registered owning nine findings. Checking each against the current contract rather than assuming, five of them need nothing here:

| Finding | State in this repository | Disposition |
|---|---|---|
| `C3` non-interactive invocation | Absent. No generated skill names any prompt-suppression mechanism. | **In scope.** This is the whole defect. |
| `C7` documented limits of the mechanism | Not applicable yet: there is no mechanism to bound. | **In scope**, conditional on `C3` landing. |
| `C5` quoted upstream shorthand | The contract never writes `@{u}`, so nothing to fix in existing text. But it instructs an agent to compare ahead and behind and integrate fast-forward-only, and an agent reaches for the shorthand to do that. | **In scope** as a hazard attached to the integration instruction, not as a correction to existing text. |
| `C4` never an environment assignment as a command prefix | Already handled elsewhere by `RH-W-0035`, which split the POSIX-only inline assignment per shell in `references/validation.md`. It is absent from the transport paragraph only because that paragraph names no environment variable yet. | **In scope**, conditional on `C3` introducing one. |
| `C1` ordered provider-first fallback | The contract keeps native Git as the core transport and provider tooling as an optional alternate credential path. An ordered provider-first fallback presumes a provider CLI this product must not require. Its loud terminal failure is already present. | **Excluded**, deliberately. |
| `C2` explicit HTTPS URL for the provider path | Same reason: a provider-path mechanic in a product that does not own a provider path. | **Excluded**, deliberately. |
| `C8` local merge rather than a re-fetching pull | Already satisfied: `git pull` is already forbidden and integration is already fast-forward-only. Only the reason is unstated, and the external harness's reason does not transfer, because it rests on a provider-first path this product does not have. | **Out of scope.** Recorded so the gap is not rediscovered as new. |
| `C11` line-ending configuration can fake a content change | Real and locally reachable: `core.autocrlf` is `true` in this clone and `.gitattributes` pins `eol=lf`. But it is a staging-report concern, not transport. | **Moved** to `RH-W-0060`. |
| `C12` a state review includes branches | Real, but not transport. | **Moved** to `RH-W-0060`. |

Finding `C10`, the bare local date, was in an earlier draft of this design and is withdrawn. **Verified non-defect**: no generated skill, no rulebook rule, and no line of the renderer takes a date through a shell command, so the hazard the external harness measured has no reachable path here. Adopting it would have added a new obligation disguised as a correction, and it would have pushed `REPO-HARNESS.md` past its own ceiling, which is at 696 tokens of 700 today.

**Four clauses remain, and three of them exist only because the first one does.**

## The Four Clauses

### 1. The invocation is non-interactive, and the contract names the mechanism

Every authorized network command runs with the interactive prompt disabled on both paths that can open one:

- SSH: `-o BatchMode=yes`, passed through `git -c core.sshCommand=` so it parses identically in every shell.
- Git's own credential prompt: `GIT_TERMINAL_PROMPT=0`, set in the environment, because it has no `-c` equivalent.
- An unreachable host: `-o ConnectTimeout=20`, so a dead network fails in seconds rather than at the operating system's default.

The two mechanisms are not redundant: batch mode covers SSH, the terminal-prompt kill switch covers HTTPS, and a repository can use either.

Failing in seconds routes into the existing `authentication or transport failure` row, which already preserves the command receipt and already requires naming an alternate credential path. **This clause adds no outcome. It makes the existing one reachable.**

### 2. The environment variable is never written as a command prefix

`GIT_TERMINAL_PROMPT=0 git fetch` is POSIX-only. PowerShell parses the assignment as a command name and fails before Git runs, on the exact platform this repository targets first. The contract states the per-shell form: `export` in Bash and Zsh, `$env:` in PowerShell.

This clause exists only because clause 1 introduces the variable. `RH-W-0035` already established the rule for the one other place this repository sets an environment variable inline.

### 3. The upstream shorthand is quoted

Any command using Git's upstream shorthand writes it as `"@{u}"`. Unquoted, PowerShell can parse it as a hashtable expression. Quoted, it is correct in PowerShell, Bash, and Zsh.

The contract does not currently write the shorthand, so this is not a correction to existing text. It is a hazard note attached to the instruction that makes an agent reach for it: compare ahead and behind, then integrate fast-forward-only.

### 4. The two known limits are documented where the mechanism is given

`-c core.sshCommand` is ignored when `GIT_SSH_COMMAND` is already set in the environment, and it overrides a configured `core.sshCommand`, so a setup depending on a specific SSH binary or identity is silently downgraded rather than honored. `GIT_TERMINAL_PROMPT=0` silences Git's own prompt but not a credential helper's graphical prompt, such as Git Credential Manager on Windows.

Stating both next to the mechanism prevents a false claim that the hang is now impossible. Clause 1 makes the common case fail fast; it does not make every case fail fast.

## What Does Not Change

- **The approval threshold.** `RH-W-0046` owns whether a read-only fetch is separable from the network-access approval. This unit governs only how an already-authorized command is invoked. If `RH-W-0046` later removes an approval, these clauses apply unchanged to whatever remains.
- **Provider tooling stays optional.** Native Git remains the core transport.
- **The outcome model.** `ready`, `paused`, `blocked`, `failed`, `degraded`, `completed` are untouched. A transport failure remains `failed` on the optional Git branch and `degraded` for startup.
- **The `git pull` prohibition.** It stays exactly as written. This design deliberately does not attach the external harness's reason to it, because that reason rests on a provider-first path this product does not have.

## The Open Decision, And The Recommendation

**Question**: does the contract name exact flags, or state the obligation and leave the flags to the implementation?

**Recommendation: name the flags.** Three reasons, in order of weight.

1. **The reader is an agent, not a compiler.** A generated contract saying "invoke it non-interactively" leaves the agent to derive the incantation. The finding exists precisely because that incantation is non-obvious: two separate suppression mechanisms covering two different transports, one of which has no `-c` form, plus a timeout, and a wrong guess reproduces the hang the clause exists to prevent.
2. **The contract is already coupled to Git's CLI surface.** It names `git fetch`, forbids `git pull`, `git rebase`, and `git reset`, and requires fast-forward-only integration. Naming three flags adds no coupling that is not already there.
3. **An unnamed obligation cannot be tested.** A focused test can assert that the generated text contains the mechanism. It cannot assert that a paraphrase would produce a non-interactive command.

**The cost, stated honestly.** If Git changes these flags, the generated contract goes stale and the checker cannot detect it, because no control here executes Git against a passphrase-protected key. That is real, and the mitigation is clause 4: the limits are documented next to the mechanism, so a reader who hits a case the flags do not cover has the reason rather than a bare incantation.

## Implementation Sketch

One rendering site per skill in `skill/repo-harness-builder/scripts/harness_tool.py`:

| Clause | Site |
|---|---|
| 1, 2, 3, 4 | The synchronization gate closing paragraph in the session-start renderer, and session-stop execution step 12 |

Both generated session skills carry all four clauses, because both reach the network and the stop path is the more damaging one. No rulebook change, no new decision gate, no new manifest field, and no change to `REPO-HARNESS.md`, which has 4 tokens of headroom.

## Test Plan

Strict TDD. Every test fails before implementation.

1. `NonInteractiveTransportTests`: the rendered session-start and session-stop skills each contain the two suppression mechanisms, the bounded connect timeout, the per-shell environment form, the quoted upstream shorthand, and both documented limits.
2. A guard asserting the POSIX-only prefix form appears in neither rendered skill, so a future edit cannot reintroduce it.
3. A guard asserting the rendered rulebook is unchanged and still inside its token ceiling.
4. The existing suite passes unchanged, and the maintenance renderer synchronizes the canonical self-hosted skills and both runtime projections.

Expected before delivery: `RH-W-0059` stays `proposed` until the owner approves this design, then `ready`, then `active` on authorized implementation.

## Review Of This Design

This design was reviewed against repository evidence after its first draft, and the review cut it from eight clauses to four.

- Three clauses were removed as scope creep: dates, branch listing, and line-ending reporting are not transport, and they were travelling on a `P1` unit's urgency.
- One clause was withdrawn as a non-defect: the date hazard has no reachable path in this repository, and adopting it would have broken the rulebook's own token ceiling, which the rulebook forbids raising.
- One reason was retracted as imported: the external harness's justification for forbidding `git pull` rests on a provider-first path this product does not have, and attaching it here would have credited this contract with a motive from an architecture it does not use.
- Two findings were reclassified as already satisfied rather than pending, so the gap is not rediscovered later as new work.

## Human Decisions Remaining

1. Approve or reject this design.
2. Confirm the flags recommendation, or choose the obligation-only form.
3. Confirm that `C1` and `C2` stay excluded, since both presume a provider CLI this product must not require.
