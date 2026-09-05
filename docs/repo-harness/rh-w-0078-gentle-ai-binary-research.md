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
