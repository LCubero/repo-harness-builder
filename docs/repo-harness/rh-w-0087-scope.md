# RH-W-0087 Proposed Scope

**Status: completed.** This owner-approved document retains the historical boundary for the smallest generated-session-stop correction, implemented and verified on 2026-09-04. The scope-writing and implementation phases did not themselves authorize or deliver Git changes; the [roadmap record](roadmap.md#rh-w-0087) owns the exact later delivery evidence.

## Exact Generated Contract

Generated harness behavior must never propose, append, or create any `Co-Authored-By` trailer, regardless of the named identity. It must also never add an equivalent AI-authorship or AI-assistance attribution in a commit subject, body, or trailer. Claude, OpenAI, ChatGPT, Gemini, and any AI are examples, not an exhaustive provider list or an allowlist or denylist.

The future concise imperative belongs in generated session-stop immediately before the existing `Then create the commit exactly once` action. It must not be duplicated in `REPO-HARNESS.md`, a rule file, or another policy owner.

Human-authored commit metadata remains outside this unit. However, if the generated workflow is asked to create a commit whose approved message contains a prohibited `Co-Authored-By` trailer or AI attribution, it must pause without creating the commit. It must report the conflict and obtain a new human decision; it must not silently rewrite the approved message.

## Focused RED And Synchronization

Implementation must begin with one focused RED that is run before any product change and fails because the rendered generated session-stop contract lacks the obligation. The focused contract assertions must then prove:

1. the generated contract carries the prohibition exactly once and with the meaning defined above;
2. the prohibition appears immediately before, and in the same commit-creation paragraph as, `Then create the commit exactly once`;
3. the exact staged-scope check, conversational approval boundary, one-commit boundary, and invalidation of consent when the approved commit message changes remain intact and in their existing order; and
4. the canonical generated output and every supported Claude Code, OpenCode, and Pi runtime projection are synchronized through the existing renderer and checker ownership model.

The RED and later GREEN prove the generated instruction contract and projection parity. They do not authorize a live-model evaluator, AI detector, commit-message classifier, or general Git trailer parser.

## Boundaries

- No history scan or rewrite and no inspection of unrelated commits.
- No general Git trailer parser or framework, binary, schema change, manifest or checker state, provider-specific list, AI detection or classification, or commit-hook installation.
- No duplicate rule in the rulebook, a rule file, another skill section, or another policy owner.
- No mutation of human-authored metadata. This unit governs only whether the generated workflow may create the currently requested commit; a conflicting approved message causes a pause without rewriting it.
- No weakening or reordering of exact staged-scope validation, approval, one-commit execution, post-commit status refresh, push separation, or consent invalidation when path scope or commit message changes.
- No product source, product test, generated skill, runtime projection, configuration, Git index, commit, remote, issue, pull request, release, or publication change under this scope-writing unit.
- Do not implement Plan 31 concurrently with Plan 18; `RH-W-0068` retains all other generated session-stop closure and Git-route behavior.

## Evidence And Lifecycle

Verified repository fact: the generated session-stop commit-creation paragraph performs the final staged-scope check and then says `Then create the commit exactly once`; it contains no AI-attribution prohibition. Verified completed evidence from `RH-W-0084`: instruction distance from the action point, rather than emphasis, predicts whether an obligation is applied. Human decision: generated harness commits must obey the exact prohibition and pause boundary defined here.

The owner approved this bilingual scope and separately authorized its one bounded test-first implementation on 2026-09-04. The focused RED failed only because the generated obligation was absent; focused GREEN and the related nine-test regression passed, all runtime projections are current with zero checker writes, and `RH-W-0087` is `completed`. Those historical phases did not themselves authorize or deliver Git changes; exact subsequent delivery evidence is maintained in the [roadmap record](roadmap.md#rh-w-0087).
