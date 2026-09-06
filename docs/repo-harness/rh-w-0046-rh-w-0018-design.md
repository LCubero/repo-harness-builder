# RH-W-0046 / RH-W-0018 Repository Consent Design

**Status:** owner-delegated approved design, not implemented. RH-W-0046 remains `proposed` and RH-W-0018 remains `deferred`. This is one design for two independently tracked work items; it does not merge their lifecycles, implementations, tests, or delivery. Implementation planning and implementation remain unauthorized.

Review this design against [RH-W-0046](roadmap.md#rh-w-0046), [RH-W-0018](roadmap.md#rh-w-0018), the [local-state taxonomy](rh-w-0051-design.md), [standing decisions](decisions.md), and the [Spanish peer](../es/repo-harness/rh-w-0046-rh-w-0018-design.md).

## Decision

Keep `git fetch` inside the network-approval threshold. Permit a human to remember consent for one repository, one identified remote, one exact fetch destination, and the `git-fetch` operation only. The grant is explicit, visible whenever used, and revocable by a simple human instruction. Without a valid grant, session-start uses the existing interactive approval path.

RH-W-0018 may reuse the same **preference pattern**, but not this fetch grant. General low-question behavior remains a separate `interaction-behavior` preference and never authorizes Git. It must not be stored in this file or inferred from it.

## Minimal Owner And Format

The ignored, non-authoritative file `.repo-harness/state/preferences/fetch-consent.md` owns only remembered fetch consent. RH-W-0051 owns its parent namespace and Git-ignore behavior. This file does not duplicate lifecycle or approval authority and is distinct from the deferred `.repo-harness/state/preferences/user-preferences.md` presentation agreement.

The file is structured Markdown with one current grant and these required fields:

```markdown
# Repository fetch consent
schema: 1
operation: git-fetch
remote: origin
fetch-url: https://example.invalid/owner/repository.git
transport: https
upstream-ref: refs/heads/main
destination-ref: refs/remotes/origin/main
credential-path: helper=<none>;useHttpPath=<unset>
granted-at: 2026-09-06T00:00:00Z
```

Values are exact observed inputs, not aliases or inferred provider identity. `credential-path` is a non-secret description of the effective credential/helper route: ordered helper identities and applicable path-scoping settings for HTTPS, or the effective SSH command/config identity for SSH. Never record credentials, tokens, usernames embedded in URLs, helper output, environment secrets, OIDs, or fetched data.

## Lifecycle

- **Read:** session-start may read the fixed file without mutation after resolving the selected repository. Absence, malformed content, unsupported schema, duplicate fields, or unsafe/sensitive values means no grant.
- **Grant:** only a current explicit human instruction to remember fetch consent may create or replace the file. The visible confirmation names the operation, remote, sanitized URL identity, upstream, destination, and revocation instruction.
- **Use:** before each fetch, session-start re-resolves every binding from current Git configuration and reports that remembered consent is being used. Reading or using the grant never refreshes, rewrites, or extends it.
- **Revoke:** ordinary human instructions such as “forget fetch consent for this repository” remove the file after reporting the exact local path. Revocation changes no Git configuration and performs no network action.
- **Invalidate:** any mismatch in schema, repository destination, remote name, sanitized fetch URL, URL scheme/transport, upstream ref, destination ref, effective helper order, credential path scoping, or effective SSH command/config makes the grant unusable. Do not rewrite or delete it automatically; report the mismatch category and fall back to interactive approval.

Moving or copying the file does not transfer consent: validity also depends on the current repository's resolved Git directory/worktree destination and exact bindings. A remote redirect observed only during network access does not silently broaden the stored identity; a materially different endpoint requires fresh consent.

## Fetch Boundary And Session-Start Transport

Session-start derives identity read-only from the current repository and Git configuration: selected repository root/Git directory, current branch, configured upstream, upstream remote name, exact remote fetch URL, URL scheme, destination remote-tracking ref, and effective credential/helper route. It then consumes the fixed Markdown file directly. No parser service, daemon, database, runtime API, or global preference state is introduced.

A remembered grant authorizes one bounded command equivalent to:

```text
git fetch --no-recurse-submodules --no-auto-maintenance <remote> <upstream-ref>:<destination-ref>
```

The existing non-interactive HTTPS or SSH transport safeguards still apply. `--no-recurse-submodules` prevents the grant from reaching other repositories. `--no-auto-maintenance` prevents an approved fetch from initiating automatic maintenance outside the named operation; explicit maintenance stays separate. Git LFS operations are excluded. Fetch may still write local objects, the bound remote-tracking ref, and `FETCH_HEAD`; invoke configured fetch hooks, the selected credential/helper path, and remote transport helpers; and disclose connection/repository/client/OID information to the remote. These effects are why network approval remains the default. `GIT_TERMINAL_PROMPT=0` does not neutralize graphical credential helpers.

Fast-forward integration is not fetch consent. Existing combined current-session sync intent may still cover bounded fetch plus fast-forward-only integration, but a remembered fetch grant alone authorizes no integration. Commit, push, publication, provider tooling, destructive actions, conflict resolution, remote changes, and credential-path changes retain separate authority.

## Reporting And Privacy

Every remembered use reports `operation`, `remote`, sanitized host/scheme identity, upstream/destination refs, and how to revoke. Reports never print userinfo, query strings, fragments, credentials, helper output, private URL paths when repository policy marks them sensitive, or fetched OIDs. When safe URL identity cannot be displayed, report a redacted stable description and require interactive approval rather than weakening the binding.

## Acceptance Criteria

- Default and invalid/missing grant paths ask the existing single network question and perform no fetch first.
- An explicit grant is repository-scoped, operation-bound, remote-bound, upstream/destination-bound, visible on use, and simply revocable.
- Every listed material change invalidates use without mutating the preference.
- The authorized fetch is non-interactive, exact-ref bounded, `--no-recurse-submodules`, and `--no-auto-maintenance`.
- Fetch consent cannot authorize LFS, submodules, integration, commit, push, provider, publication, destructive work, or RH-W-0018 low-question behavior.
- Plain reads/checks do not create directories, files, consent, or Git changes.
- English and Spanish documentation describe the same lifecycle while RH-W-0046 remains `proposed` and RH-W-0018 remains `deferred`.

## Evidence

Primary Git documentation reviewed for this decision: [`git-fetch`](https://git-scm.com/docs/git-fetch), [protocol v2](https://git-scm.com/docs/gitprotocol-v2), [credentials](https://git-scm.com/docs/gitcredentials), [remote helpers](https://git-scm.com/docs/gitremote-helpers), [hooks](https://git-scm.com/docs/githooks), and [submodules](https://git-scm.com/docs/gitsubmodules). Repository evidence is owned by the two roadmap records and the current session-start contract. The external facts are design inputs, not claims that this proposal is implemented.

## Non-Goals

No global fetch exception, consent ledger, cache, database, service, daemon, protocol, expiry scheduler, automatic cleanup, generalized preference framework, product implementation, tests, runtime projection, or implementation plan. Owner-delegated design approval does not authorize implementation planning, implementation, or delivery.
