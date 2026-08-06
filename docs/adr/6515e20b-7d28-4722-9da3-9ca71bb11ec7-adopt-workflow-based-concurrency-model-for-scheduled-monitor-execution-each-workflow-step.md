# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Each Workflow Step

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all workflow-based monitor execution systems.

## Context

- The system executes scheduled monitoring tasks that require coordination across multiple time-based steps with distinct lifecycle stages
- Monitor workflows must handle user-scoped execution with rate limiting and caching to prevent resource exhaustion
- The architecture separates workflow orchestration from task execution, requiring explicit step definitions and state transitions
- Reliability requirements demand persistent state management across workflow steps with configurable expiration policies

## Problem Statement

Scheduled monitoring systems require a concurrency model that coordinates multi-step workflows across time boundaries while maintaining reliability guarantees, rate limiting, and state persistence. Ad-hoc task scheduling approaches fail to provide the structured step transitions, error handling, and resource isolation needed for production monitoring at scale.

## Decision

1. MUST: Each workflow step MUST be implemented as a distinct, named function that can be independently invoked and tested

## Policy Block

- MUST Each workflow step MUST be implemented as a distinct, named function that can be independently invoked and tested

In scope:
- All scheduled monitor execution workflows
- Cron-triggered workflow initialization
- Multi-step workflows with time-based transitions
- User-scoped workflow execution with rate limiting

Out of scope:
- Synchronous API request handling
- Real-time event processing without workflow state
- Single-step task execution without coordination requirements
- Workflows without reliability or rate limiting constraints

## Rationale

- The evidence shows explicit workflow step definitions including initialization, intermediate states, and terminal states, indicating a structured concurrency model rather than ad-hoc task scheduling
- Rate limiting with token-based intervals and cache layers with expiration policies demonstrate reliability-focused design that prevents resource exhaustion
- Integration with task queue systems and schema validation at workflow boundaries indicates production-grade orchestration requirements
- The separation of workflow contracts from implementation enables independent testing and evolution of individual steps

## Consequences

Positive:
- Explicit workflow steps provide clear boundaries for testing, monitoring, and debugging multi-step execution
- Rate limiting and cache expiration policies prevent resource exhaustion and provide predictable performance characteristics
- Schema validation at workflow boundaries catches data integrity issues before execution begins
- Separation of workflow orchestration from task execution enables independent scaling and failure isolation

Negative:
- Workflow-based concurrency introduces additional complexity compared to simple scheduled task execution
- State persistence across workflow steps requires coordination between cache layers and task queues, increasing operational overhead
- Explicit step definitions create more code surface area and require careful versioning when workflow logic evolves
- Rate limiting at the workflow level may introduce latency for time-sensitive monitoring operations

## Alternatives

- Use simple cron-based task scheduling without workflow orchestration (rejected)
  Rejected because: Simple task scheduling cannot provide the multi-step coordination, state persistence, and rate limiting guarantees required for reliable monitor execution at scale
  When valid: Valid for single-step monitoring tasks without reliability requirements or state coordination needs
- Implement actor-based concurrency model with message passing between monitor instances (rejected)
  Rejected because: Actor-based models require more complex state management and do not naturally support time-based step transitions needed for scheduled monitoring workflows
  When valid: Valid for real-time event-driven monitoring systems where message passing semantics are primary
- Use database-backed job queues with polling for workflow coordination (deferred)
  When valid: May be considered if task queue system integration proves insufficient for workflow reliability requirements

## Risks

- Workflow state inconsistency if cache expiration occurs before workflow completion
  Mitigation: Set cache expiration policies to exceed maximum workflow duration and implement state recovery mechanisms
  Owner: engineering team
- Rate limiting may cause workflow step delays that violate monitoring SLAs
  Mitigation: Monitor rate limiter token exhaustion metrics and adjust interval parameters based on observed workflow execution patterns
  Owner: engineering team
- Schema validation failures may silently drop workflow executions without alerting
  Mitigation: Implement structured error logging and monitoring for validation failures with alerting thresholds
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define workflow step contracts as exported types or interfaces that declare input parameters, return types, and error conditions for each step in the execution sequence
- Implement cache key naming conventions that include workflow identifiers and user scope to prevent key collisions across concurrent workflow executions
- Configure rate limiter token intervals based on observed workflow execution frequency and adjust dynamically based on system load metrics

## Continuation Context


Verify commands:
- Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
- Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
- Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level

Accept when:
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing

## Enforcement

- Verified by: Code review verification that workflow steps follow explicit contract definitions
- Verified by: Integration tests that validate workflow state persistence across step transitions
- Verified by: Load testing that verifies rate limiting prevents resource exhaustion
- Violation handling: Workflow implementations without explicit step contracts are rejected in code review
- Violation handling: Cache configurations without expiration policies trigger build-time validation failures
- Violation handling: Rate limiting violations detected in load testing block deployment to production
- Exception process: Exceptions require architectural review and must document why workflow-based concurrency is inappropriate for the specific use case
- Exception process: Temporary exceptions for prototype or experimental features must include sunset dates and migration plans
- Exception process: All exceptions are tracked in architecture decision log with quarterly review