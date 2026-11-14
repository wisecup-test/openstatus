---
version: 2
name: 6e21f302-7c56-4345-a620-f05c6ffe27d0-httputil-reverseproxy-retrying-transport-with-request-body-replay-retry-loop-evaluate-req-context-done
description: "httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Loop Evaluate Req Context Done"
metadata:
  actual:
    adr_id: 6e21f302-7c56-4345-a620-f05c6ffe27d0
    status: accepted
    candidate_group_id: c395309c-fccd-436b-9eed-4e7605867f18
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Retrying Transport with Request Body Replay: Retry Loop Evaluate Req Context Done

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/railway-proxy`

Applies to:
- `**` (whole root)

## Context

- Reverse proxy services routing client traffic to external or remote upstream endpoints encounter transient network connectivity faults during TCP dial establishment.
- The standard library httputil.ReverseProxy delegates upstream dispatch to an http.RoundTripper implementation that does not execute retry loops on network failures by default.
- HTTP request payloads are streaming readers that are consumed upon initial transmission, requiring explicit buffering and reconstruction to enable multiple dispatch attempts.
- Unconditional retries across all error types risk duplicating non-idempotent requests if failures occur after the upstream endpoint has begun processing the request.

## Problem Statement

Reverse proxy dispatch to external upstream services experiences transient dial failures during connection establishment. Standard proxy transports consume request streams on first write, preventing automatic re-dispatch on transport errors. Without a dedicated transport layer that isolates body replay strictly to pre-connection dial errors and honors request context cancellations, transient connection faults result in immediate client request termination or duplicate operation execution.

## Decision

1. MUST: The retry loop MUST evaluate req.Context().Done() during inter-attempt backoff periods and terminate immediately if the client context is canceled or deadline exceeded.

## Policy Block

- MUST The retry loop MUST evaluate req.Context().Done() during inter-attempt backoff periods and terminate immediately if the client context is canceled or deadline exceeded.

In scope:
- Reverse proxy services utilizing httputil.ReverseProxy to route external client requests to upstream endpoints.
- Network gateway components requiring resilient connection establishment across network boundaries.

Out of scope:
- Direct internal client calls with built-in application-level idempotent transaction mechanisms.
- Unbuffered streaming or WebSocket connections where full body buffering is infeasible.

Exceptions:
- EXC-25-001: High-throughput streaming endpoints or large file uploads where in-memory buffering would exhaust proxy memory resources.

## Rationale

- Restricting retry eligibility strictly to isDialError guarantees that requests are re-sent only when connection establishment fails prior to upstream request processing, preserving idempotency for state-changing HTTP methods.
- Buffering request bodies into memory prior to transport execution enables deterministic body replay across retry attempts without requiring upstream protocol modifications.
- Integrating backoff waiting with req.Context().Done() prevents proxy resource leakage and hung execution threads when clients disconnect or timeout during transient network outages.

## Consequences

Positive:
- Isolates external clients from transient upstream connection establishment failures.
- Preserves HTTP idempotency guarantees by restricting retries strictly to dial errors occurring prior to request transmission.
- Prevents proxy resource leaks and stuck goroutines by tying retry backoff directly to request context cancellation.

Negative:
- Introduces heap memory allocation overhead per in-flight request proportional to buffered request body size.
- Increases downstream client latency on persistent upstream outages due to sequential retry backoff intervals.

## Alternatives

- Client-driven retry without reverse proxy transport retry (rejected)
  Rejected because: Shifts transient connection failure handling to external clients, increasing client integration complexity and error rates during network blips.
  When valid: When proxy memory constraints prohibit buffering request bodies or when client runtimes implement idempotent transaction protocols.
- Unconditional transport retries for all non-nil network and application errors (rejected)
  Rejected because: Risks duplicating non-idempotent operations if transport failures occur after an upstream service has accepted and partially processed a request.
  When valid: When every routed upstream endpoint is guaranteed to be strictly idempotent and read-only.

## Risks

- Heap memory exhaustion under high concurrent load with large request payloads due to in-memory buffering.
  Mitigation: Enforce strict maximum request body size limits at the routing gateway boundary prior to transport buffering.
  Owner: Platform Engineering Team
- Proxy connection pool exhaustion and thread saturation if upstream dial failures persist across multiple attempts.
  Mitigation: Configure small maximum retry counts and short backoff intervals coupled with upstream health checks and circuit breaking.
  Owner: Site Reliability Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Implementations wrapping httputil.ReverseProxy must assign the custom retry transport to the Transport field while ensuring rewrite or director handlers preserve target host headers.
- The incoming request body stream must be closed immediately after reading into a byte slice to prevent file descriptor leaks.

## Continuation Context


Verify commands:
- Discover and execute the project unit and integration test suites covering HTTP proxy transport behavior.
- Discover and run the project static analysis and linting checks to ensure proper error handling and context propagation.

Accept when:
- The custom transport retries requests upon simulated dial failure and delivers upstream response payloads upon successful reconnection.
- The custom transport terminates immediately without retry when encountering non-dial errors or when request context is canceled.
- All discovered automated test suites and static analysis checks pass with zero exit codes.

## Enforcement

- Verified by: Automated continuous integration pipeline running proxy integration and transport unit tests.
- Verified by: Peer code review verification of all reverse proxy transport configurations and routing boundaries.
- Violation handling: Pull requests introducing unbuffered proxy transports or unconstrained retry loops are rejected during code review.
- Violation handling: Transport implementations lacking context cancellation checks fail automated lint and static analysis gates.
- Exception process: Submit an architectural exception request detailing streaming payload characteristics and memory resource impact to the architecture review body for formal sign-off.