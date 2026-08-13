---
description: Run the repository harness builder
---

Load the `repo-harness-builder` skill by name through the runtime's skill mechanism and follow it exactly. Do not locate it by path.

Arguments are optional. When `$ARGUMENTS` names a report language (`español` or `english`), that language is binding for every user-facing word, documentation entry point, generated runtime help route, and strict builder apply domain. English routes to `README.md` and `docs/repo-harness/usage.md`; Spanish routes to `README.es.md` and `docs/es/repo-harness/usage.md`. Pass the resolved language explicitly to preview and apply. Language-specific help must not change the action-plan digest. It may also name a target repository and constraints. With no arguments, check the current working directory read-only and report what is absent. If reciprocal README selector integration is blocked, report it and do not apply any partial bilingual state.
