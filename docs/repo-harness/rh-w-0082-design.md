# RH-W-0082 Plain-Language Explanation Style Design

> **Status: OWNER-APPROVED DESIGN. Implementation is separately unauthorized.**

## Decision

RH-W-0082 owns the independent `explanation-style: standard | plain-language` axis. `plain-language` uses neutral simple language without changing evidence, authority, technical truth, artifact language, or the other preference axes. A regional dialect is used only when the human explicitly requests it for the conversation.

This axis composes with RH-W-0071 `response-structure: standard | adhd-friendly` and RH-W-0018 `interaction-behavior: interactive | low-question`. RH-W-0067 remains the complete response-contract owner, RH-W-0031 retains language provenance, RH-W-0046 retains consent boundaries, and RH-W-0051 retains local-state containment.

## Shared Preference Contract

The owner-delegated shared-file decision supersedes the unimplemented structured Markdown proposal: one small ignored `.repo-harness/state/preferences/user-preferences.json` initially holds only explicit conversation language. RH-W-0082 explanation style remains deferred; adding it to that same file requires separate lifecycle authority, without predefining its unused field. RH-W-0071 retains shared-file design ownership; the actual runtime transport/API remains unresolved. This unit owns only explanation-style semantics and must not create another file, registry, parser framework, response owner, consent store, or lifecycle store. Manifest/checker installation bytes and fetch-consent approval remain separate.

`plain-language` never removes necessary technical terms, evidence classes, contradictions, safety context, authority boundaries, or mandatory gates. It explains them with direct words, short sentences, and definitions where needed. It is not simplification by omission.

## Lifecycle And Non-Authority

RH-W-0082 is `proposed`. This design records the owner's decision and the verified next immutable ID only. It authorizes no product source, tests, generated projection, runtime reader, state file creation, migration, Git action, network action, lifecycle advancement, or implementation.

## Done Conditions

- The explanation-style axis remains independent and combinable.
- `plain-language` remains neutral, simple, accurate, and non-regional by default.
- Artifact language remains independent.
- Existing owners keep their boundaries.
- Implementation requires a reconciled runtime transport and separate explicit authorization.
