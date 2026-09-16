# RH-W-0046 Repository Fetch-Consent Implementation Plan

> **Status: completed historical execution record.** RH-W-0046 is `completed`; RH-W-0018 remains `deferred` and out of scope. Lifecycle reconciliation, final verification, and native approval are complete. No Git, network, commit, push, PR, delivery, release, or publication action occurred.

Derived from the owner-approved [design](rh-w-0046-rh-w-0018-design.md). Spanish peer: [rh-w-0046-implementation-plan.md](../es/repo-harness/rh-w-0046-implementation-plan.md).

## Exact Owners And Boundary

`skill/repo-harness-builder/scripts/harness_tool.py::session_start_skill()` is the only behavior owner. `skill/repo-harness-builder/tests/test_harness_tool.py::GeneratedCoreTests` owns its rendered-contract regression tests; `render()` and `render_with_skill_mapping()` project approved canonical changes. Keep the default network approval. Add only the approved one-repository, one-remote, exact-ref, visible, revocable `git-fetch` grant at `.repo-harness/state/preferences/fetch-consent.md`.

No RH-W-0018 behavior, general preference framework, parser service, daemon, database, cache, expiry scheduler, auto-cleanup, Git delivery authority, or provider authority is included.

## Task 1: Consolidated focused test (completed historical execution)

- [x] Implemented and passed `GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed`.
- [x] The consolidated focused test verifies the exact fixed-file schema, canonical base64url fields, fail-closed malformed/mismatch/ambiguous handling, and interactive fallback.
- [x] The four planned method names above were consolidated into this one focused test; they were not created as four separate methods.
- [x] Ran `python -B skill/repo-harness-builder/tests/test_harness_tool.py GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed -v` successfully.

## Task 2: Minimal implementation and rollback boundary (completed historical execution)

- [x] Extended only `session_start_skill()` with the approved fixed-file consent boundary, including current-explicit-instruction-only creation or replacement, invalidation, visible use, revocation, and exact fetch.
- [x] Preserved the boundary that reading or using a grant never creates, refreshes, rewrites, extends, deletes, or transfers it; replacement is never inferred or automatic.
- [x] Rollback remains historical and bounded: revert the exact `session_start_skill()` clauses and their matching test together; never remove a local grant automatically or alter Git configuration, history, remotes, or credentials.

## Task 3: Established projections and lifecycle reconciliation (completed)

- [x] Synchronized the separately authorized candidate through established runtime projections.
- [x] Completed final verification, native approval, and reconciliation across the roadmap, current-state projection, maps, Spanish peers, changelog, and `work/RH-W-0046` continuity topic.
- [x] RH-W-0046 is `completed`; RH-W-0018 remains `deferred` with its separate implementation and advancement condition.

## Bounded Verification

- [x] The actual consolidated focused test passed: `GeneratedCoreTests.test_session_start_fetch_consent_schema_is_exact_and_fail_closed`.
- [x] Independent full-suite evidence: 547 passed/10 skipped; checker `current` with 0 writes; exact projection parity and `git diff --check` clean.
- [x] Native approval lineage `review-9d5afc68be68d0be` was acknowledged with authority burned for target `sha256:628449dd9144825559da6617e237b3d09a80dbd851c39522e6f57ab54b3c5e23`.
- [x] No network access, live fetch, Git delivery, or RH-W-0018 behavior was selected.

## Review Workload And Decisions

The completed review remained bounded to the consent boundary: invalidation and exclusions first, then the consolidated focused test and independent verification. Historical plan approval and separate implementation authority are retained. RH-W-0046 is complete; reopen only for a regression of this exact consent boundary. RH-W-0018 remains `deferred` and out of scope. No Git, network, commit, push, PR, delivery, release, or publication action occurred.
