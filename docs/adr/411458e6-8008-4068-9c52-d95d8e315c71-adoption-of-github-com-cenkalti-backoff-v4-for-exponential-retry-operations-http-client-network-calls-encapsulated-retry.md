---
version: 2
name: 411458e6-8008-4068-9c52-d95d8e315c71-adoption-of-github-com-cenkalti-backoff-v4-for-exponential-retry-operations-http-client-network-calls-encapsulated-retry
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Http Client Network Calls Encapsulated Retry"
metadata:
  actual:
    adr_id: 411458e6-8008-4068-9c52-d95d8e315c71
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Http Client Network Calls Encapsulated Retry

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `handlers/**`

## Context

- Network operations against external target endpoints and remote health verification endpoints experience transient failures such as network timeouts and dropped connections.
- HTTP handler components execute outbound requests using configured timeouts but require standardized recovery strategies when temporary network errors occur.
- Handlers across the checker service require a uniform mechanism to coordinate bounded retries with exponential backoff rather than implementing ad-hoc retry loops.

## Problem Statement

Network calls executed by diagnostic and health check handlers are vulnerable to transient network failures, requiring a standardized, bounded exponential backoff mechanism to prevent cascading latency while ensuring transient errors do not cause false-positive diagnostic failures.

## Decision

1. MUST: HTTP client network calls encapsulated in a retry operation MUST enforce an explicit timeout on the client instance.

## Policy Block

- MUST HTTP client network calls encapsulated in a retry operation MUST enforce an explicit timeout on the client instance.

In scope:
- All network client request handlers executing outbound diagnostic checks and regional ping operations.
- Outbound network operations subject to transient connection resets, socket timeouts, or temporary remote endpoint unavailability.

Out of scope:
- Operations with non-idempotent semantics where retrying partial execution introduces data corruption risks.
- Internal in-memory data transformations, logging statements, and local cache reads or writes.

## Rationale

- The evidence across multiple checker handler modules demonstrates repeated adoption of github.com/cenkalti/backoff/v4 with backoff.NewExponentialBackOff and backoff.WithMaxRetries for encapsulating outbound HTTP requests.
- Using a tested external backoff implementation ensures uniform jitter and interval scaling without the risks of manual sleep loops or unconstrained retry storms.
- Encapsulating retry execution at the handler boundary preserves separation between network transport configuration and fault recovery orchestration.

## Consequences

Positive:
- Standardizes exponential retry behavior across external HTTP checks and ping health verifications.
- Eliminates bespoke retry loops, preventing thundering herd problems through consistent exponential backoff algorithms.
- Provides clean operation abstraction where retry logic is decoupled from transport and handler response decoding.

Negative:
- Introduces a third-party dependency into core handler execution paths that requires ongoing dependency tracking and vulnerability auditing.
- Misconfigured retry counts or backoff ceilings can cause cumulative latency amplification during cascading upstream service outages.

## Alternatives

- Custom in-house retry loops with manual sleep intervals (rejected)
  Rejected because: Custom retry implementations introduce inconsistent jitter, lack standardized backoff algorithms, and risk unhandled edge cases across separate handlers.
  When valid: Only when zero external dependencies are strictly mandated by runtime constraints.
- Standard library HTTP transport retry mechanisms without external backoff primitives (rejected)
  Rejected because: Transport-level retry hooks do not provide composable exponential backoff, jitter configuration, or flexible operation encapsulation for arbitrary operations.
  When valid: When retrying idempotently strictly at the transport layer for idempotent HTTP verbs only.

## Risks

- Exhaustion of handler context deadlines due to excessive retry intervals during sustained downstream outages.
  Mitigation: Set explicit maximum retry counts and ensure outbound HTTP client timeouts align with overall request context deadlines.
  Owner: engineering team
- Unintended retries on permanent HTTP failure codes or non-idempotent operations.
  Mitigation: Restrict retries to idempotent check operations and inspect error types before returning non-nil from the retry operation closure.
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
- Wrap target HTTP operations inside an operation closure that returns an error when transient failure conditions are encountered.
- Ensure backoff configuration limits both total attempts and total duration to prevent handler context exhaustion.

## Continuation Context


Verify commands:
- Discover and execute the project test runner for handler packages to validate exponential backoff behavior under simulated network faults.
- Discover and run the project static analysis and linting targets to ensure no unmanaged retry loops exist in handler implementations.

Accept when:
- All outbound HTTP client network calls that execute within request handlers coordinate transient failure recovery through the adopted backoff library.
- Automated verification scripts confirm zero unhandled transient network errors and verify that retry operations enforce configured maximum retry counts.
- Static analysis verifies that lock resolution artifacts match the active environment before building handler packages.

## Enforcement

- Verified by: Automated test suites executing simulated transient network faults against HTTP client boundaries.
- Verified by: Code review verification ensuring new outbound network calls wrap execution within standardized retry operations.
- Verified by: Static analysis sweeps checking for direct unmanaged network retry loops.
- Violation handling: Pull requests containing custom retry loops or unmanaged network calls without backoff wrappers will be blocked during review.
- Violation handling: Flagged violations must be refactored to use the adopted retry library before merging.
- Exception process: Submit an architecture variance request detailing why standard exponential backoff is incompatible with the operational requirements of the specific handler.
- Exception process: Obtain written approval from the service maintainers and document the non-standard retry or fail-fast behavior in the handler design documentation.