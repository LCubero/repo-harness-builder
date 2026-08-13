# Bilingual Documentation Architecture Research

## Decision

Use `README.md` as the English GitHub default, `README.es.md` as its Spanish peer, English durable docs under `docs/repo-harness/`, and Spanish peers under `docs/es/repo-harness/`. Root READMEs use one marked reciprocal selector.

## Normative Sources

- [GitHub, About READMEs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes): GitHub surfaces a root README automatically, supporting English as the default entry point.
- [GitHub, Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/basic-writing-and-formatting-syntax): relative links remain portable across branches and clones.
- [GitHub, Working with non-code files](https://docs.github.com/en/repositories/working-with-files/using-files/working-with-non-code-files): Markdown is rendered as prose and headings are linkable.
- Installed `readme-i18n` references: the adopted project workflow for sibling `README.<locale>.md` naming, reciprocal marked selectors, prose-only translation, and relative-link validation. This is normative for this repository because the project explicitly adopted it, not because it is a platform standard.

## Non-Normative Evidence

- [freeCodeCamp repository README](https://github.com/freeCodeCamp/freeCodeCamp/blob/main/README.md): mature repository precedent for a concise root product entry point linking to deeper documentation. It is an example, not an authority or universal convention.

## Tradeoffs

File suffixes for every durable document keep peers nearby but make navigation noisy and scale poorly. A locale tree gives Spanish readers one predictable namespace, keeps English links stable, and makes generated ownership explicit. The cost is deeper relative links, addressed by automated link validation.

## Ownership

`REPO-HARNESS.md` and `CHANGELOG.md` remain canonical operational owners. `REPO-HARNESS.es.md` and `CHANGELOG.es.md` are complete localized presentations that link to the canonical files. English current state and decisions own facts; Spanish peers must be synchronized in the same work unit but may use natural locale-specific phrasing.

## Maintenance Rules

1. Update the canonical factual owner first.
2. Update the Spanish peer in the same work unit.
3. Preserve commands, identifiers, paths, machine tokens, code, and links unless the destination is intentionally localized.
4. Validate semantic section roles rather than literal translated headings.
5. Validate all relative links from their containing files.
6. Never overwrite human-evolved localized docs in an existing repository; analyze and propose integration.
