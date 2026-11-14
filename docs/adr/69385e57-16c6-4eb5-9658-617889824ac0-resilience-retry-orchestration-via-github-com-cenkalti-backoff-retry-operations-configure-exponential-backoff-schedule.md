---
version: 2
name: 69385e57-16c6-4eb5-9658-617889824ac0-resilience-retry-orchestration-via-github-com-cenkalti-backoff-retry-operations-configure-exponential-backoff-schedule
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: Retry Operations Configure Exponential Backoff Schedule"
metadata:
  actual:
    adr_id: 69385e57-16c6-4eb5-9658-617889824ac0
    status: accepted
    candidate_group_id: 1a9fc28f-a52f-4b72-9081-afceafbc050c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
      - pkg/job/**
---

# Resilience Retry Orchestration via github.com/cenkalti/backoff: Retry Operations Configure Exponential Backoff Schedule

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `handlers/**`
- `pkg/job/**`

## Context

- Health check handlers and scheduled background verification jobs execute network operations against remote endpoints across untrusted network boundaries.
- Transient network instability, temporary DNS resolution delays, and packet loss cause intermittent failures that produce false-positive monitoring alerts if operations fail immediately.
- Uncoordinated or unbounded retry loops risk amplifying target outages, depleting system connection pools, or violating strict request deadlines.
- The codebase establishes a unified resilience mechanism using the github.com/cenkalti/backoff library across protocol handlers and background job dispatchers.

## Problem Statement

Network probe operations executed across external boundaries are inherently vulnerable to transient network failures, temporary packet drops, and momentary service latency spikes. Without a coordinated retry strategy, transient faults lead to false-positive health check failures and alert storms. Conversely, uncoordinated or immediate retry loops risk exacerbating downstream outages through retry amplification and thundering herd behavior. The architecture requires a standardized, bounded retry mechanism that enforces exponential delays across all external diagnostic operations while respecting execution deadlines.

## Decision

1. MUST: Retry operations MUST configure an exponential backoff schedule using backoff.NewExponentialBackOff to calculate interval delays between successive attempts.

## Policy Block

- MUST Retry operations MUST configure an exponential backoff schedule using backoff.NewExponentialBackOff to calculate interval delays between successive attempts.

In scope:
- Outbound network operations, probe handlers, and automated diagnostic jobs dispatching requests across external network boundaries.
- Network verification routines evaluating remote HTTP, DNS, TCP, and ICMP endpoints.

Out of scope:
- Internal in-memory computations, static payload assertions, and local cache read or write operations.
- Non-idempotent mutation requests where retrying an operation risks side-effect duplication without explicit idempotency guarantees.

Exceptions:
- EXC-24-001: A target endpoint explicitly prohibits retries or mandates a fail-fast execution policy without delay intervals.

## Rationale

- Exponential backoff with bounded retries mitigates transient network anomalies by progressively lengthening delay intervals, preventing downstream saturation while giving recovering services time to stabilize.
- Adopting github.com/cenkalti/backoff provides a standardized interface for retry orchestration, reducing repetitive boilerplate and preventing divergent custom retry implementations across protocol modules.
- Coupling retry loops with explicit maximum retry limits and execution contexts ensures operations terminate predictably and honor caller timeouts.

## Consequences

Positive:
- Eliminates false-positive monitoring alerts caused by transient network blips and temporary endpoint unresponsiveness.
- Standardizes boundary resilience semantics across diverse protocol checkers using a consistent exponential backoff interface.
- Prevents denial-of-service pressure against recovering remote targets through progressive interval backing off.
- Enforces deterministic bounded execution times by capping maximum attempts and integrating context cancellation.

Negative:
- Increases latency for calls to persistently failing endpoints as attempts exhaust the configured retry schedule.
- Introduces dependency coordination overhead to align API signatures across module boundaries.
- Requires careful calibration of client socket timeouts against cumulative backoff schedules to avoid premature deadline truncation.

## Alternatives

- Immediate retry loops without backoff intervals (rejected)
  Rejected because: Immediate retries generate bursty traffic against failing remote endpoints, worsening transient congestion and inducing retry storms.
  When valid: Valid only in ultra-low-latency in-memory operations with dedicated circuit-breaker fail-fast semantics.
- Ad-hoc sleep loops using fixed time intervals (rejected)
  Rejected because: Fixed interval loops lack exponential backoff progression, jitter control, and standard cancellation interfaces, leading to synchronized thundering herd contention.
  When valid: Valid only in isolated procedural scripts without concurrent execution or shared boundary libraries.

## Risks

- Inconsistent version usage of github.com/cenkalti/backoff across packages leading to incompatible method signatures or context handling.
  Mitigation: Enforce lock-file version resolution and conduct automated dependency audits across module boundaries.
  Owner: Architecture and Core Infrastructure Team
- Aggregate backoff duration exceeding parent context deadline or caller timeout, leading to silent aborts before retry completion.
  Mitigation: Ensure client timeout budgets account for the sum of individual attempt timeouts and accumulated backoff delays.
  Owner: Service Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure exponential backoff parameters such as initial interval, multiplier, and maximum elapsed time to align with upstream SLA constraints.
- Wrap remote network calls in idempotent closures passed to backoff.Retry, ensuring that non-retryable terminal errors short-circuit via permanent error wrappers.

## Continuation Context


Verify commands:
- Discover the project dependency audit script from repository configuration and execute it to verify lock-file consistency for third-party resilience libraries.
- Discover and run the project test suite to ensure all network retry policies adhere to configured exponential backoff bounds and context cancellations.
- Execute static analysis rules discovered from repository configuration to detect direct unbounded network calls lacking retry resilience wrappers.

Accept when:
- All outbound network probe handlers and background job runners execute external calls through bounded github.com/cenkalti/backoff retry structures.
- Project verification test suites confirm that transient network faults trigger exponential backoff retries and terminate upon reaching maximum retry limits or context deadlines.
- Static analysis checks pass across all modules without flagging unhandled or unbounded external network calls.

## Enforcement

- Verified by: Automated static analysis and code linters integrated into continuous integration pipelines to identify non-compliant external network calls.
- Verified by: Peer code reviews verifying proper error handling, context propagation, and bounded retry configurations.
- Violation handling: Pull requests introducing external network calls without backoff.Retry wrappers must be rejected until compliant retry boundaries are established.
- Violation handling: Static analysis pipeline failures block deployment of unapproved external communication patterns.
- Exception process: Teams must submit an architectural waiver request documenting operational justification and upstream constraints for review by the Architecture Review Board.