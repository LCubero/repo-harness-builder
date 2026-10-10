# RH-W-0082 Plain-Language Explanation Style Design

> **Status: `completed`. Implemented on 2026-10-10 with RH-W-0071/RH-W-0082; the [roadmap](roadmap.md) owns lifecycle and evidence.**

## Decision

RH-W-0082 owns the independent `explanation-style: standard | plain-language` axis. `plain-language` uses neutral simple language without changing evidence, authority, technical truth, artifact language, or the other preference axes. A regional dialect is used only when the human explicitly requests it for the conversation.

This axis composes with RH-W-0071 `response-structure: standard | adhd-friendly` and RH-W-0018 `interaction-behavior: interactive | low-question`. RH-W-0067 remains the complete response-contract owner, RH-W-0031 retains language provenance, RH-W-0046 retains consent boundaries, and RH-W-0051 retains local-state containment.

## Shared Preference Contract

The [2026-10-10 preference-driven response modes contract](rh-w-0071-design.md#preference-driven-response-modes-2026-10-10) owns the shared JSON schema, existing one-read transport, manual writing boundary, precedence, modules, failure and single-unit delivery. This unit owns explanation-style semantics only. Manifest/checker installation bytes and fetch-consent approval remain separate; RH-W-0018 stays excluded and deferred.

`plain-language` never removes necessary technical terms, evidence classes, contradictions, safety context, authority boundaries, or mandatory gates. It explains them with direct words, short sentences, and definitions where needed. It is not simplification by omission.

## Lifecycle And Non-Authority

RH-W-0082 is `completed` on 2026-10-10. The shared reader, one-read startup transport, base routing and plain-language module are implemented with RH-W-0071. Contract tests verify generated instructions, not live-model compliance. RH-W-0018 stays deferred.

## Done Conditions

- The explanation-style axis remains independent and combinable.
- `plain-language` remains neutral, simple, accurate, and non-regional by default.
- Artifact language remains independent.
- Existing owners keep their boundaries.
- The linked shared contract is implemented test-first with RH-W-0071.
