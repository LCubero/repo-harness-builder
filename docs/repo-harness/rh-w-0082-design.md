# RH-W-0082 Plain-Language Explanation Style Design

> **Status: active. Owner-delegated design of 2026-10-10 groups delivery with RH-W-0071; implementation is a separate later unit.**

## Decision

RH-W-0082 owns the independent `explanation-style: standard | plain-language` axis. `plain-language` uses neutral simple language without changing evidence, authority, technical truth, artifact language, or the other preference axes. A regional dialect is used only when the human explicitly requests it for the conversation.

This axis composes with RH-W-0071 `response-structure: standard | adhd-friendly` and RH-W-0018 `interaction-behavior: interactive | low-question`. RH-W-0067 remains the complete response-contract owner, RH-W-0031 retains language provenance, RH-W-0046 retains consent boundaries, and RH-W-0051 retains local-state containment.

## Shared Preference Contract

The [2026-10-10 preference-driven response modes contract](rh-w-0071-design.md#preference-driven-response-modes-2026-10-10) owns the shared JSON schema, existing one-read transport, manual writing boundary, precedence, modules, failure and single-unit delivery. This unit owns explanation-style semantics only. Manifest/checker installation bytes and fetch-consent approval remain separate; RH-W-0018 stays excluded and deferred.

`plain-language` never removes necessary technical terms, evidence classes, contradictions, safety context, authority boundaries, or mandatory gates. It explains them with direct words, short sentences, and definitions where needed. It is not simplification by omission.

## Lifecycle And Non-Authority

RH-W-0082 is `active` by owner-delegated decision on 2026-10-10. Shared transport and delivery grouping are resolved in the linked RH-W-0071 contract. Next comes separately authorized test-first implementation with RH-W-0071, followed by delivery; this documentation unit changes no product behavior.

## Done Conditions

- The explanation-style axis remains independent and combinable.
- `plain-language` remains neutral, simple, accurate, and non-regional by default.
- Artifact language remains independent.
- Existing owners keep their boundaries.
- Implement the linked shared contract test-first in one separately authorized unit with RH-W-0071, then deliver.
