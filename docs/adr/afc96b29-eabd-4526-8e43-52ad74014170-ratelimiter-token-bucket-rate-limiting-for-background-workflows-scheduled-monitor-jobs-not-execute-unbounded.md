---
version: 2
name: afc96b29-eabd-4526-8e43-52ad74014170-ratelimiter-token-bucket-rate-limiting-for-background-workflows-scheduled-monitor-jobs-not-execute-unbounded
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Scheduled Monitor Jobs Not Execute Unbounded"
metadata:
  actual:
    adr_id: afc96b29-eabd-4526-8e43-52ad74014170
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Scheduled Monitor Jobs Not Execute Unbounded

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/cron/**`

## Context

- Background monitoring workflows execute scheduled polling routines that interact with external services, databases, and notification channels.
- Unconstrained concurrent execution during scheduled runs can generate sudden traffic bursts that degrade downstream systems or exhaust resource pools.
- In-process token bucket rate limiting controls the rate of dispatched requests within workflow execution boundaries.

## Problem Statement

Uncontrolled batch execution of background monitoring tasks can overwhelm downstream services, databases, or external task infrastructure during recurring scheduled runs. Without rate limiting, rapid burst queries and task dispatches risk triggering rate limits on external services, exhausting database connections, or degrading system stability. A formal architectural constraint is required to throttle workflow task execution to sustainable token-bucket processing rates.

## Decision

1. MUST_NOT: Scheduled monitor jobs MUST_NOT execute unbounded parallel operations against external APIs, task dispatch queues, or database access layers without acquiring rate tokens from RateLimiter.

## Policy Block

- MUST_NOT Scheduled monitor jobs MUST_NOT execute unbounded parallel operations against external APIs, task dispatch queues, or database access layers without acquiring rate tokens from RateLimiter.

In scope:
- Background cron workflows and scheduled batch processing routines dispatching external or database operations.
- Asynchronous task dispatch loops requiring outbound throughput throttling.

Out of scope:
- Synchronous user request-response lifecycles handled by edge gateway rate limiting.
- Interactive administrative scripts executed locally in development environments.

## Rationale

- Configuring RateLimiter with explicit token intervals provides predictable throughput pacing across batch cron cycles, avoiding sudden bursts against downstream services.
- In-process token bucket rate limiting minimizes external network latency overhead compared to remote coordinator calls for every individual token allocation.
- Evidence indicates single-file usage within monitoring workflows, demonstrating an initial reliability guardrail that requires formalization before wider adoption.

## Consequences

Positive:
- Mitigates the risk of exceeding downstream service quotas and external API rate limits during batch cron processing.
- Smooths outbound request volume, reducing database connection contention and processing latency spikes.
- Provides lightweight in-process execution throttling without introducing mandatory external network round-trips for token acquisition.

Negative:
- In-process rate limiting does not share token state across horizontally scaled worker instances without additional coordination layers.
- Throttling extends overall completion time for high-volume batch execution cycles by pacing throughput evenly.
- Worker process restarts reset local token bucket state, momentarily permitting fresh token allocations upon initial startup.

## Alternatives

- Distributed cache-backed token bucket rate limiting across shared infrastructure (rejected)
  Rejected because: Introduces external network round-trip latency and operational dependency overhead for single-worker background tasks.
  When valid: When background cron workers are horizontally scaled across multiple instances requiring unified shared token quotas.
- Unthrottled concurrent execution with downstream retry backoff (rejected)
  Rejected because: Causes severe burst traffic spikes that trigger downstream provider rate limits and risk connection pool exhaustion before backoff mechanisms activate.
  When valid: When downstream dependencies provide unlimited burst capacity and no rate quotas exist.

## Risks

- Horizontal scaling of cron workers could bypass intended rate limits because in-process RateLimiter state is not shared across processes.
  Mitigation: Ensure singleton worker scheduling or transition to distributed coordination if workers scale horizontally across multiple instances.
  Owner: engineering team
- Overly restrictive token intervals or small token allocations could cause queue backlog buildup in background workflows.
  Mitigation: Monitor task queue processing latencies and establish alerting on task processing backlogs.
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
- Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.

## Continuation Context


Verify commands:
- Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
- Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.

Accept when:
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

## Enforcement

- Verified by: Automated test suites in continuous integration validating rate limiting behavior and timing constraints.
- Verified by: Peer architectural code review for changes affecting background workflow task dispatch loops.
- Violation handling: Pull requests introducing unthrottled batch operations against external APIs or database tables will be blocked during code review.
- Violation handling: Continuous integration verification failures will halt deployment pipelines.
- Exception process: Submit an architectural exception request documenting operational requirements that necessitate unthrottled processing or distributed quota management.
- Exception process: Obtain approval from the technical lead prior to bypassing RateLimiter controls in background jobs.