---
version: 2
name: 0aa4a96f-cd18-456f-b444-d79e6df54f93-github-com-cenkalti-backoff-v5-for-exponential-retry-resilience-operations-wrapped-backoff-retry-accept-honor
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Operations Wrapped Backoff Retry Accept Honor"
metadata:
  actual:
    adr_id: 0aa4a96f-cd18-456f-b444-d79e6df54f93
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Operations Wrapped Backoff Retry Accept Honor

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `pkg/job/**`

## Context

- Network verification tasks executing DNS queries and TCP probes encounter transient network anomalies, packet loss, and temporary service unavailability.
- Failing immediately on initial probe error results in false-positive incident notifications and degraded reliability metrics for monitored targets.
- A standardized exponential retry mechanism is required to distinguish transient connectivity interruptions from sustained target outages without overwhelming target systems with immediate retries.
- The repository standardizes on github.com/cenkalti/backoff/v5 to provide context-aware exponential backoff across asynchronous probe executions.

## Problem Statement

Network verification probes executing network-level checks frequently encounter transient socket errors, packet timeouts, and temporary DNS resolution hiccups. Without a unified, context-aware backoff and retry strategy, operations either fail prematurely or execute uncoordinated retry loops that lack jitter, exponential delay, or cancellation handling, producing misleading monitoring results and unnecessary load.

## Decision

1. MUST: Operations wrapped in backoff.Retry MUST accept and honor the cancellation signals of the passed context, terminating retry attempts immediately when the context deadline expires or cancellation is triggered.

## Policy Block

- MUST Operations wrapped in backoff.Retry MUST accept and honor the cancellation signals of the passed context, terminating retry attempts immediately when the context deadline expires or cancellation is triggered.

In scope:
- Execution of network diagnostic routines, probes, and checker jobs subject to transient network failures.
- Any external I/O operation requiring bounded exponential backoff retry semantics.

Out of scope:
- Deterministic operations where errors are non-transient, such as contract validation or malformed payloads.
- In-memory computations and non-I/O data transformations.

## Rationale

- The github.com/cenkalti/backoff/v5 library provides robust, context-aware exponential backoff capabilities with randomized jitter to prevent synchronized retry storms.
- Configuring retries via functional options backoff.WithMaxTries and backoff.WithBackOff bounds execution time while accommodating brief network turbulence.
- Standardizing on a single retry library across checker jobs eliminates divergent custom retry loops and unifies resilience behavior.

## Consequences

Positive:
- Dramatically reduces false-positive probe failures caused by transient network blips.
- Prevents target overload by spacing retries with exponential backoff and randomized jitter.
- Provides native integration with request contexts to abort retries upon timeout or shutdown.

Negative:
- Increases total execution latency and wall-clock duration for probes encountering persistent failures.
- Introduces an external module dependency that must be maintained and tracked in lock files.
- Increases memory residency while holding closures and retry state across backoff intervals.

## Alternatives

- Standard library sleep-based retry loops (rejected)
  Rejected because: Lacks jitter, exponential intervals, context integration, and introduces bespoke error-prone retry logic across different job packages.
  When valid: Trivial standalone scripts with no concurrent execution or external dependencies.
- Cross-service circuit breaker and retry mesh frameworks (rejected)
  Rejected because: Introduces unnecessary runtime complexity and stateful coordination for stateless probe execution jobs.
  When valid: Large-scale distributed microservices requiring centralized failure tracking and cross-service circuit breaking.

## Risks

- Unbounded backoff durations causing probe execution timeouts or resource starvation.
  Mitigation: Strictly enforce backoff.WithMaxTries and root context cancellation timeouts on every backoff.Retry invocation.
  Owner: engineering team
- Retrying non-transient errors such as invalid configuration or unresolvable domain schemas.
  Mitigation: Differentiate permanent errors within the operation closure to return permanent failure indicators where appropriate.
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
- Pass the invocation context directly into backoff.Retry to guarantee immediate termination when outer timeouts or cancellation signals occur.
- Always initialize backoff.NewExponentialBackOff inside backoff.WithBackOff to ensure proper initialization of jitter and backoff interval calculations.

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis and linting scripts to verify adherence to retry API usage standards.
- Discover and execute the test runner across network probe packages to ensure retry resilience and context cancellation behave as expected.

Accept when:
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline.
- Verified by: Mandatory peer code review for changes affecting job execution packages.
- Violation handling: Pull requests containing custom retry loops or missing backoff options are blocked from merging until aligned with standard backoff patterns.
- Exception process: Exceptions for non-retriable or single-shot probe executions require documented approval from the architecture review board.