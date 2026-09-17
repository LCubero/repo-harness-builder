# RH-W-0078 Gentle AI Binary Research

## Conclusion

**Supported inference:** Gentle AI supports **small loaded skill bodies, progressive disclosure, and deterministic machine ownership for behavior that does not require model judgment**. It does not justify adding a compiled binary to this Python builder. For `RH-W-0078`, the proportionate recommendation is to let the existing renderer emit multi-file generated skills and move only suitable deterministic mechanics into the existing Python renderer or checker. **Verified fact:** exact design and implementation remain unauthorized.

**Verified fact:** research was read on 2026-09-04 at Gentle AI revision `c24a724a1fc109a0cd488e0562feff916a41a7a9` and release `v2.6.0`. No repository was cloned, no package was installed, and no Gentle AI source or runtime command was executed.

## Questions

1. Which Gentle AI mechanisms reduce model-visible context rather than only reorganizing source code?
2. Which responsibilities can move from prose to deterministic machinery without hiding a human decision from the model?
3. Should this repository add a compiled binary, extend its existing Python renderer/checker, or only split generated skills?
4. What evidence is still missing before an `RH-W-0078` scope can be approved?

## Sources

**Verified fact:** all sources are class 2 primary-source evidence from the project owner.

| Source | Source class | Read | Refresh trigger |
| --- | --- | --- | --- |
| [Gentle AI README at the reviewed revision](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/README.md) | 2 | 2026-09-04 | The documented product architecture, supported runtimes, or stable-version statement changes |
| [Architecture](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/architecture.md) | 2 | 2026-09-04 | Component ownership or binary/runtime boundaries change |
| [Skill style guide](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-style-guide.md) | 2 | 2026-09-04 | Skill loading, size, or reference guidance changes |
| [Skill registry](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-registry.md) | 2 | 2026-09-04 | Discovery or loading behavior changes |
| [Shared review-ledger contract](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/skills/_shared/review-ledger-contract.md) | 2 | 2026-09-04 | Shared skill-contract composition changes |
| [OpenCode review transport plugin](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/opencode/plugins/opencode-review-transport.ts) | 2 | 2026-09-04 | Plugin transport ownership or runtime integration changes |
| [Claude](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/claude_adapter.go), [Codex](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/codex_adapter.go), and [Pi](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/reviewerprovider/pi_adapter.go) reviewer adapters | 2 | 2026-09-04 | Provider boundaries or adapter responsibilities change |
| [Gentle AI v2.6.0 release](https://github.com/Gentleman-Programming/gentle-ai/releases/tag/v2.6.0) | 2 | 2026-09-04 | The release is amended or superseded for the facts cited here |

## Current Refresh (2026-09-17; separate from pinned research)

**Verified fact:** the selected local reference snapshots were read at exact local Git commits: `gentle-shell` [`2b579c80824e83442b8ae9f7bfad629a52f9f711`](https://github.com/Gentleman-Programming/gentle-pi/tree/2b579c80824e83442b8ae9f7bfad629a52f9f711), Gentle AI [`eae8fadd5ece2622cb072c836fa49cef520afc8e`](https://github.com/Gentleman-Programming/gentle-ai/tree/eae8fadd5ece2622cb072c836fa49cef520afc8e), and Engram [`f6b22f89d024fb6aaa207606d893f47b3f4e23a9`](https://github.com/Gentleman-Programming/engram/tree/f6b22f89d024fb6aaa207606d893f47b3f4e23a9). They are current local refresh evidence only; the revision-pinned research above remains separate historical evidence and was not revised to claim that it observed these facts.

**Verified fact:** [Gentle AI](https://github.com/Gentleman-Programming/gentle-ai) keeps ODD as its default workflow, requires explicit SDD selection, and treats RDD as user-owned and opt-in. Its SDD archive is not review approval or delivery. [gentle-shell](https://github.com/Gentleman-Programming/gentle-pi) closes each substantial ODD task with a work-unit commit, treats a work-unit commit or PR slice—not a checkbox or accumulated feature branch—as the native review candidate, and uses chained delivery planning around 400 authored changed lines. When background-subagent policy is active and `subagent_run` is available, routine delegation defaults to background. `gentle-shell` is the Pi-native orchestration surface for these bounded subagents, ODD task artifacts, optional SDD, review-transport/mirror contracts, and optional Engram composition. Its project-scoped Engram mirror is specific to ODD coordination, not the limit of Engram's continuity capability; native review semantics remain Gentle AI/provider-owned. Its current product/repository name is `gentle-shell`, while `gentle-pi` still appears in npm, runtime, package, and source-repository identities during the mixed naming transition. [Engram](https://github.com/Gentleman-Programming/engram) at the refreshed revision uses direct backup format `0.2.0` to preserve local pins and complete memory-relation metadata while accepting legacy `0.1.0` backups that omit them.

**Supported inference:** these ownership boundaries reinforce the existing recommendation to transfer only progressive disclosure and deterministic mechanics that the Python renderer/checker already owns. They do not support transferring Gentle AI's review lifecycle or adding a new binary. To raise this inference to a future scope decision still requires measured loaded-context and reference-reachability evidence across all supported runtimes.

**Human decision required:** `RH-W-0078` remains proposed. This refresh authorizes no design, implementation, lifecycle change, or delivery. Refresh this section when the stable official repositories above change their documented ownership, workflow, naming, Pi setup, or runtime boundaries.

## Findings

### Model-visible context is the relevant measure

**Verified fact:** Gentle AI separates concise agent-facing skills and shared contracts from deterministic orchestration, plugins, and provider adapters. The [skill registry](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-registry.md), [style guide](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/skill-style-guide.md), and [_shared contract](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/skills/_shared/review-ledger-contract.md) show that reusable detail can live outside a single entry body and be reached when applicable. **Supported inference:** this transfers to `RH-W-0078`: reducing the always-loaded body matters; merely moving the same prose into another always-loaded file does not.

### Deterministic mechanics can leave prose, decisions cannot

**Verified fact:** the [architecture](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/docs/architecture.md), [runtime plugin](https://github.com/Gentleman-Programming/gentle-ai/blob/c24a724a1fc109a0cd488e0562feff916a41a7a9/internal/assets/opencode/plugins/opencode-review-transport.ts), and provider adapters place transport, validation, and provider-specific invocation in machine-owned boundaries. **Supported inference:** that pattern can transfer where this repository already has deterministic Python ownership: rendering, schema validation, exact-path checks, and byte comparison. Consent semantics, authority boundaries, unresolved choices, and action-point instructions should remain visible to the model at the point where it acts.

### A new compiled binary is disproportionate here

**Verified fact:** Gentle AI's compiled program is part of its established product architecture. This repository already distributes a Python renderer/checker and supports generated files across three runtimes. **Supported inference:** adding another executable would create packaging, platform, release, provenance, and recovery obligations without evidence that compilation itself reduces the two measured skill bodies. Extending the existing Python ownership achieves the transferable benefit with a smaller architectural change.

### Progressive disclosure must be proved, not assumed

**Unknown or unverifiable:** whether a generated `references/` surface reduces effective context before each supported agent is shown to load the entry skill without automatically loading every reference and to reach the required reference reliably at its action point. **Verified fact:** `RH-W-0084` established that instruction distance matters. **Supported inference:** the future scope therefore needs body measurements and loading/reachability tests for Claude Code, OpenCode, and Pi; file splitting alone is not proof of a smaller effective context footprint.

## Transfer and non-transfer

| Classification | Result for RH-W-0078 |
| --- | --- |
| Transfer | **Supported inference:** multi-file skills, concise entry contracts, progressive disclosure, shared reusable references, and deterministic transport or validation outside prose |
| Transfer with proof | **Unknown or unverifiable:** runtime-specific loading and reference reachability until the effective loaded context is measured for all three supported runtimes |
| Do not transfer | **Supported inference:** a new compiled binary, Gentle AI's Go package layout, provider-specific reviewer architecture, or its review-ledger lifecycle |
| Never cut blindly | **Supported inference:** consent, authority, transport safety, evidence classification, and action-point obligations should retain a verified reachable owner |

## Evidence classification and gaps

- **Supported inference:** a new compiled binary is not proportionate to the measured `RH-W-0078` defect; the existing Python renderer/checker is already the machine-owned delivery boundary.
- **Unknown or unverifiable:** whether generated references reduce effective context, because the result depends on how each supported runtime loads skill resources.
- **Unknown or unverifiable:** the exact body reduction, reference grouping, and runtime-specific retrieval behavior until a bilingual scope defines and measures them.
- **Verified fact:** the reviewed source `README.md` still names `v2.5.0` as stable, while the immutable official release page identifies `v2.6.0`. Release evidence governs release facts; the discrepancy does not change the architectural findings.
- **Human decision required:** approve, correct, or reject the recommended direction in a future bilingual scope; exact design and implementation remain separately unauthorized.

## Recommendation

**Supported inference:** the future bilingual scope should price one preferred direction: generate multi-file skills with a small entry contract, move deterministic mechanics selectively into the existing Python renderer/checker, preserve model-visible decisions and action-point rules, and prove effective loaded-context reduction plus reference reachability across Claude Code, OpenCode, and Pi. It should reject both a new compiled binary and blind content cutting. **Verified fact:** this recommendation informs `RH-W-0078`; it does not approve a design, implementation, lifecycle change, or delivery.
