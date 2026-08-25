# Agent Continuity and Learning Research

## Outcome

Hermes Agent, OpenClaw, and Pi Coding Agent offer useful patterns for bounded continuity, recovery, trust, and extension. None should be copied as a new authority or autonomous self-modification layer. The strongest reusable direction is to keep durable truth in repository owners, use Engram for semantic continuity, use Gentle AI for routing and review when enabled, and add only evidence-backed safeguards that reduce loss or unauthorized action.

This document is research guidance. It does not adopt a feature, create lifecycle work, change product state, or authorize implementation.

## Research Questions

1. How do the three agents preserve sessions and knowledge across context loss?
2. How do they separate memory, skills, runtime state, and authoritative project files?
3. Which controls prevent learning or extension mechanisms from becoming unsafe authority?
4. Which patterns fit this harness, require adaptation, or should be rejected?

## Pinned Revisions

| Project | Reviewed revision |
|---|---|
| Hermes Agent | [`1bbb6e5bce56e721ab685af4cd87df21bbff4d35`](https://github.com/NousResearch/hermes-agent/tree/1bbb6e5bce56e721ab685af4cd87df21bbff4d35) |
| OpenClaw | [`9cfb4ff956c9b50e713f8774b576d9edd8db50a7`](https://github.com/openclaw/openclaw/tree/9cfb4ff956c9b50e713f8774b576d9edd8db50a7) |
| Pi Coding Agent | [`a79b3733421ead0dcea3cbe32247ea2464400dcb`](https://github.com/earendil-works/pi/tree/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent) |

Sources were reviewed on 2026-08-25. Immutable links below are the evidence anchors.

## Comparative Findings

| Concern | Hermes Agent | OpenClaw | Pi Coding Agent | Harness implication |
|---|---|---|---|---|
| Session ownership | SQLite-backed searchable sessions and cross-platform continuity are presented as agent capabilities. | The gateway owns session state and routes messages into isolated or shared sessions. | JSONL session files preserve a tree with `id` and `parentId`. | Keep one explicit owner per continuity layer. Do not let a UI, skill, or summary become a parallel session authority. |
| Context recovery | Compression, session search, memory nudges, and persistent memory address context loss. | A memory flush runs before compaction; durable files and transcripts remain separate. | Compaction is lossy in active context, but full JSONL history and branch structure remain available. | Summaries should be replaceable views over retained evidence, not the only evidence. |
| Durable knowledge | Curated memory, user modeling, and procedural skills form a learning loop. | `USER.md`, `MEMORY.md`, daily notes, and optional dreaming separate profile, durable knowledge, and working observations. | Context files and skills shape behavior, while sessions retain conversation history. | Preserve the existing split: repository for durable team truth, Engram for semantic observations, and session summaries for temporary continuity. |
| Skills and extensions | Skills may be created and improved from experience; plugins and MCP add capabilities. | Skills and plugins extend behavior; memory itself is plugin-owned. | Skills are instructions; TypeScript extensions can intercept lifecycle events and tools. | New capability must remain explicit, inspectable, and subordinate to repository policy. |
| Recovery model | Checkpoints, rollback, undo, and session search are advertised recovery surfaces. | Durable session rows, archived transcripts, migration tooling, and bounded maintenance support recovery. | Tree navigation, forks, clones, branch summaries, and retained pre-compaction history support recovery. | Prefer append-only or reversible evidence before adding autonomous correction. |
| Approval and trust | Command approval and isolated terminal backends are documented security controls. | Effective exec policy is the stricter combination of tool policy and host approvals; approvals bind execution context. | Project-local settings and extensions load only after project trust; extensions run with full permissions. | Approval state must stay with its enforcing owner. Memory may record context but cannot grant authority. |
| Configuration | Provider, tool, gateway, and memory behavior are configurable. | Workspace content, state databases, session routing, memory engines, and approvals have distinct configuration surfaces. | Global and project settings have explicit precedence; project settings are trust-gated. | Keep configuration scope visible and avoid hidden fallback that broadens authority. |
| Verification | Trajectories and skill evolution provide learning material, but the reviewed overview does not establish harness-specific completion proof. | Inspection, audit, doctor, dry-run, and policy commands expose operational evidence. | Session structure, events, extension hooks, and deterministic files are inspectable. | Learning signals are not completion receipts. Tests and canonical repository evidence remain required. |

## Evidence Classification

### Verified Facts

- OpenClaw states that memory is written to plain Markdown, that the model remembers only what is saved, and that memory does not enforce policy. Its pre-compaction memory flush is enabled by default. [Memory overview](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/concepts/memory.md)
- OpenClaw assigns session state to the gateway, separates live SQLite rows from archived transcripts, and supports explicit routing and retention policies. [Session management](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/concepts/session.md)
- OpenClaw host execution requires agreement among policy, allowlist, and optional approval. Its effective policy is the stricter applicable policy, and an approved node run binds command, working directory, environment, and session context. [Exec approvals](https://github.com/openclaw/openclaw/blob/9cfb4ff956c9b50e713f8774b576d9edd8db50a7/docs/tools/exec-approvals.md)
- Pi stores sessions as tree-structured JSONL, retains full history after lossy compaction, and supports branch summaries when navigating the tree. [Coding Agent README](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/README.md) and [compaction](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/docs/compaction.md)
- Pi project trust gates project-local settings, resources, and extensions. Pi warns that packages and extensions can execute with full system access. [Coding Agent README](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/README.md) and [extensions](https://github.com/earendil-works/pi/blob/a79b3733421ead0dcea3cbe32247ea2464400dcb/packages/coding-agent/docs/extensions.md)
- Hermes presents a learning loop combining curated memory, periodic nudges, autonomous skill creation, skill improvement, session search, and user modeling. It also advertises command approval and isolated terminal backends. [Hermes README](https://github.com/NousResearch/hermes-agent/blob/1bbb6e5bce56e721ab685af4cd87df21bbff4d35/README.md)

### Supported Inferences

| Inference | Evidence and reasoning | What would raise confidence |
|---|---|---|
| Pre-loss persistence is safer than relying on a later summary alone. | OpenClaw flushes memory before compaction, while Pi keeps full history behind a lossy active summary. Both preserve evidence outside the compressed prompt. | A focused harness design proving the exact trigger, owner, failure semantics, and tests. |
| Tree or append-only continuity can improve correction without creating a new lifecycle. | Pi preserves branches and full JSONL history; OpenClaw retains archived transcripts separately from current rows. | A repository-approved design that maps this behavior onto existing owners without another state store. |
| Learned skills require stronger promotion gates than ordinary observations. | Hermes allows skills to evolve, while Pi warns that skills and extensions may cause arbitrary actions. | Review of Hermes skill-curation source plus an approved provenance, review, rollback, and test contract. |
| Approval binding should include exact action context, not only a conversational yes. | OpenClaw binds approved execution to canonical arguments, working directory, and session context. | A scoped design showing which current harness actions need machine-enforced binding. |

## Integration Classification

| Classification | Pattern | Boundary |
|---|---|---|
| Integrate as guidance | Persist critical continuity before lossy context reduction. | Use existing Engram and repository owners; do not create another memory store. |
| Integrate as guidance | Keep summaries linked to recoverable underlying evidence. | A summary never proves delivery, approval, or current truth by itself. |
| Integrate as guidance | Distinguish project content, runtime state, and user-level continuity. | Repository evidence remains authoritative for project truth. |
| Adapt before implementation | Reversible checkpoints or branch-aware handoff. | Must fit existing Git and roadmap authority without hidden snapshots or lifecycle state. |
| Adapt before implementation | Exact-context approval binding. | Gentle AI or another enforcing owner must issue and validate the approval; Engram only preserves context. |
| Adapt before implementation | Experience-derived improvement proposals. | Observed failures may propose bounded work, but cannot rewrite canonical skills or advance lifecycle automatically. |
| Reject | Autonomous promotion of memory or trajectories into policy. | It would bypass repository ownership and human authority. |
| Reject | Self-modifying skills that deploy without review, tests, and rollback evidence. | Learning quality is not authorization or verification. |
| Reject | Copying an upstream agent's session database, gateway, plugin manager, or scheduler. | This harness coordinates existing owners and must not become a general agent runtime. |
| Reject | Treating approval records, summaries, or memory confidence as completion receipts. | Completion remains grounded in repository evidence and applicable owner status. |

## Safety and Verification Requirements

Any future proposal based on this research should prove all of the following:

1. One canonical owner exists for every new fact and transition.
2. Raw or recoverable evidence survives summary loss where proportional.
3. Memory cannot grant approval, close work, or overwrite repository truth.
4. Learned behavior enters through an explicit proposal, review, deterministic tests, and rollback path.
5. Project-local executable capability is trust-gated and disabled safely when no decision channel exists.
6. Failure reports distinguish unavailable continuity, denied authority, failed verification, and incomplete delivery.
7. The design adds no duplicate lifecycle, session authority, conflict store, receipt cache, or hidden background mutation.

## Unread or Incomplete Sources

- `docs/architecture.md` at the pinned Hermes revision returned 404. The Hermes architecture claims here therefore rely only on the pinned README and previously located repository surfaces, not on that missing path.
- `packages/coding-agent/docs/session.md` at the pinned Pi revision returned 404. Session claims use the pinned README and `docs/compaction.md` instead.
- A large Pi extensions document was retrieved but tool output was truncated after the relevant trust, lifecycle, persistence, and permission sections. Unread trailing sections are not used as evidence.
- Earlier GitHub tree listings for the three repositories were truncated. Located but unread snippets are excluded from claims.
- Detailed Hermes curator, memory-provider, checkpoint, plugin-transaction, and trajectory implementation files were not re-read completely after context compaction. Those areas remain candidates for focused follow-up, not verified implementation guidance here.

## Unresolved Questions

- Which existing lifecycle item, if any, should own a future pre-compaction continuity design? Human decision required only if implementation is proposed.
- Can Gentle AI expose stable action-bound approval metadata without creating a harness-owned receipt? Unknown until its current owner contract is researched for that specific question.
- Which Hermes skill-curation checks separate a useful experience from an unsafe instruction? Unknown because the complete curator implementation was not retained as read evidence.
- Would branch-aware recovery materially improve this repository beyond Git, Engram, and current handoffs? Unknown until failure data demonstrates a concrete gap.

## Confidence

Confidence is high for the verified OpenClaw and Pi ownership, continuity, trust, and approval patterns cited above. Confidence is medium for Hermes learning-loop implications because the pinned overview is authoritative about product behavior but this bounded pass did not retain complete implementation evidence. Recommendations are intentionally conservative and do not constitute adoption.
