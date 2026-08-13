# Bundled Gentle AI Skill Rules

Revision used: repository `skill-style-guide.md`, read on 2026-08-10.

Use frontmatter with `name`, a one-line quoted `description`, `license`, `metadata.author`, and `metadata.version`. Keep the body in this order: Activation Contract, Hard Rules, Decision Gates, Execution Steps, Output Contract, References. Target 180 to 450 body tokens, recommended maximum 700, hard maximum 1000. Write imperative runtime instructions. Put conceptual detail in `references/` and templates or generated examples in `assets/`. Use local references. Do not add a Keywords section.

The guide's trigger-first `description` rule serves automatic discovery. This skill and everything it generates are invoked deliberately through their own command, so they state what they do and omit the `Trigger:` prefix on purpose. Declaring a discovery trigger for a command-only skill would advertise an activation path that is not intended.

The local upgrade protocol and its verified style guide are normative when they differ from this bundled summary. Do not claim an absent repository guide is available.
