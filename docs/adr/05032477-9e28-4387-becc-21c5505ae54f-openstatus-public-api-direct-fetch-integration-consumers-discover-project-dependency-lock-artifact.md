---
version: 2
name: 05032477-9e28-4387-becc-21c5505ae54f-openstatus-public-api-direct-fetch-integration-consumers-discover-project-dependency-lock-artifact
description: "OpenStatus Public API Direct Fetch Integration: Consumers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 05032477-9e28-4387-becc-21c5505ae54f
    status: accepted
    candidate_group_id: 6f15e95c-2f35-4911-9ab8-93253015e0a7
    evidence:
      source: deep
  scope:
    root: packages/react
    paths:
      - src/**
---

# OpenStatus Public API Direct Fetch Integration: Consumers Discover Project Dependency Lock Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/react`

Applies to:
- `src/**`

## Context

- Client-side user interface widgets require real-time service health and status metrics to display operational availability.
- Deploying a dedicated backend proxy for unauthenticated public status endpoints adds infrastructure complexity and latency.
- Direct HTTP queries using standard platform fetch interfaces allow lightweight embedding of status indicators within frontend components.

## Problem Statement

Displaying real-time operational status within client components requires balance between implementation simplicity and operational overhead. Routing unauthenticated status queries through an internal backend proxy introduces unnecessary infrastructure and latency, whereas client-side integration must ensure stale status states are prevented without introducing external dependency bloat.

## Decision

1. MUST: Consumers MUST discover the project dependency lock artifact and verify the exact resolved version of any shared networking or rendering packages before implementation.

## Policy Block

- MUST Consumers MUST discover the project dependency lock artifact and verify the exact resolved version of any shared networking or rendering packages before implementation.

In scope:
- Standalone user interface components requiring direct client-side external service status reporting.

Out of scope:
- Internal authenticated service-to-service communication.
- Backend API integrations requiring credentialed gateway proxies.

## Rationale

- Directly querying the external public API via standard fetch avoids maintaining dedicated proxy routes for unauthenticated public data.
- Explicitly disabling cache via the no-cache directive ensures the widget reflects immediate health status rather than cached responses.

## Consequences

Positive:
- Eliminates intermediate backend proxy service dependencies and simplifies operational architecture for public status displays.
- Guarantees that clients receive immediate real-time availability status without intermediary stale cache layers.

Negative:
- Couples client-side widget uptime and performance directly to external public API endpoint availability.
- Precludes centralized server-side response caching or shared request throttling across client sessions.

## Alternatives

- Server-side proxy gateway for external status queries (rejected)
  Rejected because: Introduces unnecessary backend infrastructure and maintenance overhead for unauthenticated public status metrics.
  When valid: When status data requires credential augmentation, tenant aggregation, or strict centralized rate limiting.
- Dedicated third-party client SDK integration (rejected)
  Rejected because: Increases client bundle overhead and external dependency surface for a single unauthenticated HTTP endpoint.
  When valid: When advanced subscription features such as real-time bi-directional streaming are required.

## Risks

- External public API rate limiting or downtime directly impacts client widget rendering.
  Mitigation: Implement client-side timeout thresholds and graceful fallback UI states when external requests fail.
  Owner: Frontend Engineering
- Uncached requests generate redundant network traffic during frequent component re-renders.
  Mitigation: Debounce requests and isolate fetching triggers to controlled component lifecycle events.
  Owner: Frontend Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure client-side fetch handlers normalize transport exceptions and non-success HTTP status codes into deterministic fallback states for UI presentation.
- Parametrize dynamic endpoint path segments such as status slugs through component properties rather than hardcoded identifiers.

## Continuation Context


Verify commands:
- Discover and execute the project automated test script defined in the workspace manifest to validate widget fetching behavior.
- Discover and execute the repository static analysis script to verify HTTP cache configuration constraints.

Accept when:
- Automated test suites confirm client-side widget requests target the expected public endpoint with cache set to no-cache.
- Widget gracefully handles simulated external endpoint failure without throwing unhandled runtime exceptions.

## Enforcement

- Verified by: Automated pull request test suites and static analysis checks
- Verified by: Peer code review during component integration
- Violation handling: Pull requests introducing unapproved server proxies or missing cache control policies for public status queries are blocked from merging.
- Exception process: Architecture review submission detailing security or compliance requirements that mandate server-side proxy mediation.