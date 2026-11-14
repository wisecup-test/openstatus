---
version: 2
name: 826a44b4-2d88-4f7b-b9b9-cb4e4e433bf4-next-server-after-asynchronous-callback-adoption-route-handlers-performing-non-critical-post
description: "next/server after Asynchronous Callback Adoption: Route Handlers Performing Non Critical Post"
metadata:
  actual:
    adr_id: 826a44b4-2d88-4f7b-b9b9-cb4e4e433bf4
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Route Handlers Performing Non Critical Post

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`

## Context

- API route handlers process incoming requests requiring both immediate client responses and secondary side effects including analytics tracking and auxiliary persistence.
- Executing secondary side effects synchronously within the request-response cycle degrades latency and couples client success to auxiliary service availability.
- The codebase establishes a repeated pattern of encapsulating asynchronous telemetry and storage operations inside deferred execution callbacks.

## Problem Statement

Synchronously executing telemetry and auxiliary reporting during HTTP request handling introduces latency penalties and risks returning client-facing errors when secondary operations fail.

## Decision

1. MUST: Route handlers performing non-critical post-response side effects MUST schedule those operations using the after callback function from next/server rather than awaiting them on the primary response path.

## Policy Block

- MUST Route handlers performing non-critical post-response side effects MUST schedule those operations using the after callback function from next/server rather than awaiting them on the primary response path.

In scope:
- API route handlers processing requests that produce secondary telemetry, analytics, or asynchronous persistence.
- Stateless serverless endpoints prioritizing client-observed response latency over immediate side-effect confirmation.

Out of scope:
- Transactional endpoints where the client requires synchronous confirmation of state updates.
- Long-running batch workloads that exceed request lifecycle duration limits.

## Rationale

- Encapsulating secondary tasks within the after lifecycle hook allows the HTTP response to be returned immediately while ensuring background tasks complete reliably.
- Independent error handling inside the callback isolates auxiliary failures from the client, preserving route availability.

## Consequences

Positive:
- Reduces client response latency by offloading non-critical telemetry and secondary persistence from the synchronous request path.
- Isolates auxiliary failures so that secondary reporting issues do not cause route handler response errors.
- Maintains clean boundaries between core request validation and subsequent analytics or reporting workflows.

Negative:
- Background task failures occur out of band and cannot surface error statuses directly to the client.
- Developers must manage independent try-catch blocks within background blocks to isolate multi-step tasks from cascading failures.

## Alternatives

- Synchronous inline execution of persistence and analytics on the primary request-response path (rejected)
  Rejected because: Adds client-observed latency by forcing HTTP responses to block on non-critical secondary storage and telemetry operations.
  When valid: When the client requires immediate transactional confirmation or read-after-write consistency for secondary updates.
- Dedicated external message broker or background queue system (rejected)
  Rejected because: Introduces additional infrastructure dependencies and operational complexity for lightweight side effects within stateless route handlers.
  When valid: When background workloads exceed execution time boundaries or require guaranteed delivery and retry guarantees across node failures.

## Risks

- Secondary task failures execute out of band and remain unnoticed without monitoring.
  Mitigation: Enforce explicit error logging and structured reporting within all deferred callbacks.
  Owner: Engineering Team
- Platform runtimes may terminate asynchronous execution if deferred operations exceed execution timeouts.
  Mitigation: Restrict operations in deferred callbacks to lightweight, bounded, and idempotent calls.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Wrap distinct secondary tasks inside the deferred execution callback in separate try-catch blocks to prevent an initial failure from terminating subsequent operations.
- Capture all request-scoped data into local constants before delegating to the deferred callback to prevent state corruption across asynchronous boundaries.

## Continuation Context


Verify commands:
- Discover the test runner execution script declared in the repository dependency manifest and run the test suite verifying route handler lifecycle behavior.
- Discover and execute the project static analysis validation script to verify that post-response background operations adhere to after callback scheduling constraints.

Accept when:
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

## Enforcement

- Verified by: Automated test suites validating response completion timing and side-effect isolation.
- Verified by: Peer code review inspecting route handler implementations to ensure non-critical operations are deferred.
- Violation handling: Pull requests with synchronous non-critical side effects blocking route responses will be blocked until refactored into deferred callbacks.
- Exception process: Exceptions require architectural review and documentation detailing why secondary effects must block the synchronous HTTP response for data consistency.