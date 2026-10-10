# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

**English** | [Español](./README.es.md)

<!-- README-I18N:END -->

`repo-harness` is a repository-scoped set of prompts, rules, documentation, skills, and commands for AI coding agents such as Claude Code, OpenCode, and Pi. It keeps project guidance and handoffs in the repository, where maintainers can inspect them. `repo-harness-builder` creates, audits, and safely evolves that harness.

## What It Generates
<!-- doc-role:outputs -->

- A concise `REPO-HARNESS.md` rulebook and a README when one is missing.
- Project documentation for usage, current state, decisions, and completed changes.
- Session and documentation skills, with commands for your chosen runtimes.
- A checker for keeping runtime copies consistent, plus repository file settings.

The harness works on its own. Integrated mode composes available Gentle AI and Engram capabilities independently; CodeGraph is optional. See the [builder guide](docs/repo-harness/builder-guide.md) for integration boundaries and the complete list of generated files.

## Install
<!-- doc-role:quick-start -->

1. Download and extract the zip of the [latest release](https://github.com/LCubero/repo-harness-builder/releases).
2. Copy the skill folder (`skill/repo-harness-builder/`) and command file (`command/repo-harness-builder.md`) into the repository where you will use them, using the destinations below. Keep both inside that repository, never in a global or user-level configuration folder.

   | Runtime | Skill folder | Command file |
   |---|---|---|
   | Claude Code | `.claude/skills/repo-harness-builder/` | `.claude/commands/repo-harness-builder.md` |
   | OpenCode | `.opencode/skills/repo-harness-builder/` | `.opencode/commands/repo-harness-builder.md` |
   | Pi | `.pi/skills/repo-harness-builder/` | `.pi/prompts/repo-harness-builder.md` |

3. Open that repository in your runtime, run `/repo-harness-builder`, and follow its steps.

Requires Python 3.10+ and your runtime already installed; if a destination already exists, inspect it before copying.

## Daily Use
<!-- doc-role:skills -->

Use `/repo-harness-builder` to create, audit, or update the harness. Once installed, use `repo-harness-session-start` to begin or resume work, `repo-harness-documentation` when durable project facts change, and `repo-harness-session-stop` to pause or finish with a handoff. Invoke these skills through your runtime's skill mechanism. The [usage guide](docs/repo-harness/usage.md) explains the daily workflow and approval boundaries.

## Learn More
<!-- doc-role:license -->

- [Builder guide](docs/repo-harness/builder-guide.md): generated files, language choices, safe upgrades, and development.
- [Usage guide](docs/repo-harness/usage.md): working with the installed harness.
- [CONTRIBUTING.md](CONTRIBUTING.md): contributing to this project.
- [Changelog](CHANGELOG.md): completed changes.
- [MIT License](LICENSE).
