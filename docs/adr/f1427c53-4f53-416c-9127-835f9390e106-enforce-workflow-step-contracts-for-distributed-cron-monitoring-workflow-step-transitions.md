# Enforce Workflow Step Contracts for Distributed Cron Monitoring: Workflow Step Transitions

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE for all workflow orchestration components that coordinate distributed monitoring tasks.

## Context

- The system coordinates distributed monitoring workflows using cloud task queues, requiring explicit contracts between workflow initialization and multi-day step transitions to ensure reliable state progression across service boundaries.
- Workflow steps operate across temporal boundaries (3-day, 14-day, and paused states) with persistent state stored in distributed cache layers and database records, necessitating type-safe contracts to prevent state corruption during step transitions.
- Rate limiting (15 tokens per second) and cache expiration policies (30-day TTL) constrain workflow execution, requiring contracts that encode retry semantics and temporal guarantees to maintain monitoring reliability under resource limits.
- The monitoring system processes user-scoped workflow entries with workspace validation, creating coordination dependencies between database queries, cache operations, and task queue submissions that must be governed by explicit interface contracts.

## Problem Statement

Distributed monitoring workflows that span multiple temporal stages and coordinate across task queues, cache layers, and persistent storage require explicit public contracts to prevent state inconsistencies, ensure type-safe step transitions, and maintain reliability guarantees when workflow components evolve independently or fail partially.

## Decision

1. MUST: All workflow step transitions MUST be governed by explicit public contract types that define input state, output state, and temporal progression semantics.

## Policy Block

- MUST All workflow step transitions MUST be governed by explicit public contract types that define input state, output state, and temporal progression semantics.

In scope:
- Workflow orchestration components that coordinate distributed monitoring tasks across temporal boundaries
- Step transition logic that persists state to cache layers or database records
- Task queue submission interfaces that initialize or advance multi-stage workflows
- Rate-limited workflow execution paths that must maintain reliability under resource constraints

Out of scope:
- Internal implementation details of individual workflow steps that do not cross service boundaries
- Cache layer implementation specifics beyond key patterns and expiration policies exposed in contracts
- Database query optimization strategies that do not affect contract-level state semantics
- Rate limiter internal token bucket mechanics that do not impact contract-specified retry behavior

Exceptions:
- EXC-001: Prototype workflows that execute entirely within a single process boundary without persistent state or task queue coordination
- EXC-002: Emergency hotfixes that modify workflow step logic without changing contract signatures or state transition semantics

## Rationale

- The evidence shows explicit workflow step contracts (LaunchMonitorWorkflow, Step14Days, Step3Days, StepPaused, workflowStep) coordinating with cloud task queues, database access patterns, and cache layers, indicating a deliberate architectural choice to govern distributed state transitions through typed interfaces.
- Rate limiting at 15 tokens per second combined with 30-day cache expiration policies creates resource constraints that require contracts to encode retry semantics and temporal guarantees, preventing workflow failures under load.
- Workspace validation using schema parsing (selectWorkspaceSchema.parse) and user-scoped database queries demonstrate that contracts must enforce type safety across coordination boundaries to prevent state corruption when processing multi-tenant monitoring data.
- The presence of multiple temporal step types (3-day, 14-day, paused) alongside workflow initialization logic indicates that contracts serve as the reliability mechanism for ensuring correct state progression across long-running distributed monitoring operations.

## Consequences

Positive:
- Type-safe workflow step transitions prevent state corruption when monitoring workflows span multiple days and coordinate across task queues, cache layers, and persistent storage.
- Explicit contracts enable independent evolution of workflow components (initialization, step execution, state persistence) without breaking distributed coordination guarantees.
- Contract-encoded temporal boundaries and rate limiting constraints make reliability requirements explicit and verifiable at compile time rather than discoverable only through runtime failures.
- Separation of concerns between user-scoped retrieval, workspace validation, and task submission reduces coupling and enables targeted testing of each coordination boundary.

Negative:
- Contract maintenance overhead increases when workflow requirements evolve, requiring coordinated updates across initialization, step transition, and state persistence logic.
- Explicit contract types may introduce rigidity that complicates rapid prototyping of new monitoring workflow patterns or experimental step transition strategies.
- Contract-level exposure of cache layer operations (key patterns, expiration policies) creates tighter coupling between workflow logic and infrastructure implementation details.
- Type-safe contracts require additional validation logic (schema parsing, state verification) that adds latency to workflow step transitions and increases computational overhead.

## Alternatives

- Use untyped message passing with runtime validation for workflow step coordination (rejected)
  Rejected because: Runtime validation cannot prevent state corruption at coordination boundaries and increases failure surface area in distributed monitoring workflows where partial failures are common. The evidence shows explicit typed contracts (LaunchMonitorWorkflow, Step14Days, Step3Days, StepPaused) indicating a deliberate choice for compile-time safety over runtime flexibility.
  When valid: Valid for prototype workflows with single-process execution and no persistent state requirements
- Embed all workflow logic in monolithic step handlers without explicit contract boundaries (rejected)
  Rejected because: Monolithic handlers prevent independent evolution of coordination concerns (task queue submission, cache operations, database queries) and make it impossible to test workflow reliability properties in isolation. The evidence shows separation between workflowInit, step types, and workflowStep, indicating intentional boundary definition.
  When valid: Valid for simple single-stage workflows that do not require temporal progression or distributed coordination
- Define contracts as runtime configuration rather than compile-time types (rejected)
  Rejected because: Configuration-based contracts cannot enforce type safety for state transitions and lose the ability to verify temporal progression semantics at build time. The evidence shows schema validation (selectWorkspaceSchema.parse) and typed step contracts, indicating preference for static verification over dynamic configuration.
  When valid: Valid when workflow patterns must be modified by operators without code deployment, accepting increased runtime failure risk

## Risks

- Contract evolution may break existing workflow instances mid-execution if state serialization formats change incompatibly, causing monitoring gaps or duplicate notifications.
  Mitigation: Implement contract versioning with backward-compatible state migration logic. Maintain parallel contract versions during transition periods. Add monitoring to detect workflow instances stuck on deprecated contracts.
  Owner: Engineering team (workflow reliability)
- Explicit cache layer contracts may leak infrastructure implementation details that constrain future migration to alternative caching strategies or expiration policies.
  Mitigation: Define cache contracts at semantic level (state persistence guarantees, consistency requirements) rather than implementation level (specific key patterns, TTL values). Use adapter pattern to isolate cache implementation from workflow contracts.
  Owner: Engineering team (infrastructure)
- Rate limiting constraints encoded in contracts may become bottlenecks as monitoring workload scales, requiring contract changes that ripple through all workflow components.
  Mitigation: Externalize rate limit configuration from contracts where possible. Design contracts to accept rate limiting as injected dependency rather than hardcoded constant. Monitor token exhaustion rates and alert before limits impact reliability.
  Owner: Engineering team (observability)

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define workflow step contracts as exported types that specify input state structure, output state structure, temporal progression rules, and failure recovery semantics. Ensure contracts are co-located with workflow orchestration logic for discoverability.
- Implement contract validation at workflow initialization and step transition boundaries using schema parsing to catch state inconsistencies before they propagate to cache layers or task queues. Log validation failures with sufficient context for debugging distributed coordination issues.
- Design cache key patterns and expiration policies as part of the public contract surface when cache consistency is critical to workflow reliability. Document the relationship between cache TTL values and workflow temporal boundaries to prevent premature state eviction.
- Separate workflow contracts into initialization contracts (LaunchMonitorWorkflow), step transition contracts (Step3Days, Step14Days, StepPaused), and completion contracts to enable independent testing and evolution of each workflow stage.

## Continuation Context


Verify commands:
- Discover the project's workflow contract type definitions and verify that all step transition types export explicit input state, output state, and temporal progression semantics
- Locate the project's workflow orchestration test suite and execute tests that verify contract validation occurs at initialization and step transition boundaries
- Identify the project's cache layer integration and verify that cache key patterns and expiration policies referenced in workflow contracts match actual cache operations

Accept when:
- All workflow step contracts define explicit types for state transitions and temporal progression, verified by successful compilation without type errors
- Contract validation logic executes at workflow initialization and step boundaries, verified by test coverage showing validation failures prevent invalid state transitions
- Cache operations use key patterns and expiration policies consistent with workflow contract specifications, verified by integration tests that exercise full workflow lifecycle

## Enforcement

- Verified by: Type system verification during compilation ensures workflow contracts are satisfied at all coordination boundaries
- Verified by: Integration tests exercise full workflow lifecycle including initialization, multi-day step transitions, and paused state handling to verify contract compliance
- Verified by: Code review checklist includes verification that new workflow steps define explicit contracts and implement validation at transition boundaries
- Violation handling: Compilation failures block deployment when workflow code violates contract type signatures or omits required state validation
- Violation handling: Runtime contract validation failures trigger workflow suspension and alert on-call engineers with state context for debugging
- Violation handling: Code review identifies missing contracts or inadequate validation logic and requires revision before merge approval
- Exception process: Exception requests must document specific workflow requirements that cannot be satisfied by existing contract patterns and propose alternative reliability mechanisms
- Exception process: Engineering lead reviews exception requests to assess impact on distributed coordination guarantees and approves only when alternative mechanisms provide equivalent reliability
- Exception process: Approved exceptions require documentation of contract limitations, alternative validation strategy, and migration path to contract-governed implementation