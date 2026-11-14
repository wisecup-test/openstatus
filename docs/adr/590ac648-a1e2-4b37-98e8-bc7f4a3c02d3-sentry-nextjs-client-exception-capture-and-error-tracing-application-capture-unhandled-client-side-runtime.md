---
version: 2
name: 590ac648-a1e2-4b37-98e8-bc7f4a3c02d3-sentry-nextjs-client-exception-capture-and-error-tracing-application-capture-unhandled-client-side-runtime
description: "@sentry/nextjs Client Exception Capture and Error Tracing: Application Capture Unhandled Client Side Runtime"
metadata:
  actual:
    adr_id: 590ac648-a1e2-4b37-98e8-bc7f4a3c02d3
    status: accepted
    candidate_group_id: 0c5cef7f-e698-4627-b0e0-54cab25681c9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/trpc/**
---

# @sentry/nextjs Client Exception Capture and Error Tracing: Application Capture Unhandled Client Side Runtime

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`
- `src/lib/trpc/**`

## Context

- Client-side web applications encounter runtime exceptions during component rendering and asynchronous remote procedure call interactions.
- Uncaught client errors captured solely by console warnings fail to provide centralized operational visibility into production software defects.
- Standardizing error reporting using a unified SDK ensures uniform error ingestion and stack trace inspection across user interface boundaries and client data layers.

## Problem Statement

Client-side application runtime crashes and asynchronous remote procedure call errors lack centralized observability when recorded solely through local browser console mechanisms, preventing rapid triage and resolution of production user defects.

## Decision

1. MUST: The application MUST capture all unhandled client-side runtime errors and failed remote procedure calls using @sentry/nextjs Sentry.captureException for centralized exception tracing.

## Policy Block

- MUST The application MUST capture all unhandled client-side runtime errors and failed remote procedure calls using @sentry/nextjs Sentry.captureException for centralized exception tracing.

In scope:
- All client-side user interface error boundary components.
- Shared client-side RPC and API communication error handlers.

Out of scope:
- Server-side background worker tasks that do not utilize client telemetry bindings.
- Standard non-exceptional application control flow and anticipated form validation logic.

## Rationale

- Standardizing on @sentry/nextjs via Sentry.captureException provides structured error logging with stack trace capture across web application error states.
- Injecting endpoint metadata into error captures enriches RPC client failure diagnostics without requiring custom distributed tracing middleware.
- Consistent adoption across both route-level error boundaries and root-level error boundaries guarantees that unhandled rendering faults are reliably tracked.

## Consequences

Positive:
- Guarantees centralized visibility into runtime exceptions occurring in client application error boundaries.
- Correlates asynchronous RPC client communication failures with specific query and mutation endpoint identifiers.
- Enables automated alerting, error deduplication, and stack trace inspection across client and routing boundaries.

Negative:
- Adds external network transmission overhead during client error states.
- Requires rigorous sanitization of contextual metadata attached to error payloads to prevent accidental transmission of sensitive data or personally identifiable information.
- Introduces operational coupling to external telemetry ingestion availability and vendor quota constraints.

## Alternatives

- Native Console Logging Only (rejected)
  Rejected because: Console logging records failures only within local user browser sessions without centralized aggregation, alerting, or stack-trace grouping.
  When valid: Isolated local development or lightweight diagnostic prototyping environments without production telemetry infrastructure.
- Custom Centralized HTTP Error Ingestion Endpoint (rejected)
  Rejected because: Requires developing and maintaining bespoke client reporting protocols, retry buffering, source-map symbolication, and stack trace aggregation pipelines.
  When valid: Environments with strict data sovereignty mandates prohibiting external error monitoring software-as-a-service providers.

## Risks

- Sensitive user data or tokens could leak into error tracking platforms via unscrubbed exception metadata.
  Mitigation: Configure client-side telemetry scrubbing hooks to remove credentials, tokens, or personal identifiers before event transmission.
  Owner: engineering team
- High-frequency client error loops may consume external ingestion quotas and degrade client performance.
  Mitigation: Implement client-side sample rate limits and rate limiting rules within the telemetry configuration.
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
- Ensure error boundaries execute Sentry.captureException inside client lifecycle hooks to capture errors after initial rendering failure.
- Include relevant transport or routing metadata in the extra payload when capturing client-side RPC invocation errors.

## Continuation Context


Verify commands:
- Discover and execute the project linting and static analysis scripts to verify adherence to error logging standards.
- Discover and run the project automated test suite to ensure error boundary reporting behavior is validated.

Accept when:
- All client-side error boundary components invoke Sentry.captureException within error lifecycle hooks.
- Client-side RPC error dispatchers invoke Sentry.captureException with associated route extra metadata before propagation or warning.
- Static analysis checks and test suites confirm that unhandled client exceptions report to Sentry without missing contextual payloads.

## Enforcement

- Verified by: Automated continuous integration pipeline analyzing source files for required exception reporting calls.
- Verified by: Mandatory peer review on modifications to shared communication layers and error boundary components.
- Violation handling: Pull requests missing Sentry error capture in error boundaries or client RPC handlers are blocked from merge.
- Violation handling: Static analysis warnings must be resolved prior to release tag creation.
- Exception process: Exceptions require submission of an architectural variance request detailing alternative telemetry mechanisms or justifying offline-only operation.
- Exception process: Formal review and approval by the observability team prior to merging.