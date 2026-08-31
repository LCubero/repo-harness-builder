# RH-W-0084 LLM-First Research

## Status and purpose

**Status: research executed on 2026-08-31 under owner-granted execution authority, inside the seven `RH-W-0012` boundaries.** This document records findings and routes them to existing owners. It changes no generated surface, no product source, no test, no manifest, and no checker, and it advances no other unit's lifecycle. Every conclusion it reaches about this repository still needs separate authorization before it becomes a control.

**Evidence classes** follow the `RH-W-0084` [scope](rh-w-0084-scope.md) ladder: class 1 is this repository's own measurements, class 2 is primary documentation from the model, runtime, or standards owner, class 3 is peer-reviewed or standards-body research, and class 4 is secondary material, which is never a repository fact on its own. No class 4 source is cited below.

**Sources read, with date read.** Six sources were located and five were read in full. All reads were HTTP GET of a public page; nothing was cloned, downloaded, or executed, per boundaries 2 and 5.

| Source | Class | Version evidence | Read | Refresh trigger |
| --- | --- | --- | --- | --- |
| [Anthropic, Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | 2 | undated page | 2026-08-31 | The context-rot or attention-budget claims change, or the page starts carrying a version |
| [Anthropic, Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) | 2 | undated page | 2026-08-31 | The longform-data-at-top ordering rule or the 30 percent figure changes, or model-specific guidance supersedes it |
| [OpenAI, Model Spec 2026-08-18](https://model-spec.openai.com/2026-08-18.html) | 2 | version-dated in the URL | 2026-08-31 | A later dated Model Spec changes the authority levels or the untrusted-content rule |
| [OpenAI, Prompt engineering guide](https://developers.openai.com/api/docs/guides/prompt-engineering) | 2 | undated page | 2026-08-31 | The recommended developer-message order changes, or restating instructions becomes recommended |
| [Liu et al., Lost in the Middle, TACL vol. 12 (2024), pp. 157-173](https://aclanthology.org/2024.tacl-1.9/) | 3 | published volume and pages | 2026-08-31 | A later peer-reviewed result overturns positional degradation in current models |
| `docs.anthropic.com/.../long-context-tips` | 2 | none; 301 to a path that did not resolve | located, not read | The redirect resolves, if its content ever diverges from the Prompting best practices page |

The sixth source is reported rather than dropped, as `RH-W-0065` requires. Its content is superseded by the Prompting best practices page, which was read.

## Findings against the six questions

### 1. Format

Both vendors converge on delineated sections rather than undifferentiated prose. Anthropic states that XML tags "help Claude parse complex prompts unambiguously, especially when your prompt mixes instructions, context, examples, and variable inputs", and recommends "organizing prompts into distinct sections (like `<background_information>`, `<instructions>`, `## Tool guidance`, `## Output description`, etc) and using techniques like XML tagging or Markdown headers to delineate these sections". OpenAI likewise says "XML tags can help delineate where one piece of content (like a supporting document used for reference) begins and ends."

Two qualifications matter more than the convergence. Anthropic states that "the exact formatting of prompts is likely becoming less important as models become more capable", which argues against spending a budget on format precision. And it reports a style-transfer effect: "removing markdown from your prompt can reduce the volume of markdown in the output." A contract's format therefore influences the shape of the reply it produces, not only its comprehension.

**Applicability limit:** these are vendor claims about their own models, not measurements on this repository's generated surfaces.

### 2. Size

Anthropic frames size as a budget rather than a ceiling: "you should be striving for the minimal set of information that fully outlines your expected behavior. (Note that minimal does not necessarily mean short; you still need to give the agent sufficient information up front)". The mechanism it names is **context rot**: "as the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases", because attention forms "n² pairwise relationships for n tokens", so models "remain highly capable at longer contexts but may show reduced precision for information retrieval and long-range reasoning". It describes an attention budget where "every new token introduced depletes this budget by some amount".

Two concrete numbers appear. Sub-agent summaries are described as "often 1,000-2,000 tokens". Long-document prompt structuring is triggered at "20k+ tokens".

**Applicability limit:** neither number is a contract-body ceiling, and no source states a maximum size for an agent-facing instruction document. Nothing read supports or refutes a specific numeric cap.

### 3. Path and location

This is the best-evidenced question, and it is the only one with both class 2 and class 3 support.

Liu et al. found that "performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts", concluding that "current language models do not robustly make use of information in long input contexts". Anthropic's operational form of the same effect is directional and quantified: "Put longform data at the top: Place your long documents and inputs near the top of your prompt, above your query, instructions, and examples. This improves performance across all models", with the note that "queries at the end can improve response quality by up to 30 percent in tests, especially with complex, multidocument inputs."

The vendors disagree on the tail. Anthropic puts data first and the query last. OpenAI's recommended developer-message order is Identity, then Instructions, then Examples, then Context, with context near the end because it varies per request. Both are consistent with avoiding the middle; they differ on which end an instruction occupies. **Unknown:** which ordering is better for a repository contract that is loaded as a document rather than composed per request. Nothing read settles it.

### 4. Reinforcement

The evidence is thinner than expected and points one way. OpenAI's guide does not recommend restating instructions; it emphasizes initial structure and a high-level instructions parameter that takes priority. Anthropic addresses reinforcement only for duration, not for importance: "for very long conversations, inject what were previously prefilled-assistant reminders into the user turn", and otherwise "consider hydrating through tools (expose or encourage use of tools containing context based on heuristics such as number of turns) or during context compaction".

**Supported inference:** reinforcement is a remedy for distance between the instruction and the action, not a general emphasis device. Repeating a rule that is already adjacent to its action point buys nothing and spends attention budget.

### 5. Forms and visibility

The strongest result here is about precedence rather than typography. OpenAI's Model Spec defines an explicit ordered chain of command, Root, System, Developer, User, Guideline, and states that "instructions with higher authority override those with lower authority", with "when two root-level principles conflict, the model should default to inaction". It also fixes a trust boundary that a harness inherits directly: "quoted text (plaintext in quotation marks, YAML, JSON, XML, or `untrusted_text` blocks) in ANY message, multimodal data, file attachments, and tool outputs are assumed to contain untrusted data and have no authority by default."

**Applicability limit:** this is one vendor's specification of its own models. It is evidence that explicit ordered precedence is a real mechanism, not evidence that any particular runtime honors this repository's precedence claims.

### 6. Control question

Internal, and answered by the scope rather than by any source. No external evidence was sought for it.

## Routing to existing owners

Per the scope, an accepted conclusion goes to exactly one existing owner, and this document changes none of them.

| Finding | Owner it informs | What it offers, and what it does not |
| --- | --- | --- |
| Instruction distance from the action point, not emphasis, predicts whether an obligation is applied | `RH-W-0083` | Its measured defect, an obligation named in steps that complete before a reply is composed, matches finding 4 exactly. The evidence supports reinforcement at the action point for this specific case. It does not choose between wording, a development-time control, and an accepted limitation, which stays the owner's open question. |
| "Minimal does not necessarily mean short", plus context rot as the mechanism | `RH-W-0076`, `RH-W-0078` | Gives their measured breaches a mechanism instead of an assertion: cost is degraded recall, which scales with total loaded context, not with one file crossing a number. It does **not** supply a defensible replacement number, and nothing read supports 1000, 1200, or any other cap. |
| Beginning and end are privileged positions; the middle is not | `RH-W-0067`, `RH-W-0073` | Argues that where a rule sits in a loaded contract is a real variable. It does not resolve the vendor disagreement about which end an instruction should occupy. |
| Explicit ordered precedence and untrusted-content boundaries are established vendor mechanisms | `RH-W-0079`, `RH-W-0051` | Supports the existing practice of naming precedence and treating tool output as untrusted. It adds no new requirement. |

## The boundary defect this research found

The first real use of the `RH-W-0012` boundaries exercised boundary 4 and found it internally inconsistent. It reads: pin an exact revision when the source offers one, cite the version-dated page when it does not, and "never cite a moving ref such as `main`, `latest`, or an undated page."

Four of the five sources read here are undated vendor documentation pages that offer neither a revision nor a version. Boundary 4 blesses a version-dated fallback and then forbids an undated page, leaving no rule for the most common class of primary source. Taken literally it forbids citing most vendor documentation, which would make the primary-source-first rule of boundary 1 unusable.

The conservative behavior was applied rather than the literal rule: every citation above records its read date, which is what boundaries 6 and 7 already require, and the version-evidence column states plainly which pages carry no version. This is the same shape as the boundary 4 legacy clause, applied forward instead of backward.

**Resolved the same day.** `RH-W-0012` reopened on the condition it had named, and the correction landed after measuring precedent rather than inventing a rule. `rh-w-0031-research.md`, this repository's canonical primary-source evidence owner, already cites undated vendor pages with an exact URL plus a **refresh trigger** naming what change would invalidate the claim, and carries a version inside the link text where the source exposes one. The boundary 4 undated-source clause codifies that, which is stronger than a bare read date: a read date says when someone looked, a refresh trigger says when to look again. The source table above was rewritten to comply, since it did not. `RH-W-0012` returned to `completed`.

## Non-goals honored

No generated surface, product source, test, manifest, checker, or runtime behavior changed. No unit other than this one advanced. No source was cloned, downloaded, or executed. No numeric ceiling was selected for `RH-W-0076` or `RH-W-0078`, because nothing read supports one. No vendor claim is recorded as a repository fact; each carries its class and its applicability limit.
