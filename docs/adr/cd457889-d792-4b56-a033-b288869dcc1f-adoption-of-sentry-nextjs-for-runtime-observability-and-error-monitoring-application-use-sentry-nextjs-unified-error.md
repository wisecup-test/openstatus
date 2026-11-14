---
version: 2
name: cd457889-d792-4b56-a033-b288869dcc1f-adoption-of-sentry-nextjs-for-runtime-observability-and-error-monitoring-application-use-sentry-nextjs-unified-error
description: "Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Application Use Sentry Nextjs Unified Error"
metadata:
  actual:
    adr_id: cd457889-d792-4b56-a033-b288869dcc1f
    status: accepted
    candidate_group_id: f9ac9ba3-1eee-4b96-86b5-30eaf058325a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Adoption of @sentry/nextjs for Runtime Observability and Error Monitoring: Application Use Sentry Nextjs Unified Error

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/**`

## Context

- Applications deployed across heterogeneous execution runtimes require a unified exception monitoring and telemetry capture mechanism.
- Unhandled runtime errors occurring in client rendering boundaries and remote procedure call transport pipelines need structured capture and contextual attribution.
- Disparate execution environments including server, edge, and browser client runtimes require coordinated observability configuration without fragmented tracking vendors.

## Problem Statement

Fragmented error monitoring across hybrid server, edge, and browser environments causes unhandled runtime failures to go unnoticed or lack diagnostic context. Without a standardized error tracking integration across all execution contexts, client-side crashes, transport failures, and backend exceptions are captured inconsistently, hindering incident resolution.

## Decision

1. MUST: The application MUST use @sentry/nextjs as the unified error tracking and observability library across client, server, and edge runtimes.

## Policy Block

- MUST The application MUST use @sentry/nextjs as the unified error tracking and observability library across client, server, and edge runtimes.

In scope:
- Client-side error boundaries and exception handling hooks
- Server-side and edge runtime error handling and instrumentation
- Remote procedure call client transport error handlers

Out of scope:
- Standard debug and informational console logging during local development
- Transient validation failures managed entirely within localized user interface components

Exceptions:
- EXC-20-001: Isolated offline testing environments where external telemetry endpoints are unreachable

## Rationale

- @sentry/nextjs provides comprehensive cross-runtime error monitoring covering browser, server, and edge environments with a consistent instrumentation model.
- Direct integration of Sentry.captureException within client error boundaries and remote procedure call client handlers ensures unhandled errors retain operational context.
- Centralizing error tracking into a single telemetry provider minimizes telemetry overhead and eliminates disparate monitoring silos across runtimes.

## Consequences

Positive:
- Consistent error tracking and alerting across server, edge, and client execution runtimes.
- Enriched diagnostic context through structured metadata attached to Sentry.captureException invocations.
- Centralized incident triage and reduced mean time to detection for production exceptions.

Negative:
- Coupling of runtime error handling and boundary components to the external telemetry SDK.
- Runtime overhead for capturing and transmitting telemetry payloads over the network.
- Operational requirement to manage dedicated telemetry ingestion configuration across environments.

## Alternatives

- Standard runtime console logging without external telemetry SDK integration (rejected)
  Rejected because: Lacks persistent aggregation, structured contextual tracing, edge runtime alerting, and real-time incident alerting
  When valid: Lightweight local scripts or command-line utilities without production service availability requirements
- Custom bespoke HTTP error reporting service (rejected)
  Rejected because: Requires ongoing infrastructure maintenance, custom SDK maintenance across edge and client runtimes, and high operational burden
  When valid: Strict air-gapped environments prohibiting third-party cloud telemetry services

## Risks

- Accidental exposure of sensitive operational data or user information in exception context payloads
  Mitigation: Configure data scrubbing and sanitize error parameters before invoking Sentry.captureException
  Owner: Engineering team
- Telemetry transmission failures or network timeouts degrading application performance
  Mitigation: Utilize non-blocking asynchronous transmission and local buffering provided by the SDK configuration
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure separate runtime telemetry initialization entrypoints for server, edge, and browser contexts while retrieving ingestion keys from environment variables.
- Wrap remote procedure call client listeners and top-level user interface error boundaries to dispatch unhandled exceptions with associated request path metadata.

## Continuation Context


Verify commands:
- # Discover and run the project linting and static analysis scripts from the repository manifest
- # Discover and run the project build script to verify multi-runtime compilation and instrumentation
- # Discover and run the project automated test suite to ensure error boundaries and handlers pass

Accept when:
- Static analysis and compilation complete across all target runtimes with zero errors.
- All error boundary and remote procedure call client tests pass successfully.
- Telemetry instrumentation initializes without throwing unhandled exceptions across server, edge, and client environments.

## Enforcement

- Verified by: Continuous integration automated static analysis and test validation pipelines.
- Verified by: Peer code review verifying adherence to error handling and telemetry capture rules.
- Violation handling: Pull requests failing error tracking instrumentation or introducing unauthorized telemetry SDKs are blocked from merging.
- Violation handling: Violations discovered in codebase reviews require remediation prior to production deployment.
- Exception process: Submit an architectural review request detailing the constraints preventing integration with the standard telemetry library.
- Exception process: Approval required from the principal architecture and observability leads prior to merging.