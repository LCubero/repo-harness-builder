---
description: Run the repository harness builder
---

Load the `repo-harness-builder` skill by name through the runtime's skill mechanism and follow it exactly. Do not locate it by path.

Arguments are optional. When `$ARGUMENTS` names a report language (`español` or `english`), that language is binding for every user-facing word. It may also name a target repository and constraints. With no arguments, check the current working directory read-only and report what is absent.
