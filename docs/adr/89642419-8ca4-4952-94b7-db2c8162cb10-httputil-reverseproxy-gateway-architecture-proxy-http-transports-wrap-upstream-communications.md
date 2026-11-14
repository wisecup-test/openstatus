---
version: 2
name: 89642419-8ca4-4952-94b7-db2c8162cb10-httputil-reverseproxy-gateway-architecture-proxy-http-transports-wrap-upstream-communications
description: "httputil.ReverseProxy Gateway Architecture: Proxy Http Transports Wrap Upstream Communications"
metadata:
  actual:
    adr_id: 89642419-8ca4-4952-94b7-db2c8162cb10
    status: accepted
    candidate_group_id: 4f5b9518-22b8-4be4-a515-5b19dc1988c1
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# httputil.ReverseProxy Gateway Architecture: Proxy Http Transports Wrap Upstream Communications

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/railway-proxy`

Applies to:
- `**` (whole root)

## Context

- Reverse proxy components route inbound requests across service boundaries and require resilient connection handling when dialing remote backends.
- Transient network partitions or dial failures to backend hosts cause unhandled connection drops unless retry mechanics are integrated at the transport layer.
- HTTP request bodies are non-reusable streams by default, requiring in-memory buffering when retrying requests across failed dial attempts.

## Problem Statement

Direct proxy forwarding without retry handling drops inbound client traffic during transient dial failures, while naive retries fail because consumed HTTP request streams cannot be re-read without buffering.

## Decision

1. MUST: Proxy HTTP transports MUST wrap upstream communications in a dedicated RoundTrip implementation that buffers incoming request bodies and handles dial failure retries with exponential backoff and context cancellation.

## Policy Block

- MUST Proxy HTTP transports MUST wrap upstream communications in a dedicated RoundTrip implementation that buffers incoming request bodies and handles dial failure retries with exponential backoff and context cancellation.

In scope:
- Reverse proxy and API gateway services routing inbound traffic to upstream backends.
- Network perimeter endpoints requiring transport-level dial retry resilience.

Out of scope:
- Direct internal service-to-service RPC calls managed by mesh proxies.
- Non-HTTP streaming protocols where payload replay is unsupported.

Exceptions:
- EXC-24-001: Streaming upload endpoints where payload size exceeds buffering memory thresholds.

## Rationale

- Configuring httputil.ReverseProxy with a custom Rewrite function guarantees standardized host and target URL normalization across inbound routes.
- Encapsulating retry logic within an http.RoundTripper implementation isolates dial resilience from routing declarations.
- Buffering request bodies specifically for dial errors avoids premature failure without retrying non-idempotent application errors.

## Consequences

Positive:
- Protects clients from transient upstream dial failures via transparent exponential backoff retries.
- Centralizes header inspection and destination rewrite rules within the proxy gateway boundary.
- Separates health and diagnostic endpoints from reverse proxy pipeline execution.

Negative:
- Consumes heap memory by buffering full request payloads in memory to enable retry replay.
- Increases latency for requests experiencing upstream dial failures due to retry backoff intervals.

## Alternatives

- Standard library default ReverseProxy transport without dial retries (rejected)
  Rejected because: Drops active requests during transient network dial failures without recovery opportunities.
  When valid: Valid when upstream connections are guaranteed persistent or backed by infrastructure-level service meshes.
- Application middleware-level retry handler (rejected)
  Rejected because: Couples retry handling to HTTP handler abstractions rather than transport dial operations.
  When valid: Valid when retries depend on HTTP response status codes rather than low-level socket dial failures.

## Risks

- Unbounded request body buffering causing memory exhaustion under high payload volumes.
  Mitigation: Enforce maximum request body read limits and bypass buffering for designated streaming endpoints.
  Owner: Gateway Engineering Team
- Retry amplification under sustained upstream outages exhausting gateway worker goroutines.
  Mitigation: Strict maximum retry counts bounded by context timeout propagation.
  Owner: Gateway Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure request body readers are reset using buffer readers prior to each retry iteration within RoundTrip.
- Propagate context cancellation checks prior to initiating backoff pauses to prevent lingering goroutines during client disconnects.

## Continuation Context


Verify commands:
- Discover and run the project's gateway routing and proxy integration test suites.
- Discover and execute the repository static analysis and verification scripts to confirm reverse proxy transport configurations.

Accept when:
- Reverse proxy tests verify that upstream target URLs and Host headers are rewritten correctly.
- Transport tests verify that dial failures trigger retry attempts with backoff while non-dial errors fail immediately without retry.
- Context cancellation aborts retry loops without executing subsequent RoundTrip calls.

## Enforcement

- Verified by: Automated integration test suites executed in continuous integration pipelines.
- Verified by: Peer code review for changes to gateway routing and transport definitions.
- Violation handling: Pull requests lacking required dial error checks or retry body restoration are blocked from merging.
- Violation handling: Unbounded buffering without streaming exceptions requires architectural revision.
- Exception process: Submit an architectural review request outlining streaming constraints and memory profile justifications.