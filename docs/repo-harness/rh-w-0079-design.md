# RH-W-0079 Optional Capability and Release Boundary Design

Status: `ready`, design complete and owner-approved on 2026-08-28. The separate bilingual implementation plan is also owner-approved. Neither approval grants implementation, release, or delivery authority.

## Purpose

Define one generic repo-harness contract that remains useful when Git or an ecosystem integration is absent, while preserving stronger capability-backed behavior when it is available. Reciprocal self-hosting is the proving ground, not permission to export this repository's policy to consumers.

## Evidence and Confidence

### Verified repository evidence

- `skill/repo-harness-builder/scripts/harness_tool.py` currently defaults builder configuration to `integrated` when `--standalone` is absent, and `select_integration_mode()` permits that route only when both Gentle AI and Engram classify as `verified`.
- Completed RH-W-0025 owns the existing `not_found`, `found_on_path`, and `verified` detection results for Gentle AI and Engram. This design changes routing, not those detector semantics.
- Completed RH-W-0051 owns the fixed non-authoritative local namespaces. This design creates no capability store or new namespace.
- The roadmap gives RH-W-0048 runtime projection recovery and gives RH-W-0052 archive, uninstall, reinstall, and rollback design. RH-W-0079 coordinates with those owners without absorbing them.
- `REPO-HARNESS.md` identifies this repository as self-hosted and distinguishes the builder from the consumer harness.

### Primary external evidence

- Git defines a repository through repository metadata, including `HEAD`, refs, objects, configuration, and usually an index. `HEAD` may name a branch that does not yet exist or may point directly to a commit in detached state. This supports treating initialized, unborn, and detached states separately. [Git repository layout](https://git-scm.com/docs/gitrepository-layout)
- `git init` creates repository metadata and an initial branch with no commits. It is therefore a mutation that changes the selected folder, not a harmless detection step. [Git init](https://git-scm.com/docs/git-init)
- `git status` distinguishes index, working-tree, and untracked changes, and its porcelain format is intended for scripts. This supports preserving dirty state as evidence rather than reducing it to a generic failure. [Git status](https://git-scm.com/docs/git-status)
- GitHub documents issues as its own work-tracking feature and pull requests as proposals to merge changes with GitHub review context. They are provider capabilities, not properties of Git itself or universal repository requirements. [GitHub Issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues), [GitHub pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

Confidence is high for the current repository behavior and Git state model because they come from canonical source, tests, local Git evidence, and official Git documentation. Confidence is high that GitHub issues and pull requests are provider-owned capabilities based on GitHub's own contracts. Exact implementation surfaces remain deliberately unknown until an approved implementation plan measures them.

## Capability Ownership Model

The repository remains the authority for durable facts, local policy, source, tests, and approvals. Each optional capability owns only the behavior it can actually provide:

| Capability | Owned behavior | Behavior it does not own |
|---|---|---|
| Filesystem and repository documents | Supported baseline inspection, documentation, local edits, and visible session handoff | Version history, remote state, provider workflow, or external continuity |
| Git | Commit history, tracked comparison, branches, commits, Git rollback evidence, remotes, and upstream comparison | GitHub issues, pull requests, provider checks, or semantic memory |
| Provider or GitHub tooling | Provider authentication and provider-specific issues, pull requests, checks, and metadata | Local Git correctness or repository policy |
| `CONTRIBUTING.md` or equivalent | Repository-specific contribution policy when present and applicable | Generic product policy when absent |
| Gentle AI | Its verified routing, review, receipt, and delivery coordination | Repository truth or Engram continuity |
| Engram | Its verified semantic continuity and memory operations | Repository truth, Git history, or delivery authority |
| CodeGraph | Its verified indexed structural exploration and impact evidence | Compiler, test, Git, or repository authority |
| Compatible agent runtime | Reading and applying the emitted file contracts | Capabilities the runtime does not expose |

Capabilities compose independently. Git plus Engram without Gentle AI retains Git and Engram behavior. No named pair becomes a hidden mode prerequisite.

## Detection, Use, and Mutation Boundaries

1. **Detect** with the narrowest read-only owner surface available. Preserve the typed outcome and exact limitation. Detection does not authorize use or mutation.
2. **Use** a capability only when its detected state supports the requested operation. Read-only use stays bounded to current intent and repository policy.
3. **Mutate** only when the requested outcome authorizes that mutation and every applicable approval gate is satisfied. A capability being installed never grants mutation authority.
4. Re-detect before an approved apply when changed capability evidence could invalidate approved bytes, scope, or authority.
5. Stable absence between preview and apply is not a failure. A material mismatch is a scoped refusal only for the affected action.
6. Never infer provider, remote, policy, history, branch, continuity, or graph guarantees from an unrelated capability.

## Capability State Matrix

| State | Meaning | Generic behavior | Mutation and approval |
|---|---|---|---|
| `available` | Detection verifies the capability needed for the requested action | Use only its owned behavior and report capability-backed guarantees | Existing action-specific approval rules still apply |
| `unavailable` | Executable, service, owner file, or required evidence is absent | Continue all unaffected behavior and omit only owned guarantees | Do not install, initialize, authenticate, or configure automatically |
| `partial` | Some owned behavior is verified while another required part is absent or unusable | Use the verified subset, name the unavailable subset, and avoid whole-system failure | Ask only if the requested action needs the missing subset |
| `disabled` | The repository or human explicitly opted out of an otherwise available capability | Respect the opt-out and use the same bounded degradation as absence | Re-enabling is a separate explicit decision |
| `policy-required` | Current repository authority requires a capability for one specific workflow | Block only that workflow when the requirement cannot be met | Never promote local policy into the generic product |

`policy-required` is contextual, not a sixth technical availability result and not a vendor default. A missing optional integration degrades only the capability it owns.

## Git States and Recommendation

| Git state | Supported behavior | Guarantees withheld |
|---|---|---|
| Executable absent | Continue supported non-Git work and make at most one useful, operation-scoped recommendation that names the concrete history, comparison, recovery, rollback, or delivery safety Git would add | Every Git-backed claim |
| Executable present, selected folder uninitialized | Continue non-Git behavior. Mention initialization only when relevant | Repository history, branches, commits, tracked comparison, and Git rollback |
| Initialized, unborn branch | Use repository metadata and working-tree evidence | Prior commit history and rollback to an earlier commit |
| Initialized, detached HEAD | Use commit and working-tree evidence | Current-branch ownership and ordinary branch delivery claims |
| Initialized, dirty | Preserve existing changes, identify overlap, and constrain writes to authorized surfaces | Any claim that current changes belong only to this task |
| Initialized, clean | Use applicable local Git evidence | Remote and provider claims unless separately verified |
| No upstream or remote | Retain local Git safety | Ahead or behind, synchronization, publication, and remote recovery |
| Diverged or unresolved operation | Preserve state and stop only the affected integration or delivery action | Safe automatic integration or delivery |

The recommendation creates no persistent state, warning ledger, counter, or nag mode. It is emitted once per applicable interaction and omitted when it adds no useful action. `git init` always requires explicit human approval because it mutates the selected folder. The harness never runs it as setup, recovery, or an apply side effect.

## Repository Policy and Provider Behavior

- GitHub, issues, pull requests, provider checks, and provider tooling are optional even when a Git remote exists.
- A present `CONTRIBUTING.md` or equivalent is repository-specific authority only within its applicable scope. Its absence never creates issue-first, branch, provider, or review requirements.
- Provider absence does not weaken local repository evidence or local Git safety.
- Provider features are offered only after provider recognition and capability verification. The generic contract does not assume GitHub from a URL, a `.git` directory, or the word `origin`.
- Self-hosted issue-first and bilingual policy remains repository-specific unless separate consumer evidence supports a generic rule.

## Selection and Apply Contract

Current default integrated selection couples the whole non-standalone route to both Gentle AI and Engram. Migration is bounded:

1. Preserve RH-W-0025 detectors and their typed evidence.
2. Replace the binary mandatory pair with a capability set derived from verified, disabled, partial, and unavailable outcomes.
3. Render useful standalone repository behavior by default when no optional integration is verified.
4. Add each verified optional capability's owned contract without making any other capability mandatory.
5. Bind the exact effective capability configuration into existing preview and apply authority using existing mechanisms where sufficient.
6. Before apply, reject only material capability drift that invalidates the approved plan, target, scope, or authority.
7. Do not add a manifest field, migration file, compatibility store, or second authority path unless implementation evidence proves the existing binding insufficient.

Existing explicit standalone behavior remains supported. Existing integrated installations migrate through ordinary generated-output replacement after separate implementation approval. Completed RH-W-0025 detection and completed RH-W-0051 storage remain closed.

## Coordination With RH-W-0048 and RH-W-0052

- RH-W-0048 retains runtime projection recovery, including its tracked-versus-local policy and additive, non-overwriting recovery proof.
- RH-W-0052 retains archive, uninstall, reinstall, rollback, and non-Git folder consequences after RH-W-0048 proves ownership and recovery primitives.
- RH-W-0079 contributes only the shared degradation rule: retain useful behavior for capabilities still available, visibly omit unavailable guarantees, and never fabricate restoration.
- Each record keeps separate lifecycle, RED tests, implementation, rollback, and delivery evidence. This design neither reopens nor advances either record.

## Approval Boundaries

- No approval is needed to report read-only capability detection already authorized by the requested work.
- Repository writes require the normal applicable authority for the requested change.
- Network access, provider mutation, publication, destructive action, external-path access, authentication changes, Git commits, Git pushes, and Git initialization retain their own explicit gates.
- One approval may cover an unchanged bounded sequence only where the owning contract already permits it. A material change invalidates that approval.
- Missing capability evidence never authorizes the harness to install, configure, initialize, authenticate, or substitute another provider.

## Focused RED Tests

Implementation must first prove failure for:

1. default preview with neither Gentle AI nor Engram;
2. each integration available alone and both available together;
3. `available`, `unavailable`, `partial`, `disabled`, and repository `policy-required` outcomes without global failure;
4. Git executable absent with useful output and one bounded recommendation;
5. uninitialized folder with no `git init` before explicit approval;
6. clean, dirty, detached, unborn, no-upstream, and diverged Git states with only supported claims;
7. provider tooling absent and Git present;
8. GitHub unavailable and a non-GitHub provider available;
9. missing `CONTRIBUTING.md` without invented issue-first behavior;
10. repository policy requiring one absent capability, blocking only its workflow;
11. stable capability absence across preview and apply;
12. material capability drift invalidating only affected approved authority;
13. standalone migration and existing integrated-install migration without a duplicate store;
14. RH-W-0048 and RH-W-0052 boundaries remaining independent;
15. release inventory rejecting an unclassified changed behavior.

Tests use fakes or temporary repositories and no live provider, network, daemon, or destructive mutation. They must preserve pre-existing uncommitted work.

## Cross-platform and Agent Acceptance

- Focused tests pass on Windows, Linux, and macOS where CI evidence exists. Missing CI evidence is reported, never inferred.
- Commands and paths use the project's existing cross-platform Python and Git boundaries, with no POSIX-only shell assumption.
- OpenCode, Claude-compatible, Pi-compatible, and generic file-following agents receive equivalent semantics on surfaces they support.
- An agent lacking an integration follows repository-only behavior rather than failing the harness.
- Current behavior remains the baseline until implementation and release verification prove the replacement.

## Release and Rollback Acceptance

Before release:

1. classify every changed behavior exactly once as builder-only scaffolding, repository-specific policy, or generic consumer contract;
2. prove focused RED then GREEN evidence and the applicable canonical suite;
3. verify checker, generated projections, localized peers, migration, standalone behavior, and capability combinations;
4. replay a non-Git folder, a local Git repository without a provider, and this self-hosted repository;
5. verify no automatic Git initialization, provider assumption, duplicate state, or unsupported guarantee;
6. record exact rollback evidence.

Rollback restores selection, apply routing, tests, and generated projections as one consistent versioned unit. It does not rewrite consumer repositories, initialize or remove Git, delete local state, or pretend capabilities remain available. A rollback candidate that restores mandatory paired preflight does not satisfy this approved direction and must not be released without a new explicit owner decision.

## Rejected Overengineering and Non-goals

- No adapter framework, plugin system, provider abstraction layer, capability service, daemon, scheduler, or telemetry system.
- No duplicate capability store, continuity store, approval store, warning ledger, recommendation counter, or migration registry.
- No automatic `git init`, provider installation, authentication, issue creation, pull request creation, commit, push, or publication.
- No assumption that Git implies GitHub, issues, pull requests, `CONTRIBUTING.md`, or provider tooling.
- No broad rewrite of session contracts, manifest schema, checker, local-state taxonomy, recovery, or reinstall behavior.
- No reopening RH-W-0025 or RH-W-0051, and no merging or advancing RH-W-0048 or RH-W-0052.
- No implementation, tests, generated projection changes, release, or delivery under this design authority.

## Owner Gate

Owner decision recorded: the owner answered `daale` to the exact approval question for this complete bilingual design on 2026-08-28. The separate bilingual implementation plan now identifies exact code surfaces, migration bytes, RED tests, rollback commands, and delivery evidence, and was later owner-approved after a fresh final PASS review. Implementation remains separately unauthorized.
