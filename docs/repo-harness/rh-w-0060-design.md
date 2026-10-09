# RH-W-0060 Repository State Reporting Design

> **Status: RH-W-0060 is `completed`.** The [roadmap record](roadmap.md#rh-w-0060) owns its outcome; the text below is retained as historical evidence.

Status: `proposed`; C11-only design approved by the owner. Implementation planning and implementation are separately unauthorized.

## Decision

Add one conditional clause to generated session-stop for C11. Its Git work-unit classification already owns staged, unstaged, and untracked reporting. When EOL or filter normalization plausibly explains a tracked path's observed bytes, compare the index blob or Git's path-aware cleaned worktree view, as applicable, with the committed `HEAD:<path>` blob. Equality means EOL/filter-only, not content-modified. A substantive difference remains content-modified. Unavailable or ambiguous evidence yields unknown.

C12 is closed with no generated residue. Branch enumeration and ancestry analysis belong only to an explicitly requested branch review, not routine generated session-start behavior. Existing session-start reporting of the checked-out branch and upstream remains unchanged.

## Why generated behavior is justified

Repository-only policy was preferred unless a consumer need existed. C11 occurs inside the already-generated session-stop report that classifies Git paths before staging. The builder also emits path-specific text/EOL rules for byte-compared generated files. Keeping the correction only in this repository's working agreements would leave the consumer report that owns the claim unchanged.

No new owner is introduced. The working agreements retain proportionality and stopping rules; they do not duplicate Git-reporting mechanics.

## Alternatives considered

1. **Repository-local working agreement only.** Smallest edit, but rejected because it fixes only this self-hosted repository while generated consumer claims remain unchanged.
2. **One conditional clause in generated session-stop — selected for C11.** Repairs the claim where it is made and reuses Git.
3. **Generated session-start branch inventory — rejected for C12.** Branch enumeration belongs to an explicitly requested branch review; routine branch and upstream reporting stays unchanged.

## Behavioral boundaries

### C11 content classification

The extra comparison runs only for a tracked text path whose attributes or observed bytes make normalization plausible. Compare the index blob or Git's path-aware cleaned worktree view to the committed blob. Do not hide substantive changes merely because line endings also differ.

Name the compared representations and result. If the committed blob is absent, attributes are ambiguous, a custom filter prevents reliable comparison, or Git cannot produce one side, classification remains unknown with the missing evidence stated.

## Acceptance obligations for a later authorized implementation

Focused tests must first fail for these exact behaviors:

1. EOL-only is not content-modified.
2. A substantive difference remains content-modified.
3. A differing index blob remains content-modified.
4. Insufficient evidence yields unknown.

Canonical generated source and required runtime projections would change only after separate approval. This design adds no implementation plan and advances no lifecycle state.

## Explicit exclusions

No session-start change; branch inventory or ancestry behavior; automatic branch cleanup or deletion; fetch, network, credential, or transport changes; conflict detection or resolution; `.gitattributes` redesign; broad Git inventory engine; persistent state; new command, API, dependency, tool, outcome, or authority store.
