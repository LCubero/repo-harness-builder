# Stateless Correction Authority

This is preview/apply transport authority, not Gentle AI RDD correction authority, lineage, receipt, review, or delivery authority.

Preview and apply are separate CLI processes with no shared memory; only host-returned bytes can carry an approval. This transport is opt-in. Builder activation alone means only the strict approval domain.

- **Host:** Write canonical authority context: exact compact ASCII JSON, sorted keys, no spaces after separators, and no trailing newline. Declare the material revision, project, exact builder configuration, classified premises, separate human decisions, and typed evidence bindings.
- **Preview:** Read it with `--authority-context-file` and `--project-id`; resolve and re-read every declared evidence item. Emit `authorityContextSha256`, `targetBindingSha256`, `planSha256`, and `generationSha256` with stable identifiers and explicit redaction markers. Emit no premise value, decision statement, raw evidence, or absolute path.
- **Human:** Review bindings and approve the plan.
- **Apply:** Receive the same context plus `--approved-plan-sha256` and `--approved-generation-sha256`. Re-read context, revalidate every binding against current bytes, recompute generation, and refuse before any write if anything differs.

If a premise was wrong, refresh the context with a new material revision and preview again. A corrected identical plan keeps `planSha256` but changes `generationSha256`; the plan digest alone cannot authorize the write.

Every authority failure at apply collapses into one content-free `fresh preview` refusal, because naming the difference could echo evidence the binding protects. Initial preview reports exactly what is wrong with its context.

Under default `--activation maintenance`, all three authority arguments are rejected and maintenance applies on the matching plan digest alone. For canonical JSON, digest definitions, and privacy rules, see `architecture.md` and `preservation-and-safety.md`.
