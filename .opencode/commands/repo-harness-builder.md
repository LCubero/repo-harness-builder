---
description: Run the repository harness builder
---

Load the `repo-harness-builder` skill by name through the runtime's skill mechanism and follow it exactly. Do not locate it by path.

Arguments are optional. When `$ARGUMENTS` names a report language (`español` or `english`), it is binding only for builder output and the strict builder apply domain. Resolve the conversation language and documentation languages independently through the skill interview, then pass all three values explicitly to preview and apply. Route generated help and documentation entry points to Spanish only when Spanish documentation is declared and the conversation language is Spanish; otherwise route to the English documents the render creates. Never advertise an entry point blocked by the plan. Language-specific reporting must not change the action-plan digest. Arguments may also name a target repository and constraints. With no arguments, check the current working directory read-only and report what is absent. If reciprocal README selector integration is blocked, report it and do not apply any partial bilingual state.
