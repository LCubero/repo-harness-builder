# RH-W-0070 Preventive Resource-Safety Design

## Decision

RH-W-0070 defines proportional preventive resource safety for agent and harness operations. An observed delegated review reached near-saturation CPU, but available evidence does not verify a root cause. The policy controls launch shape and scope without attributing the incident to a specific command, tool, or subsystem.

Review this design against the canonical [RH-W-0070 roadmap record](roadmap.md#rh-w-0070), [working agreements](../../.repo-harness/rules/working-agreements.md), and the [Spanish peer](../es/repo-harness/rh-w-0070-design.md). This is a separate cross-cutting lifecycle unit. It guides later work without merging into RH-W-0051, RH-W-0071, or another implementation.

## Authority And Applicability

Repository rules and explicit current user instructions remain authoritative. This design adds a preventive policy for actions initiated or delegated by an agent or harness.

- Lightweight documentation edits, bounded reads, readback, and similarly cheap work proceed without forecasts or extra ceremony.
- Long, very-long, expensive, or cost-unknown work requires a forecast before launch.
- An action whose risk cannot be bounded fails closed to a forecast, a smaller bounded action, or a user decision.
- CPU, GPU, and RAM are considered where observable and applicable. GPU awareness never creates a GPU requirement.

The operational target is to remain below 90 percent utilization. It is a planning ceiling, not a promise of exact measurement, continuous enforcement, or identical behavior across hosts and providers.

## Inputs And Precedence

Inputs are the requested outcome, authorized scope, expected duration, expected concurrency and batch size, available CPU, GPU, and RAM telemetry, provider cost signals when available, and the cheapest sufficient verification path.

Precedence is:

1. Explicit user limits and authorization within repository safety rules.
2. Known action-specific bounds supported by current evidence.
3. Available resource or provider telemetry.
4. Conservative static bounds when telemetry is unavailable.

Missing telemetry is reported as unavailable. No measurement is inferred or fabricated.

## Preventive Behavior

Before launch, select the smallest action that can establish the requested outcome:

- Use focused scope instead of broad or recursive scans.
- Bound concurrency, workers, and batch size.
- Launch once per requested action.
- Set an explicit timeout for potentially long work.
- Use the cheapest sufficient verification and avoid redundant scans, builds, tests, and reviews.
- Forecast duration, concurrency, likely CPU, GPU, and RAM pressure, and provider cost before long or very-long work.
- If the forecast may exceed the policy, request explicit user approval before launch and allow the user to decline or reduce scope.

When telemetry is available, use it only as an input to the launch decision. When it is unavailable, use conservative static bounds and disclose the limitation. Never broaden scope, add retries, or increase concurrency merely because measurements are absent.

## Outputs And Failure Behavior

Lightweight actions produce their normal result. A forecasted action reports the proposed scope, bounds, timeout, expected duration class, resource or cost risk, telemetry availability, and the user decision required, if any.

Failure to obtain telemetry is not failure by itself. Failure to bound an expensive or unknown action blocks that launch and returns a smaller safe action or a forecast for approval. Timeout, partial evidence, or provider uncertainty is reported honestly and never converted into success.

No action may kill, throttle, reprioritize, or otherwise interfere with unrelated processes.

## Acceptance Criteria

- Lightweight documentation and bounded readback remain direct and low ceremony.
- Long, very-long, expensive, or unknown actions receive a resource or cost forecast before launch.
- Concurrency, batch size, launches, scope, and timeout are explicit and bounded where applicable.
- The chosen verification is the cheapest sufficient path and avoids redundant expensive work.
- Available CPU, GPU, and RAM evidence is considered without promising exact enforcement.
- Unavailable telemetry is reported and replaced by conservative static bounds, never invented values.
- A forecast that may exceed policy requires explicit user approval and supports declining or reducing scope.
- Unrelated processes remain untouched.

## Tests And Evidence

Any future implementation must start with focused evidence, not a full or recursive suite. Expected cases cover lightweight bypass, bounded forecast fields, one launch, timeout, conservative no-telemetry fallback, user decline or reduced scope, and proof that unrelated processes are never targeted. Documentation review and bounded readback are sufficient evidence for this design-only work.

## Non-Goals

This design adds no daemon, monitor subsystem, scheduler, dependency, process killer, retry loop, GPU requirement, command-specific API, command-specific implementation, universal throttle, exact utilization guarantee, or continuous telemetry. It does not diagnose the observed incident or authorize implementation.

## Migration And Compatibility

The design changes no product bytes, persisted schema, generated output, manifest, checker, runtime dependency, or existing approval. Existing lightweight workflows remain compatible. Future units adopt this policy within their own lifecycle and verification boundaries.

## Lifecycle Relationship

RH-W-0070 remains `proposed` as a separate cross-cutting policy unit. It may guide RH-W-0051, RH-W-0071, delegated review, and later work, but it does not merge their ownership or implementation. No implementation plan is required now. A later plan requires evidence that documentation and operating policy are insufficient plus separate owner authorization.
