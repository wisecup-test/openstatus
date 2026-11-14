---
version: 2
name: 9b8e7085-0620-4a19-9b3c-d217440aeaf4-regional-checker-internal-api-dispatch-via-fetch-and-abortsignal-internal-worker-dispatch-requests-include-regional
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Worker Dispatch Requests Include Regional"
metadata:
  actual:
    adr_id: 9b8e7085-0620-4a19-9b3c-d217440aeaf4
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Worker Dispatch Requests Include Regional

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Backend route handlers and remote procedure call endpoints must coordinate monitoring and health-check executions across geographically distributed worker nodes.
- Internal service-to-service communication requires predictable regional routing, authenticated boundaries, and deterministic execution boundaries to prevent hung connections during worker stalls.
- Direct HTTP invocations across internal services must establish explicit timeout signals and structured payloads rather than relying on unbounded socket defaults.

## Problem Statement

Internal service invocations to distributed execution workers require standardized boundary encapsulation. Without explicit regional routing metadata, credential passing, and request cancellation signals, service-to-service calls risk routing to incorrect geographic worker pools, leaking unauthorized access across internal networks, or exhausting server connection pools when downstream workers experience latency or network partitions.

## Decision

1. MUST: Internal worker dispatch requests MUST include the regional affinity header specifying the target execution region alongside serialized JSON payloads.

## Policy Block

- MUST Internal worker dispatch requests MUST include the regional affinity header specifying the target execution region alongside serialized JSON payloads.

In scope:
- All internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes.
- Remote procedure call handlers triggering remote execution tasks across internal infrastructure boundaries.

Out of scope:
- Client-side browser requests originating from user interfaces.
- Database access queries executed through internal data access layers.

## Rationale

- Evidence across multiple route handlers and remote procedure call handlers demonstrates consistent use of fetch with AbortSignal.timeout, fly-prefer-region headers, and shared secret Authorization for internal worker coordination.
- Enforcing timeout bounds at the client boundary prevents connection pool exhaustion and cascading failures when regional workers face network degradation.
- Specifying regional routing headers directly at the client boundary guarantees that synthetic checks execute from their configured geographic vantage points.

## Consequences

Positive:
- Guarantees bounded latency for internal service invocations through mandatory AbortSignal timeouts.
- Ensures deterministic regional request routing to specific worker nodes.
- Standardizes internal authentication and payload conventions across disparate route handlers.

Negative:
- Requires manual configuration of timeout durations and region headers for each internal client invocation site.
- Imposes client-side abort errors that must be explicitly handled and mapped to domain status codes.

## Alternatives

- Unbounded HTTP client requests without AbortSignal cancellation (rejected)
  Rejected because: Omitting timeouts causes client connections to hang indefinitely when regional workers become unresponsive, leading to socket exhaustion in the API tier.
  When valid: Valid only in offline batch scripts where execution duration is externally governed by process supervisors.
- Asynchronous task queues with message broker dispatch (deferred)
  Rejected because: Adds infrastructure complexity and message broker operational overhead when immediate synchronous check results are required by callers.
  When valid: Valid when check execution can be processed fully asynchronously without immediate caller response requirements.

## Risks

- Premature request abortion if timeout limits are configured shorter than normal regional check execution duration.
  Mitigation: Derive timeout limits from target monitor configurations and maintain a sensible default safety margin.
  Owner: Backend Engineering Team
- Misconfigured regional headers routing traffic to invalid or unavailable infrastructure zones.
  Mitigation: Validate region identifiers against permitted infrastructure regions prior to dispatch.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct the signal parameter using AbortSignal.timeout configured with entity-specific timeout limits or system default limits.
- Ensure internal authorization headers and regional routing headers are applied consistently across all route and handler dispatch points.

## Continuation Context


Verify commands:
- Discover the project test runner from the repository configuration and execute the test suites covering internal client boundaries and route handlers.
- Discover the repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies.

Accept when:
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

## Enforcement

- Verified by: Automated test suites validating request headers, authorization, and timeout signals on internal client calls.
- Verified by: Code review verification of client boundary invocations in route and RPC handlers.
- Violation handling: Pull requests introducing unbounded fetch calls or missing regional routing headers will be blocked during automated review.
- Violation handling: Non-compliant client calls must be refactored to incorporate AbortSignal.timeout and required authentication headers.
- Exception process: Submit an architectural exception request detailing why an internal service invocation cannot support cancellation signals or regional routing headers.
- Exception process: Obtain formal approval from the backend engineering team before merging any exempt internal client calls.