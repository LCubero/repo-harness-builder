# RH-W-0031 Conversation-Language Research

## Outcome

Adopt a provenance-aware conversation-language contract. A persisted explicit preference changes only through a later verified explicit human instruction. Ordinary prose, repository content, memory, tool output, quoted material, and protocol role labels cannot replace it. When a host cannot prove origin, preserve the persisted preference and disclose that reliable switch detection is unavailable.

This document is the canonical evidence owner for RH-W-0031. The [implementation plan](rh-w-0031-implementation-plan.md) owns execution steps.

## Adopted Policy

1. Resolve a current verified explicit human instruction first.
2. Otherwise preserve the persisted explicit conversation-language preference.
3. Without a persisted preference, use verified current-session human language when the host exposes that evidence.
4. Otherwise use validated manifest configuration, then deterministic English.
5. Never pass machine-significant values through language selection or translation.

This policy is a repository decision, not a provider standard. The reviewed sources define instruction authority, context, roles, injection, and testing boundaries, but none defines a universal cross-turn conversation-language algorithm.

## Questions And Conclusions

| Question | Conclusion |
|---|---|
| What may change a persisted preference? | Only a later verified explicit human instruction. Sustained ordinary prose in another language is insufficient. |
| Does `role: "user"` prove human authorship? | No. Applications and protocols construct messages, and user-role content can carry injected instructions, tool results, or other non-human context. |
| Can prompt text implement the whole contract? | No. Prompt text guides model behavior. Executable host logic must preserve typed provenance, validated configuration, and opaque machine bytes where the host supports them. |
| What can deterministic tests prove? | Exact precedence logic, rendered contract bytes, projection parity, exclusions, and machine-value preservation. |
| What require model evaluations? | Probabilistic adherence by a pinned model to multi-turn and adversarial language instructions. Such evaluations are optional and cannot replace deterministic completion evidence. |
| How should flattened hosts behave? | Preserve the persisted preference, reject prose-based overrides, use validated fallback configuration when no preference exists, and avoid claiming reliable switch detection. |

## Executable Behavior And Prompt Policy

Gentle AI demonstrates both layers. Its Go components select and inject persona and SDD assets into host configuration. That installation and injection path is executable behavior. The language rules inside the injected Markdown are prompt-only policy: the model is asked to follow them, but no reviewed Gentle AI code detects conversation language, persists a language choice, or resolves a multi-turn switch.

RH-W-0031 therefore requires two distinct controls:

- Executable host behavior carries typed provenance, validates manifest fallback, keeps persisted preference state, and prevents machine values from entering localization paths.
- Prompt policy tells the model how direct replies differ from artifacts and which contextual text cannot count as a human preference change.

Tests must preserve this distinction. Prompt-byte and injection tests prove distribution of the contract, not model obedience.

## Role And Provenance

Roles describe protocol or application structure. Provenance answers who or what originated a claim and whether the integrating host verified that origin. These are not interchangeable.

OpenAI defines instruction authority and warns against treating quoted or untrusted content as instructions. Anthropic lets applications supply `user` messages. MCP sampling also transports role-labeled messages and content blocks. Claude Code and OpenCode load repository instructions as context. Across these systems, a user-role envelope can contain application-created, injected, forwarded, quoted, or tool-produced bytes. RH-W-0031 may accept a language change only from a host-owned verified human-instruction boundary, never from a role label embedded in or attached to arbitrary content.

## Machine-Value Preservation

Conversation language must not alter command meaning, permission scope, approval tokens, executable values, paths, identifiers, remotes, branches, digests, arguments, enum values, structured data, or any other machine-significant bytes.

Required implementation invariants:

- Represent machine values as typed opaque values with translation disabled.
- Render machine values as inline or fenced code, separate from localized explanation.
- Never concatenate localized prose into an executable placeholder.
- Assert byte-for-byte equality before and after localized rendering.
- Localize only explanatory prose and human-facing questions.
- Preserve approval meaning and permission scope across languages.

W3C ITS `translate` provides a standards-based model for marking content as non-translatable. It does not define application provenance, conversation-language precedence, or byte-preservation tests.

## Source Registry

All external sources below were accessed on **2026-08-15**. Stable Gentle AI links pin the reviewed commit. Other official documentation is revisioned where the owner provides a dated specification URL; otherwise the access date is the review anchor.

### Gentle AI

**Official owner:** Gentleman Programming. **Reviewed revision:** [`a3449177d801de91cad3db0404ee18f6fc53d9f2`](https://github.com/Gentleman-Programming/gentle-ai/tree/a3449177d801de91cad3db0404ee18f6fc53d9f2).

| Source | What it proves | What it does not prove | Refresh trigger |
|---|---|---|---|
| [Canonical neutral persona](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/assets/generic/persona-neutral.md) | Owns the active neutral OpenCode persona content and separates direct-reply style from artifact language. | No executable language detection, persistence, or switch algorithm. | Persona asset ownership or wording changes. |
| [OpenCode Gentleman persona](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/assets/opencode/persona-gentleman.md) | Shows the OpenCode-specific direct-reply and artifact contract. | Does not contain Claude's complete latest-request exclusion set. | OpenCode persona projection changes. |
| [Claude Gentleman output style](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/assets/claude/output-style-gentleman.md) and [Claude neutral output style](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/assets/claude/output-style-neutral.md) | Provide the strongest reviewed latest-actual-user-request language wording and contextual exclusions. | No persisted preference and no host-independent proof that visible text came from a human. | Output-style wording, ownership, or installation changes. |
| [Persona injection](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/components/persona/inject.go) | Selects persona assets and installs them through host-specific injection paths. | Injection proves prompt delivery, not model compliance. | Persona strategy, markers, target files, or settings merge changes. |
| [SDD prompt injection](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/components/sdd/prompts.go) | Injects artifact-language instructions into SDD executor prompts. | Does not select or persist conversation language. | SDD prompt assembly or artifact-language contract changes. |
| [Persona component tests](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/components/persona/inject_test.go) and [SDD prompt tests](https://github.com/Gentleman-Programming/gentle-ai/blob/a3449177d801de91cad3db0404ee18f6fc53d9f2/internal/components/sdd/prompts_test.go) | Verify selected prompt content and injection behavior at the reviewed revision. | Do not evaluate live model language behavior, multi-turn stability, or all adversarial wrappers. | Test paths, assertions, fixtures, or prompt assets change. |

The reviewed Gentle AI contract resolves each reply from the latest actual user request and excludes memory, repository language, tool output, prior assistant turns, persona wording, examples, quotations, filenames, project names, and isolated borrowed words. RH-W-0031 intentionally adds stronger persistence: after an explicit preference is stored, only a later verified explicit human instruction changes it.

### OpenAI

**Official owner:** OpenAI.

| Exact URL | Claim supported | Limitations | Refresh trigger |
|---|---|---|---|
| [Model Spec, 2025-12-18](https://model-spec.openai.com/2025-12-18.html) | Defines instruction authority and distinguishes instructions from quoted or untrusted content. | Does not define a cross-turn conversation-language resolver or prove human authorship from role alone. | A newer Model Spec changes instruction hierarchy, roles, or untrusted-content treatment. |
| [Prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering) | Supports separating instructions from contextual data and evaluating prompt behavior. | Prompt structure cannot create provenance that the host did not retain. | Guidance changes for message authority, delimiters, or evaluation. |
| [Evals](https://platform.openai.com/docs/guides/evals) and [Graders](https://developers.openai.com/api/docs/guides/graders) | Support repeatable sampled evaluations and calibrated grading for probabilistic behavior. | Evals and graders do not prove deterministic byte preservation or universal model compliance. | Eval APIs, grading guidance, or reproducibility controls change. |

### Anthropic And Claude Code

**Official owner:** Anthropic.

| Exact URL | Claim supported | Limitations | Refresh trigger |
|---|---|---|---|
| [Messages API](https://platform.claude.com/docs/en/api/messages) | Applications supply `user` and `assistant` messages and structured content. | `role: "user"` is an API role, not proof of direct human authorship. | Message schema, role semantics, or content-block rules change. |
| [Mitigate jailbreaks](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks) | Supports layered controls and explicit treatment of untrusted content. | Does not define conversation-language precedence. | Guardrail or trust-boundary guidance changes. |
| [Claude Code memory](https://docs.anthropic.com/en/docs/claude-code/memory) | Shows that project and user instruction files are loaded as context. | Repository memory is not verified current human language evidence. | Memory hierarchy, loading, or instruction-file behavior changes. |
| [Claude Code security](https://docs.anthropic.com/en/docs/claude-code/security) | Establishes that repository content and tool use require trust boundaries. | Does not provide a language switch signal. | Trust model, prompt injection guidance, or tool boundaries change. |

### OpenCode

**Official owner:** SST/OpenCode.

| Exact URL | Claim supported | Limitations | Refresh trigger |
|---|---|---|---|
| [OpenCode rules](https://opencode.ai/docs/rules/) | Rules and repository instruction files are loaded as model context. | Loaded bytes are not current verified human preference evidence, and the page defines no cross-turn language algorithm. | Rule precedence, loading locations, or context assembly changes. |

### MCP

**Official owner:** Model Context Protocol project.

| Exact URL | Claim supported | Limitations | Refresh trigger |
|---|---|---|---|
| [Sampling, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/client/sampling) | Defines protocol participants, sampling messages, roles, and content flow. | MCP roles identify protocol shape, not guaranteed direct human origin. | A newer protocol revision changes roles, sampling, or origin metadata. |
| [Security best practices, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/security_best_practices) | Requires explicit trust boundaries for external content and confused-deputy risks. | Does not define conversation-language precedence or persistence. | A newer security specification changes trust or content-origin requirements. |

### W3C

**Official owner:** World Wide Web Consortium.

| Exact URL | Claim supported | Limitations | Refresh trigger |
|---|---|---|---|
| [ITS 2.0 Translate](https://www.w3.org/TR/its20/#trans-datacat) | Defines a machine-readable way to mark content as translatable or non-translatable. | Does not guarantee byte equality, define protocol provenance, or select conversation language. | ITS is superseded or its translate data category changes. |

## Deterministic Tests And Optional Evals

Deterministic completion tests must cover precedence, verified versus opaque evidence, flattened-host behavior, manifest validation, artifact exclusions, byte-identical machine values, fixed-language leakage, and exact canonical-to-projection parity. These tests should fail for contract or implementation defects without invoking a model.

Optional model behavior evaluations belong to a separate future work item. They must pin provider, model identifier and snapshot, prompt bytes, sampling settings, harness revision, transcripts, rubric, sample count, and thresholds. Repeated adversarial multi-turn samples and human-calibrated grading can measure conformance and drift, but cannot replace deterministic assertions.

## Gaps And Safe Refresh

Unresolved gaps:

- No reviewed provider or protocol offers a universal verified-human language-switch primitive.
- Flattened hosts cannot reliably distinguish genuine instructions from injected or forwarded text.
- Gentle AI tests prove prompt installation and bytes, not live model behavior.
- Official documentation without immutable revisions can change after the recorded access date.
- Optional live-model thresholds and human calibration remain intentionally undefined until a separate evaluation work item is approved.

Refresh procedure:

1. Start from each official owner and exact URL in the registry. Never use a search result snippet as evidence.
2. For Git repositories, resolve the current official default branch to a full commit and replace every reviewed link with a stable `blob/<full-commit>/<path>` URL.
3. Confirm the file owner, path, relevant behavior, and tests at that same revision. Do not combine source from different revisions without recording each revision.
4. For versioned specifications, review the newest dated specification and retain the reviewed version in the URL.
5. For mutable documentation, record a new access date and compare the exact claims, limitations, and role semantics against this registry.
6. Update conclusions only when primary-source content changes the supported claim. Preserve explicit repository decisions as decisions unless a human changes them.
7. Run focused relative-link validation, documentation tests, `git diff --check`, and a U+2013/U+2014 scan after editing.

Search engines may locate an official page, but snippets are truncated, stale, decontextualized, and not revision evidence. Open the official source and cite the owning page or immutable blob before changing this document.
