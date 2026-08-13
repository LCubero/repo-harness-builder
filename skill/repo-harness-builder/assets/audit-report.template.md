<!--
Report template.
Replace every {{t "..."}} with that text written in the report language.
Replace every other {{...}} with the observed value.
Never translate: commands, paths, identifiers, enum values, states, and anything inside backticks.
-->

# {{t "Repository harness audit"}}

- {{t "Selected repository"}}: `{{selected_root}}`
- {{t "State"}}: `{{state}}`
- {{t "Runtime profiles inspected"}}: {{runtimes_inspected}}
- {{t "Runtime set"}}: {{runtime_set_or_open}}
- {{t "Absent and eligible to create"}}: {{absent}}
- {{t "Already present and left untouched"}}: {{present}}
- {{t "Evidence read"}}: {{evidence}}
- {{t "Recovery condition"}}: {{recovery_or_none}}

{{t "One applicable planning decision, or nothing when there is no decision for the human to make. When the runtime set is open, that same decision resolves it."}}
