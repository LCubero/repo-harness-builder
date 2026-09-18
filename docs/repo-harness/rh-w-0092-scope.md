# RH-W-0092 Proposed Scope

**Status: proposed; corrected bilingual scope approved by the owner.** This artifact proposes Windows long-path guidance only. Scope approval, design, implementation, and delivery remain separate, and this document does not approve itself. The [roadmap record](roadmap.md#rh-w-0092) owns lifecycle.

## Verified Baseline

Current generated `.gitattributes` already protects every byte-compared generated path with exact `text eol=lf` rules, and verification covers `core.autocrlf=true`. Do not reopen or redesign that behavior without a measured defect.

## Proposed Direction

Detect and suggest guidance only when an actual Windows path-length condition affects an operation. The human-run remediation option is:

```text
git config --global core.longpaths true
```

It changes the user's global Git configuration, so it must never run automatically, is never a prerequisite, is not relevant on non-Windows, and is not suggested when no relevant failure exists. It grants no authority to modify global configuration.

## Boundaries

- Preserve ordinary local work when guidance is unnecessary.
- No registry or OS-policy change, manifest field, preference, state, repeated startup warning, dependency, or platform abstraction.
- No product, generated-contract, runtime-projection, `.gitattributes`, Git configuration, test, network, or delivery change under this scope-writing unit.

## Final Decision

The owner approved the corrected bilingual scope. Exact detection evidence and wording await a separately authorized design for conditional detection and suggestion only; implementation and delivery remain separately unauthorized.
