# repo-harness-builder
<!-- doc-role:repository-harness -->

<!-- README-I18N:START -->

**English** | [Español](./README.es.md)

<!-- README-I18N:END -->

`repo-harness` is a repository-scoped set of prompts, rules, documentation, skills, and commands for AI coding agents such as Claude Code, OpenCode, and Pi. It keeps project guidance and session notes in the repository, where maintainers can inspect them.

**The builder installs and updates; the harness supports daily work.** Neither requires Git or a remote: private, local-only projects work too.

## What It Generates
<!-- doc-role:outputs -->

- A concise `REPO-HARNESS.md` rulebook and a README when one is missing.
- Project documentation for usage, current state, decisions, and completed changes.
- Session and documentation skills, with commands for your chosen runtimes.
- A checker for keeping runtime copies consistent, plus repository file settings.

The harness works on its own. Integrated mode composes available Gentle AI and Engram capabilities independently; CodeGraph is optional. See the [builder guide](docs/repo-harness/builder-guide.md) for integration boundaries and the complete list of generated files.

## What The Harness Does
<!-- doc-role:features -->

- **Picks up where you left off:** each session starts from repository evidence and the last local handoff.
- **Keeps documentation current:** one home per fact, with current state, decisions, and changelog kept in sync.
- **Asks before acting:** writes, commits, pushes, and network access need your approval; Git stays optional.
- **Says how sure it is:** claims are labeled as verified, inferred, unknown, or needing your decision.
- **Speaks your language:** English or Spanish replies, with English, Spanish, or bilingual documentation.
- **Updates safely:** the builder audits an existing harness and keeps your own edits.

## Install
<!-- doc-role:quick-start -->

1. Download the zip labeled **pi**, **claude-code**, or **opencode** for your runtime from the [stable v3.1 release](https://github.com/LCubero/repo-harness-builder/releases/tag/v3.1).
2. Extract it at the root of the repository where you will use it. It creates your runtime's skills folder listed below (`skills` plural). Keep it inside the repository, never in a global or user-level configuration folder.

   - **Claude Code:** `.claude/skills/repo-harness-builder/`
     Invoke `/repo-harness-builder`.
   - **OpenCode:** `.opencode/skills/repo-harness-builder/`
     Ask the agent to use the `repo-harness-builder` skill.
   - **Pi:** `.pi/skills/repo-harness-builder/`
     Invoke `/skill:repo-harness-builder`.

3. Open that repository in your runtime and invoke the skill once to set up the harness. Answer its questions and approve the plan before it makes changes.

Requires Python 3.10+ and your runtime already installed; if a destination already exists, inspect it before extracting or overwriting.

## Daily Use
<!-- doc-role:skills -->

1. Start or resume with `repo-harness-session-start`.
2. Work normally: ask the agent to do your task.
3. When important project facts change, use `repo-harness-documentation` to keep project docs current.
4. When pausing or finishing, use `repo-harness-session-stop` to leave context for the next session.

Ask your agent to use each skill when needed. Session notes stay local under `.repo-harness/memory-bk/`; optional Engram memory never overrides repository files. See the [usage guide](docs/repo-harness/usage.md) for details.

## Response Preferences
<!-- doc-role:response-preferences -->

You choose how the agent answers:

- **Language:** English or Spanish.
- **ADHD-friendly structure:** answer first, short chunks, one question at a time.
- **Plain language:** simple words, short sentences, technical terms defined.

Ask for a mode during a session, or ask the agent to save it for future sessions. It never guesses or saves preferences on its own. See [response preferences](docs/repo-harness/usage.md#response-preferences).

## What You Can Ask
<!-- doc-role:requests -->

Ask in your own words; no exact phrase is needed.

- **Time zone:** say which one to use (for example, `America/Costa_Rica`). Changelog dates, backups, and reports use it; otherwise local computer time, never UTC. To keep it for future sessions, ask to record it as a decision.
- **Language and reply style:** change them for this session, or ask to save them.
- **More detail:** ask for it at any time; your reply style stays the same.
- **Batch mode:** name one bounded task and ask for batch mode. The agent groups its open questions once, then works without asking per file. Approvals are still asked one at a time.
- **Safe sync:** ask to sync safely. The agent fetches and updates only when no merge or conflict is needed.
- **Commit or push:** ask for a commit, a push, or both when you stop. The agent shows the exact files and message first.
- **Documentation update:** ask to update the project docs after important changes.
- **Pause or finish:** ask to pause or close the session; the next session resumes from that handoff.
- **Help:** run `repo-harness-help` for a short guide.
- **Audit or update the harness:** run the builder again.
- **Remove the harness:** ask for the [manual removal steps](docs/repo-harness/usage.md#removing-the-harness-manually); there is no automatic uninstaller.

## Learn More
<!-- doc-role:license -->

You can run the builder again later for an optional audit or update; see the builder guide below.

- [Agent installation entry](llms.txt): technical instructions for agents helping install the harness.
- [Builder guide](docs/repo-harness/builder-guide.md): generated files, language choices, safe upgrades, and development.
- [Usage guide](docs/repo-harness/usage.md): working with the installed harness.
- [CONTRIBUTING.md](CONTRIBUTING.md): contributing to this project.
- [Changelog](CHANGELOG.md): completed changes.
- [MIT License](LICENSE).

<a href="https://github.com/Gentleman-Programming/gentle-ai">
  <img width="220" src="https://raw.githubusercontent.com/Gentleman-Programming/gentle-ai/main/docs/assets/brand/built-with-gentle-ai.png" alt="Built with Gentle-AI" />
</a>
