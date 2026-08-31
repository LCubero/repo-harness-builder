# RH-W-0082 Plain-Language Explanation Style Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Decision

RH-W-0082 owns the independent `explanation-style: standard | plain-language` axis. `plain-language` uses neutral simple language without changing evidence, authority, technical truth, artifact language, or the other preference axes. A regional dialect is used only when the human explicitly requests it for the conversation.

This axis composes with RH-W-0071 `response-structure: standard | adhd-friendly` and RH-W-0018 `interaction-behavior: interactive | low-question`. RH-W-0067 remains the complete response-contract owner, RH-W-0031 retains language provenance, RH-W-0046 retains consent boundaries, and RH-W-0051 retains local-state containment.

## Shared Preference Contract

The approved shared LLM-first contract is `.repo-harness/state/preferences/user-preferences.md`. RH-W-0071 owns its complete persistence and runtime-resolution design. This unit owns only explanation-style semantics and must not create another file, registry, parser framework, response owner, consent store, or lifecycle store.

`plain-language` never removes necessary technical terms, evidence classes, contradictions, safety context, authority boundaries, or mandatory gates. It explains them with direct words, short sentences, and definitions where needed. It is not simplification by omission.

## Lifecycle And Non-Authority

RH-W-0082 is `proposed`. This design records the owner's decision and the verified next immutable ID only. It authorizes no product source, tests, generated projection, runtime reader, state file creation, migration, Git action, network action, lifecycle advancement, or implementation.

## Done Conditions

- The explanation-style axis remains independent and combinable.
- `plain-language` remains neutral, simple, accurate, and non-regional by default.
- Artifact language remains independent.
- Existing owners keep their boundaries.
- Implementation requires a reconciled runtime transport and separate explicit authorization.
