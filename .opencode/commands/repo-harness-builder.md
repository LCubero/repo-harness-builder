---
description: Run the repository harness builder
---

Load the `repo-harness-builder` skill by name through the runtime's skill mechanism and follow it exactly. Do not locate it by path.

Arguments are optional. When `$ARGUMENTS` names a report language (`español` or `english`), it is binding only for builder output and the strict builder apply domain. Resolve conversation language first, stop, then immediately ask for documentation languages in the next turn without waiting for filler input. Pass all three values explicitly to preview and apply. Human documentation may be English-only, Spanish-only at canonical paths, or bilingual with English owners and Spanish peers. Technical artifacts stay in English. Never advertise an entry point blocked by the plan. Language-specific reporting must not change the action-plan digest. Arguments may also name a target repository and constraints. With no arguments, use the current working directory and follow the skill's installation, update, or correction flow; it audits read-only and writes nothing before plan approval. Take the read-only inspection branch only when the user explicitly requests inspect/check.
