# RH-W-0081 Host-Local Changelog Chronology Design

> **Status: OWNER-APPROVED DESIGN. A proposed implementation plan exists; implementation remains separately unauthorized.**

## Decision

Human-facing changelog chronology derives dates from the host-local timestamp of the completed change. Use the exact local timestamp when needed, or the local calendar date with its offset. Never silently substitute UTC or invent a rounded minute.

The pending correction is to merge both `2026-08-29` changelog sections into `2026-08-28`. Local Git records `1e5759e0` at `2026-08-28T22:33:40-06:00` and `c892cfb` at `2026-08-28T22:40:57-06:00`. This design records but does not perform that correction.

## Ownership Boundaries

- RH-W-0068 remains the sole owner of changelog disposition: whether closure requires an entry.
- RH-W-0081 solely owns host-local date derivation for human-facing changelog chronology.
- RH-W-0069 owns machine-oriented continuity backup identity and ordering. Its UTC filename and `timestamp_utc` metadata neither derive nor control changelog dates, and RH-W-0081 does not alter backup identity or ordering.
- RH-W-0080 owns only ephemeral batch interaction. Batch activation and grouped answers grant no chronology or approval authority.

## Derivation Contract

1. Use the host-local system clock for a completed non-Git change. For completed Git work, prefer verified local Git evidence tied to the exact change.
2. Use one heading per host-local calendar date.
3. For non-Git work when the host-local clock was not observed, or whenever local Git evidence is unavailable, contradictory, or ambiguous, ask one separate resumable clarification question. Do not infer a timestamp from filesystem metadata and do not group the question as authority.
4. Preserve exact seconds and offset when presenting a timestamp. If minute precision is unnecessary, state only the local date and offset.

## Exclusions

No product behavior, test, generated projection, changelog correction, Git action, network access, provider action, publication, or change to RH-W-0069 backup identity is authorized by this design or the proposed plan.

## Later Verification Targets

The proposed [implementation plan](rh-w-0081-implementation-plan.md) covers host-local derivation, exact timestamp handling, ambiguity behavior, one-heading-per-local-date reconciliation, UTC backup independence, bilingual parity, and the recorded changelog correction. It awaits owner approval.
