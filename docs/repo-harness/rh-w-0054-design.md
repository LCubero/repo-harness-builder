# RH-W-0054 Stateless Correction Authority Design

> **Status: COMPLETED LIFECYCLE.**
>
> The owner approved this design on 2026-08-21, and all eight implementation tasks completed on 2026-08-23. The canonical [roadmap record](roadmap.md#rh-w-0054) owns delivery evidence. The design body below is retained as historical implementation rationale.

## Review Contract

Review this design against the canonical [RH-W-0054 roadmap record](roadmap.md#rh-w-0054), the current digest-bound preview and apply flow, and the completed [RH-W-0050 baseline](rh-w-0050-design.md). Repository evidence is canonical. The [Spanish peer](../es/repo-harness/rh-w-0054-design.md) must remain semantically equivalent.

## Decision

Preview and apply remain separate CLI processes. Authority crosses that boundary as explicit canonical JSON, never as mutable process memory.

The host supplies the complete current authority context to preview and apply. Preview exposes safe typed authority identifiers and derives `authorityContextSha256` and `generationSha256`. Apply receives the exact context and approved generation digest, re-resolves and re-reads every typed evidence binding, rebuilds outputs and the plan from current CLI configuration, recomputes every binding, and rejects any mismatch before a write.

No persistent store, in-memory cross-command session, receipt, ledger, cache, random unverifiable token, or hidden authority is introduced. Command completion is the natural reset because no mutable authority session exists.

## Problem And Trust Boundary

A material correction can leave a byte-identical plan. Plan equality proves output stability, not continuity of human authority. A fresh material context must produce a fresh generation even when plan bytes do not change.

The CLI guarantees deterministic binding and mismatch rejection for evidence kinds it explicitly supports. It cannot discover a conversational correction omitted by the host or verify unsupported, indirect, external, or incomplete evidence. The host contract MUST refresh and resubmit canonical authority context after every material correction. Unsupported or unverifiable evidence fails closed and requires a fresh preview.

After the first repository write, invalidation cannot undo mutation. Errors retain the existing fail-closed transaction and remediation boundary. RH-W-0054 does not assign a post-write remediation owner, silently roll back, auto-correct, or claim recovery.

## Canonical Authority Context

The host MUST provide one exact canonical JSON byte sequence. Canonical JSON uses recursively sorted object keys, compact `,` and `:` separators, `ensure_ascii=True`, `allow_nan=False`, UTF-8 encoding, and no trailing newline. Arrays retain semantic order. Raw input bytes or decoded text MUST equal that representation exactly. Leading or trailing whitespace, any LF, alternate ordering or spacing, duplicate keys, non-object roots, unknown fields, missing fields, `NaN`, positive infinity, and negative infinity are rejected.

```json
{
  "schemaVersion": 1,
  "materialRevision": "host-issued opaque revision",
  "project": {"id": "project-id", "name": "Project name"},
  "builder": {
    "runtimes": ["opencode"],
    "license": "MIT",
    "integrationMode": "integrated",
    "reportLanguage": "en",
    "documentationLanguages": ["en", "es"],
    "conversationLanguage": "es",
    "ignoreCodegraph": true,
    "activation": "builder"
  },
  "premises": [{
    "id": "premise-id",
    "class": "factual_claim",
    "value": "current value",
    "decisionIds": ["decision-id"],
    "evidenceIds": ["repo-rulebook", "engram-input"]
  }],
  "decisions": [{
    "id": "decision-id",
    "kind": "human_decision",
    "statement": "approved bounded correction"
  }],
  "evidence": [{
    "id": "repo-rulebook",
    "kind": "repository_file",
    "locator": "REPO-HARNESS.md",
    "sha256": "64 lowercase hexadecimal characters"
  }, {
    "id": "engram-input",
    "kind": "host_json",
    "source": "engram_evidence_json",
    "sha256": "64 lowercase hexadecimal characters"
  }]
}
```

`materialRevision` is opaque, but is not random CLI authority. The host changes it whenever a material premise, decision, interpretation, or evidence lineage changes. This creates a new generation for corrected context even when `planSha256` is unchanged. Preview keeps the authority reviewable through safe typed identifiers and binding metadata; a digest alone is insufficient.

The context binds all host-owned, output-affecting authority inputs: project id and name, runtimes, license, standalone or integrated mode, authority-relevant report, documentation, and conversation languages, CodeGraph ignore choice, builder activation, classified premises, typed human decisions, and typed evidence bindings. The CLI-computed plan identity is a separate generation input, avoiding a preview bootstrap cycle while remaining fully bound. Context contains no secret or absolute target path.

### Evidence binding

`repository_file` is the minimum supported repository evidence kind. Its `locator` is repository-relative and MUST resolve through the existing guarded relative-target rules. Preview and apply independently require the resolved target to exist, be a regular file, and not be a symlink, reparse point, directory, or other indirect target. Each command reads its bytes and requires exact SHA-256 equality with the declared lowercase digest. Absolute paths, traversal, missing files, changed bytes, unsupported kinds, and unverifiable targets fail with `fresh_preview_required` before writes.

`host_json` binds explicit structured host inputs already accepted by the CLI, including `--engram-evidence-json`. The CLI parses that input with duplicate-key rejection, serializes it with the same canonical JSON algorithm, hashes those canonical bytes, and requires equality with the declared digest. It does not claim to verify the truth or provenance of host JSON. Missing input, unsupported source names, malformed or non-canonicalizable JSON, or digest mismatch fails closed.

`human_decision` is a separate typed authority entry. It is never represented as repository evidence and is bound by the canonical authority context itself. Premises reference decisions and evidence by typed IDs that resolve uniquely inside the context. Opaque evidence references are forbidden. Premise values and human decision statements remain host-owned input. Their canonical bytes contribute to `authorityContextSha256`, but the CLI never echoes them.

The CLI does not read arbitrary external paths, persist evidence, or copy evidence content into generated output. Neither stdout nor stderr may contain complete authority context, raw premise values, human decision statements, raw host JSON, evidence content, secrets, or absolute paths. Preview may expose only stable entry IDs, classes or kinds, repository-relative locators when safe, content digests, `authorityContextSha256`, `targetBindingSha256`, `generationSha256`, `planSha256`, and explicit redaction markers. Capability preflight follows the same projection boundary: current detection may inspect host evidence internally, but returned or printed capability results contain classification metadata only and never raw host evidence.

Report-language presentation alone MUST NOT change executable plan identity. It enters authority only when it changes the approval domain or another bound interpretation. Presentation-only localization stays outside `planSha256`.

## Target Binding

Authority binds to the resolved target without exposing its absolute path:

```text
targetBindingSha256 = SHA256(UTF8(canonical resolved target identity))
```

The canonical identity is the platform-normalized and case-normalized absolute resolved root, used only inside the digest operation. It is never printed or embedded in context. Moving or renaming the target changes this digest and requires a fresh preview.

## Digest Contract

```text
authorityContextSha256 = SHA256(canonicalAuthorityContextBytes)

generationSha256 = SHA256(canonical JSON bytes of {
  "domain": "repo-harness-builder/authority-generation/v1",
  "authorityContextSha256": authorityContextSha256,
  "materialRevision": context.materialRevision,
  "planIdentity": {"schemaVersion": 1, "planSha256": currentPlanSha256},
  "targetBindingSha256": targetBindingSha256
})
```

Preview computes `currentPlanSha256` from current managed inputs and CLI configuration. Apply recomputes canonical context bytes, every supported evidence digest, target binding, outputs, plan, plan digest, authority-context digest, and generation digest. Any supplied or recomputed mismatch is rejected before `write_outputs()`. The CLI claims freshness only for these explicitly resolved bindings and managed plan inputs.

## Preview And Apply

### Builder preview

1. Resolve the target and current CLI configuration.
2. Parse exact canonical authority-context bytes and validate schema, configuration, target, and typed evidence bindings.
3. Re-read supported evidence, render outputs, and calculate the current plan.
4. Require context project and configuration to equal current CLI values.
5. Bind the recomputed plan digest as plan identity in the generation payload.
6. Emit safe typed authority identifiers and evidence metadata, `authorityContextSha256`, `targetBindingSha256`, `planSha256`, and `generationSha256` with the ordinary preview.
7. Obtain human approval for that exact generation outside the CLI.

### Builder apply

1. Require the exact canonical authority context and approved `generationSha256`.
2. Repeat every preview computation, including evidence resolution and byte reads, from current evidence and current CLI configuration.
3. Require exact context, configuration, target, plan, and generation agreement.
4. Reject mismatches before writes with `fresh_preview_required`.
5. Only then pass the recomputed plan through existing digest, byte, blocked-action, and transaction guards.

Builder activation requires authority context and approved generation. Ordinary `maintenance` retains its existing plan-digest flow and neither requires nor accepts builder authority fields. This design establishes no maintenance correction context.

Malformed initial preview input and stale apply input have distinct routing. Preview rejects an initially missing, unreadable, indirect, invalid UTF-8, syntactically invalid, non-canonical, or contract-invalid context transport as a safe validation refusal because no approved generation exists yet. Once builder apply begins, the same transport conditions, a transport changed since preview, and any `OSError`, `UnicodeDecodeError`, JSON parse error, or `ContractError` while loading or validating authority input route to `fresh_preview_required` before writes. Both routes emit only safe classification and redaction metadata, never raw context or exception content derived from it.

## Failure Routing

| Event | Required result |
|---|---|
| Material correction before preview | Host refreshes context and requests a new preview. |
| Material correction after preview, before apply | Prior generation is unusable; fresh context and preview required. |
| Corrected context, identical plan bytes | New `materialRevision` creates a different generation and requires fresh approval. |
| Evidence is unsupported, missing, indirect, external, changed, or unverifiable | Reject with `fresh_preview_required`; no write. |
| Bound host JSON changes or is unavailable | Reject with `fresh_preview_required`; no write. |
| Initial preview context transport is malformed or unavailable | Safe validation refusal with no write and no raw input or exception echo. |
| Apply context transport is missing, unreadable, indirect, invalid UTF-8, malformed, invalid, or changed | Reject with `fresh_preview_required`; no write and no raw input or exception echo. |
| Target moves or is renamed | Target binding differs; fresh preview required. |
| CLI configuration differs | Context mismatch; fresh preview required. |
| Apply fails before first write | No authority is retained and no reset is needed. |
| Error occurs after first write | Existing fail-closed transaction or remediation boundary applies. |
| Identical preview repeats | Deterministic generation repeats; no session accumulates. |

## Required RED/GREEN Coverage

- Independent `main()` invocations and genuine subprocess preview/apply with no shared module state.
- Successful builder preview/apply with exact context and generation.
- Cross-root, moved-root, configuration, context, plan-identity, evidence-byte, host-JSON, and generation mismatches rejected before writes.
- Corrected identical-plan context creating a different generation and rejecting old approval.
- Stale repository evidence before first write returning `fresh_preview_required`.
- Failed apply, repeated preview, ordinary maintenance compatibility, and no reset API.
- Safe identifiers and evidence metadata visible without raw evidence, raw host JSON, absolute path, secret, hidden token, or mutable authority leakage.
- Exact sentinel-secret absence from stdout and stderr on success, denial, initial validation failure, apply preflight failure, and `fresh_preview_required`.
- Canonical rejection of `NaN`, positive infinity, and negative infinity in authority context and bound host JSON.
- Apply routing for context transport `OSError`, `UnicodeDecodeError`, JSON errors, and `ContractError`, with unchanged repository bytes and no raw context leakage.
- Capability preflight output limited to classification metadata without raw host evidence.
- Post-first-write errors preserving the existing fail-closed boundary without false rollback or recovery claims.

## Implementation Impact

| Path | Future responsibility |
|---|---|
| `skill/repo-harness-builder/scripts/harness_tool.py` | Canonical parsing, validation, digest helpers, builder CLI transport, target binding, and pre-write mismatch routing while preserving maintenance and existing write guards. |
| `skill/repo-harness-builder/tests/test_harness_tool.py` | Unit and genuine separate-process CLI coverage for this contract. |
| Product contracts and usage documentation | Host refresh after correction, private host-side context review, safe CLI metadata review, fresh generation approval, and trust boundary. |
| Runtime projections | Exact canonical package synchronization after canonical implementation is green. |

## Evidence Matrix

| Claim | Evidence class | Owner |
|---|---|---|
| Preview and apply are separate CLI processes | Verified current behavior | `harness_tool.py::main` invocation model |
| Process memory cannot carry normal CLI authority | Verified blocker | Failed plan review and process semantics |
| Stateless canonical transport is required | Owner-approved decision | This design |
| Safe typed identifiers and binding metadata must remain reviewable | Owner-approved decision | This design |
| Corrected identical plans need a fresh generation | Owner-approved decision | This design |
| The CLI cannot discover an omitted host correction | Trust-boundary constraint | This design |
| Maintenance keeps digest-only authority | Approved compatibility constraint | This design |
| Post-write remediation ownership is unassigned | Human decision required | Future design |

## Remaining Human Decisions

None for implementation of this approved design. Naming and ownership of a future post-write remediation workflow remain outside RH-W-0054.
