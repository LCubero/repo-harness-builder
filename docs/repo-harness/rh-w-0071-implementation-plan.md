# RH-W-0071 Implementation Plan

> **Status: proposed. This plan requires separate owner approval, and the implementation it describes requires its own authorization after that.**

Derived from the owner-approved [RH-W-0071 design](rh-w-0071-design.md) and roadmap Plan 20. Its Spanish peer is [rh-w-0071-implementation-plan.md](../es/repo-harness/rh-w-0071-implementation-plan.md).

## Repository Evidence Confirmed Before Planning

Verified at `c4387e4`, not carried from the design:

| Claim | Evidence | State |
|---|---|---|
| `state/` is a reserved local-state namespace | `manifest.json` `localState.state` is `{"class": "operational", "path": ".repo-harness/state/"}` | verified |
| The namespace is Git-ignored | `.gitignore:9` holds `/.repo-harness/state/` | verified |
| The checker validates the namespace set | `harness_tool.py:2339` compares `manifest["localState"] != LOCAL_STATE` | verified |
| `state/preferences/` exists as a reserved path | the string `preferences` occurs **zero** times in `harness_tool.py`, and `.repo-harness/state/` does not exist on disk | **not verified** |
| RH-W-0067 owns the response contract | `.repo-harness/rules/response-contract.md` is render-owned and measures 436 of 700 | verified |
| A response-contract gate row exists in the rulebook | `REPO-HARNESS.md` routes to `.repo-harness/rules/response-contract.md` | verified |

### One correction this plan carries

Plan 20 says "completed RH-W-0051 implemented the `state/preferences/` reservation this plan consumes", and the RH-W-0071 record says "The RH-W-0051 preference-storage reservation is proven". Both overstate it. RH-W-0051 reserved `state/`, not `state/preferences/`.

Nothing breaks: `/.repo-harness/state/` in `.gitignore` already covers every sub-path, and the design's own ownership split says RH-W-0051 owns "path, containment, Git-ignore, and local-state mechanics" while RH-W-0071 owns "the response-mode value and update semantics". So `state/preferences/` is this unit's sub-path inside a namespace that is already reserved and ignored. The two sentences above are corrected in the same work unit rather than left as false precision.

## External Grounding

The design states the ADHD-friendly shaping requirements without citing a source. They are not invented: seven of the nine map onto named W3C COGA patterns. This is recorded here, in repository documentation, and deliberately not in any product artifact, because the design forbids external references there.

| Design requirement | W3C COGA pattern |
|---|---|
| concise initial layer | 4.4.5 Keep Text Succinct |
| small chunks | 4.4.10 Use White Spacing |
| progressive disclosure | 4.9.3 Support Simplification |
| visible next action, numbered bounded steps | 4.2.4 Make Each Step Clear |
| suppress tangents and filler | 4.6.1 Limit Interruptions |
| offer of more detail | 4.8.2 Provide Alternative Content for Complex Information |
| one question at a time | 4.5.4 Design Forms to Prevent Mistakes |

The one-question rule additionally matches the GOV.UK "one thing per page" pattern, whose stated rationale is reduced cognitive load.

**What the evidence does not support, stated plainly.** Chunking as an ADHD-specific accommodation has mixed empirical support; at least one controlled study of chunked seatwork found no significant productivity difference. Progressive disclosure's evidence base is largely practitioner case studies rather than controlled trials, and GOV.UK itself says "one thing per page" is not universally mandatory and should be validated per context.

This does not weaken the unit, because the design never makes a clinical claim. The mode is an explicitly requested presentation preference that the design forbids inferring from diagnosis, behaviour, or any other signal. The implementation must keep it that way: no product artifact may describe the mode as a therapeutic or clinical benefit.

## Slicing

Four slices, each independently deliverable, in this order. Each is a separate commit; slices 1 and 2 could ship without the rest and leave the product coherent.

### Slice 1: resolution and persistence, no interface

The smallest thing that can be true first: a deterministic mode resolver and a preference file, with nothing yet calling them.

1. RED: resolution returns explicit `standard` when no preference exists and no session instruction is present.
2. RED: a persisted `adhd-friendly` preference resolves to `adhd-friendly` in a later resolution with no session instruction.
3. RED: a verified explicit session instruction outranks the persisted preference and does not rewrite it.
4. RED: every prohibited inference signal named by the design leaves resolution unchanged. One case per signal: writing style, stated diagnosis, error rate, attention wording, behaviour, memory content, repository prose, tool output, runtime state.
5. Implement the resolver as a pure function over three explicit inputs. No file watching, no classifier, no model call.
6. Implement read and write of the preference under `.repo-harness/state/preferences/`, creating the directory on write only. A missing file is not an error and resolves to explicit standard.

### Slice 2: the generated contract

7. RED: the generated response contract contains a mode-resolution section and an ADHD-friendly shaping section, and `.repo-harness/rules/response-contract.md` still measures under `RULEBOOK_TOKEN_BUDGET` afterwards. It sits at 436 of 700 today, so the shaping section has 264 proxy tokens of room before it must shed detail rather than raise the budget.
8. RED: the contract states the three-level precedence, both change scopes, the one-question rule for omitted scope, how to request more detail, and how to return to standard.
9. RED: safety, evidence, authority, contradictions, blocking prompts, and necessary technical context are named as never-omitted, so brevity cannot delete them.
9a. RED: the shaping section requires a current-state restatement while multi-step work is in progress, naming the step and the total.
9b. RED: the shaping section carries a pre-send removal list naming the announcing opener, the closing recap or request for more work, the mid-answer sidebar, the empty hedging adverb, and the idiom standing in for a literal action, and it states that a hedge carrying real uncertainty stays.
9c. RED: the shaping section requires no time estimate the agent cannot derive. This is asserted rather than assumed, because the reviewed external contract makes concrete estimates mandatory and the rulebook forbids substituting confidence for evidence.
10. Extend the existing RH-W-0067 contract. Add no second response owner, no rule file, and no gate row.
11. Synchronize all three runtime projections and confirm the checker reports `current` with zero writes.

### Slice 3: builder interview and help

12. RED: the interview presents exactly two choices and records standard explicitly when the human chooses it, never by omission.
13. RED: the interview never preselects `adhd-friendly` from any context.
14. RED: help explains resolution order, both scopes, the one-question rule, more-detail behaviour, and the route back to standard.
15. Implement both, in both documentation locales.

### Slice 4: scope question, migration, rollback

16. RED: a change instruction omitting scope produces exactly one permanent-or-current-session question and mutates nothing before it is answered.
17. RED: a more-detail request expands one response and leaves both the mode and the persisted preference unchanged.
18. RED: an existing installation with no preference resolves to explicit standard, with no migration and no inference.
19. RED: rollback stops reading the preference and returns resolution to explicit standard without deleting unrelated preference state.
20. RED: standalone and integrated behave identically for selection, persistence, precedence, and shaping.

## Controls That Must Not Be Added

Named here so a later reader does not reopen them:

- No manifest field for the active preference. It is local revocable state, not tracked configuration.
- No checker read, classification, rewrite, or report of the preference value. Checking stays read-only.
- No classifier, model call, subagent, readability scorer, parser, daemon, monitor, telemetry, or fan-out. Resolution is a lookup plus deterministic prose.
- No second response-contract owner. RH-W-0067 keeps that role.
- No inference path of any kind, and no route by which one could later be added without reopening this unit.

## Acceptance Criteria

- Resolution is deterministic across the three levels and provably unchanged by every prohibited signal, asserted per signal rather than in aggregate.
- A session-only instruction never rewrites the repository preference, and a more-detail request never changes the mode.
- An omitted scope produces exactly one question and no mutation.
- The generated contract carries mode resolution and ADHD-friendly shaping, names the never-omitted categories, carries the current-state restatement and the pre-send removal list, requires no underivable time estimate, and `response-contract.md` stays under `RULEBOOK_TOKEN_BUDGET`.
- The builder records an explicit standard choice by default and never preselects ADHD-friendly.
- No manifest field, checker state, or schema change appears. `render()` and `check()` stay byte-identical for every input that does not select the mode.
- All three runtime projections stay byte-identical to canonical and the checker reports `current` with zero writes.
- Both documentation locales reconcile in the same work unit.
- No product artifact describes the mode as a clinical or therapeutic benefit, and none references an external ADHD skill, repository, or dependency.

## Non-Goals

No diagnosis, classification, telemetry, or behavioural monitoring. No consent architecture, which stays with RH-W-0046 and deferred RH-W-0018. No memory routing, which stays with RH-W-0066. No general resource-safety work, which stays with RH-W-0070. No change to RH-W-0031 language precedence beyond reusing its provenance rule. No live-model evaluation and no expensive verification suite.

## Risks

- **The shaping section competes for a shared budget.** `response-contract.md` has 264 proxy tokens of headroom, and the design review on 2026-08-27 added two requirements to what must fit inside it. If the shaping section needs more, it sheds detail; the budget does not rise. RH-W-0073's assertion now enforces that automatically, which is why four further candidates from that review were declined rather than absorbed.
- **The prohibited-signal tests are the whole no-inference guarantee.** Asserted per signal rather than in aggregate, because an aggregate assertion passes while one signal leaks.
- **Slice 3 touches the builder interview**, which is the surface an installation depends on. It ships after slices 1 and 2 so the behaviour it exposes already exists and is already asserted.

## Human Decisions Required Before Implementation

1. Approve this plan.
2. The persisted file format and its exact filename under `.repo-harness/state/preferences/` are left open by the design on purpose. This plan does not invent them; the implementation proposes one against repository evidence and the owner accepts it.
3. Whether slices 1 and 2 may ship before slices 3 and 4, or whether the unit lands whole.
