# RH-W-0059 Design: Non-Interactive Transport Contract

<!-- doc-role:design -->

Draft design for [RH-W-0059](roadmap.md#rh-w-0059). Pending owner approval; no implementation is authorized by this document. The Spanish peer is [`../es/repo-harness/rh-w-0059-design.md`](../es/repo-harness/rh-w-0059-design.md).

## The Defect

The generated session contracts promise a remediation banner they cannot always reach.

**Verified fact.** `.repo-harness/skills/repo-harness-session-start/SKILL.md` requires one approval before network access, names `git fetch` and fast-forward-only integration, and closes its synchronization gate with an obligation to name an available alternate credential path after a transport failure. `repo-harness-session-stop/SKILL.md` step 12 carries the same obligation for the push path. Neither specifies how the network command is invoked. The strings `BatchMode`, `GIT_TERMINAL_PROMPT`, `TTY`, and `passphrase` appear in no generated skill and in no roadmap record.

**Supported inference.** Against a passphrase-protected SSH key or an uncached HTTPS credential, `git fetch` opens a prompt. An agent has no TTY, so nothing answers it. The session hangs with no error, no exit status, no banner, and no timeout, which means the transport-failure row of the gate never executes and the alternate-credential-path obligation never fires. The evidence is the absence of any non-interactive requirement in the contract plus the reviewed external harness stating the identical failure and stating that exactly this configuration exists on one of its two machines. Reproducing it against a passphrase-protected key would raise this to verified fact.

**Why it is worse on the close path.** `repo-harness-session-stop` reaches the network after the memory save and after the commit. A hang there leaves the user with a frozen session, a commit that may already exist, and no output saying so.

## What Changes

Eight obligations across the two generated session skills, six in the transport gate and two adjacent. Each is one clause; none changes an outcome, a threshold, or an approval.

### 1. The invocation is non-interactive, and the contract names the mechanism

Every authorized network command runs with the interactive prompt disabled on both paths that can open one:

- SSH: `-o BatchMode=yes`, passed through `git -c core.sshCommand=` so it parses identically in every shell.
- Git's own credential prompt: `GIT_TERMINAL_PROMPT=0`, set in the environment, because it has no `-c` equivalent.
- An unreachable host: `-o ConnectTimeout=20`, so a dead network fails in seconds rather than at the operating system's default.

Failing in seconds routes into the existing `authentication or transport failure` row, which already preserves the command receipt and already requires naming an alternate credential path. This obligation does not add a new outcome; it makes the existing one reachable.

### 2. The environment variable is never written as a command prefix

`GIT_TERMINAL_PROMPT=0 git fetch` is POSIX-only. PowerShell parses the assignment as a command name and fails before Git runs, on the exact platform this repository targets first. The contract states the per-shell form: `export` in Bash and Zsh, `$env:` in PowerShell.

### 3. The upstream shorthand is quoted

Any command using Git's upstream shorthand writes it as `"@{u}"`. Unquoted, PowerShell can parse it as a hashtable expression. Quoted, it is correct in PowerShell, Bash, and Zsh. The current contracts use fast-forward-only integration without stating this hazard.

### 4. Integration is a local merge of the already-fetched upstream

The contract already forbids `git pull`. It does not say why, and the reason is operational rather than stylistic: a pull re-fetches over the default remote configuration, which can resolve to a different transport than the one that just succeeded. The clause gains its reason so a future edit does not relax it as arbitrary.

### 5. The two known limits are documented where the mechanism is given

`-c core.sshCommand` is ignored when `GIT_SSH_COMMAND` is already set in the environment, and it overrides a configured `core.sshCommand`, so a setup depending on a specific SSH binary or identity is silently downgraded rather than honored. `GIT_TERMINAL_PROMPT=0` silences Git's own prompt but not a credential helper's graphical prompt. Stating both next to the mechanism prevents a false claim that the hang is now impossible.

### 6. Dates are taken with a bare local `date`

**Verified fact from the external review.** A `TZ=` command prefix is ignored without error by Git Bash on Windows and returns UTC. After 18:00 local, UTC is already tomorrow, so every date written that evening is one day forward and no reader can detect it. This repository writes dates into `CHANGELOG.md`, roadmap decision histories, and current state. The clause requires a bare local `date` and an explicit conversion when a UTC source must be converted.

### 7. A repository state review includes the branches

**Verified fact from the external review.** An audit covering documentation, code, controls, and delivery never listed branches, and a merged branch dead for fifteen days kept appearing and raising doubt about whether it held work. What settles deletion is an ancestry check against both the local and the remote tip, never the branch name or its date. The generated session-start step 3 currently inspects branch, worktree, index, upstream, remotes, and remote URLs. It gains the branch listing.

### 8. Line-ending configuration can fake a content change

**Verified fact from the external review.** With `core.autocrlf=true` fighting a `.gitattributes eol=lf` rule, a tool that normalizes line endings reports one spurious modification per affected file. This repository already pins LF through `.gitattributes` for every byte-compared generated path (`RH-W-0029`), and `RH-W-0042` already fixed the conflict-detection half. The remaining half is reporting: before recording a content change, compare against the committed blob. This clause belongs in the session-stop staging classification, not in the transport gate.

## What Does Not Change

- **The approval threshold.** `RH-W-0046` owns whether a read-only fetch is separable from the network-access approval. This unit governs only how an already-authorized command is invoked. If `RH-W-0046` later removes an approval, these clauses apply unchanged to whatever remains.
- **Provider tooling stays optional.** The contract keeps native Git as the core transport and provider tooling as an alternate credential path that must be installed and separately approved. This design does not add an ordered provider-first fallback, because finding `C1`'s ordered fallback presumes a provider CLI this product must not require. What it takes from `C1` is only the loud terminal failure, which the contract already has.
- **The outcome model.** `ready`, `paused`, `blocked`, `failed`, `degraded`, `completed` are untouched. A transport failure remains `failed` on the optional Git branch and `degraded` for startup.
- **Finding `C2`** (pass an explicit HTTPS URL rather than overriding the remote URL by configuration) is out of scope for the same reason as the ordered fallback: it is a provider-path mechanic, and this product does not own a provider path. It is recorded here as deliberately excluded rather than missed.

## The Open Decision, And The Recommendation

**Question**: does the contract name exact flags, or state the obligation and leave the flags to the implementation?

**Recommendation: name the flags.** Three reasons, in order of weight.

1. **The reader is an agent, not a compiler.** A generated contract saying "invoke it non-interactively" leaves the agent to derive the incantation. The finding exists precisely because that incantation is non-obvious: three separate mechanisms, one of which has no `-c` form, and a wrong guess reproduces the hang the clause exists to prevent.
2. **The contract is already coupled to Git's CLI surface.** It names `git fetch`, forbids `git pull`, `git rebase`, `git reset`, and requires fast-forward-only integration. Naming three flags adds no coupling that is not already there.
3. **An unnamed obligation cannot be tested.** A focused test can assert that the generated text contains the mechanism. It cannot assert that a paraphrase would produce a non-interactive command.

**The cost, stated honestly.** If Git changes these flags, the generated contract goes stale and the checker cannot detect it, because no control here executes Git against a passphrase-protected key. That is real, and the mitigation is clause 5: the limits are documented next to the mechanism, so a reader who hits a case the flags do not cover has the reason rather than a bare incantation.

## Implementation Sketch

Rendering sites in `skill/repo-harness-builder/scripts/harness_tool.py`:

| Clause | Site |
|---|---|
| 1, 2, 3, 5 | The synchronization gate closing paragraph, and session-stop step 12 |
| 4 | The same paragraph, as the reason attached to the existing `git pull` prohibition |
| 6 | A hard rule in the generated rulebook, since dates are written by every skill |
| 7 | Session-start execution step 3 |
| 8 | Session-stop staging classification, step 8 |

Both generated session skills carry the transport clauses, because both reach the network and the stop path is the more damaging one.

## Test Plan

Strict TDD. Every test fails before implementation.

1. `NonInteractiveTransportTests`: the rendered session-start and session-stop skills each contain the three mechanisms, the per-shell environment form, the quoted upstream shorthand, and both documented limits.
2. A guard asserting the POSIX-only prefix form appears in neither rendered skill, so a future edit cannot reintroduce it.
3. `RepositoryStateBranchTests`: the rendered session-start step 3 requires a branch listing.
4. `LocalDateTests`: the rendered rulebook forbids the timezone command prefix and requires a bare local date.
5. Line-ending reporting: the rendered session-stop staging classification requires comparison against the committed blob.
6. The existing suite passes unchanged, and the maintenance renderer synchronizes the canonical self-hosted skills and both runtime projections.

Expected before delivery: `RH-W-0059` stays `proposed` until the owner approves this design, then `ready`, then `active` on authorized implementation.

## Human Decisions Remaining

1. Approve or reject this design.
2. Confirm the flags recommendation, or choose the obligation-only form.
3. Confirm that `C1`'s ordered provider fallback and `C2`'s explicit HTTPS URL stay excluded, since both presume a provider CLI this product must not require.
