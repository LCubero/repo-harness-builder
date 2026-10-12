# Bundled Gentle AI Skill Rules

Revision used: repository `skill-style-guide.md`, read on 2026-08-10.

Use frontmatter with `name`, a one-line quoted `description`, `license`, `metadata.author`, and `metadata.version`. Keep the body in this order: Activation Contract, Hard Rules, Decision Gates, Execution Steps, Output Contract, References. Target 180 to 450 body tokens, recommended maximum 700, hard maximum 1000. This ceiling binds both this builder skill and every skill it generates; never raise 1000/700 or add runtime refusal based on size. Write imperative runtime instructions. Put conceptual detail in `references/` and templates or generated examples in `assets/`. Use local references. Do not add a Keywords section.

The guide's trigger-first `description` rule serves automatic discovery. This builder is invoked deliberately as a skill; generated consumer skills retain their own command entry points. Both state what they do and omit the `Trigger:` prefix on purpose. Declaring an automatic discovery trigger would advertise an activation path that is not intended.

The local upgrade protocol and its verified style guide are normative when they differ from this bundled summary. Do not claim an absent repository guide is available.

## Stable Prefixes

Repository-owned prompt-composition rule: put imperative stable instructions and shared references first, then changing session or task data, wherever this repository controls the actual composition and the provider supports prefix caching. Avoid putting changing timestamps, session IDs, or status before the stable reusable prefix; data that must stay fresh remains present and current. A date or ID fixed for one session may still permit reuse within that session.

Preserve role authority, precedence, ordering dependencies, and security: do not move instructions to a lower-trust role or omit required content to optimize caching. Providers can reuse identical rendered prefixes; a change limits reuse from that point onward, not all cache. Tool definitions, their order, and settings may also affect reuse. Caching neither shrinks context nor guarantees savings; measure cached-input/usage before claiming savings. This is distinct from the registered-skill freshness cache. Example: stable policy and shared references → dynamic session/task data.

Contextual source: [OpenAI prompt caching](https://platform.openai.com/docs/guides/prompt-caching); this guidance is provider-neutral, not a provider requirement. This documentation does not add a cache subsystem or configuration, enforce runtime composition, or prove savings.
