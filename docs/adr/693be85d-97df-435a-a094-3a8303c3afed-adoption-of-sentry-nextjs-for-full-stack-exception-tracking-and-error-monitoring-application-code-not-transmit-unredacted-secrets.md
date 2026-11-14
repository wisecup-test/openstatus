---
version: 2
name: 693be85d-97df-435a-a094-3a8303c3afed-adoption-of-sentry-nextjs-for-full-stack-exception-tracking-and-error-monitoring-application-code-not-transmit-unredacted-secrets
description: "Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Code Not Transmit Unredacted Secrets"
metadata:
  actual:
    adr_id: 693be85d-97df-435a-a094-3a8303c3afed
    status: accepted
    candidate_group_id: e73b9b92-e034-4340-9b15-8297af91da59
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Code Not Transmit Unredacted Secrets

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/**`

## Context

- Modern multi-tier web applications execute code across heterogeneous runtimes including browser clients, Node.js servers, and edge environments, making centralized error tracking necessary.
- Runtime exceptions and communication failures require consistent diagnostic metadata, stack trace resolution, and telemetry ingestion.
- Framework-level build and runtime lifecycle hooks necessitate integrated error monitoring modules across compilation and execution phases.

## Problem Statement

Heterogeneous web application runtimes spanning browser clients, Node.js serverless functions, and edge compute nodes risk fragmented error tracking and unobserved runtime failures when relying on ad hoc or runtime-specific logging mechanisms.

## Decision

1. MUST_NOT: Application code MUST NOT transmit unredacted secrets, credentials, or sensitive authentication tokens within exception context payloads.

## Policy Block

- MUST_NOT Application code MUST NOT transmit unredacted secrets, credentials, or sensitive authentication tokens within exception context payloads.

In scope:
- All application modules executing within browser, edge, and server runtimes requiring exception tracking and error observability.
- Shared API routing and communication layers handling remote procedure errors and cron triggers.

Out of scope:
- External standalone workers and non-framework processes that do not execute within the web application environment.
- Static asset builds and mock test suites that do not execute runtime application logic.

Exceptions:
- EXC-20-001: Local development or isolated test environments where telemetry ingestion is intentionally disabled

## Rationale

- The presence of @sentry/nextjs across server, edge, client, and configuration files demonstrates a deliberate architectural decision to standardize full-stack observability on a single SDK.
- Explicit invocations of Sentry.captureException within API communication handlers and top-level error boundaries provide structured diagnostic telemetry with context metadata.
- Integrating instrumentation across build and runtime tiers ensures consistent exception reporting and source mapping across all execution environments.

## Consequences

Positive:
- Provides unified exception capture and tracing context across server, edge, and browser execution environments.
- Enables automated capture of uncaught runtime errors at top-level UI and framework error boundary levels.
- Standardizes structured diagnostic metadata enrichment through Sentry.captureException in shared API and communication layers.

Negative:
- Couples client, edge, and server application runtimes to an external observability vendor SDK and data transmission pipeline.
- Introduces runtime overhead and bundle size expansion across client and server execution contexts.
- Requires continuous maintenance of authentication tokens, ingestion endpoints, and runtime environment variable configurations across environments.

## Alternatives

- Ad hoc logging with console statements and custom error handling without a centralized SDK (rejected)
  Rejected because: Lacks cross-runtime telemetry, source-mapped stack traces, and unified event correlation across client, server, and edge environments.
  When valid: Minimal prototypes or isolated scripts where external network egress and third-party error ingestion are prohibited.
- Generic Node.js and browser error logging libraries decoupled from framework runtime lifecycle hooks (rejected)
  Rejected because: Requires manual instrumentation of framework-specific build pipelines, edge middleware, and serverless lifecycle entry points.
  When valid: Non-framework runtime services that do not target multi-tier web application architectures.

## Risks

- Accidental transmission of sensitive user data or environment secrets within error metadata payloads.
  Mitigation: Sanitize error contexts and apply ingestion filters to ensure sensitive data and authentication credentials are scrubbed before transmission.
  Owner: engineering team
- Network latency spikes or telemetry ingestion quota exhaustion caused by high-frequency repetitive errors.
  Mitigation: Implement rate limiting, error sampling, and client-side error filtering to constrain outgoing event volumes.
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
- Discover configuration entry points for each runtime tier and ensure client, server, and edge environments initialize monitoring modules consistently.
- Enrich Sentry.captureException calls with operational context such as invocation paths and operational parameters while ensuring sensitive credentials remain excluded.

## Continuation Context


Verify commands:
- # Discover and execute project validation suites to ensure error tracking configuration validity
npm test || pnpm test || yarn test || true
- # Inspect repository manifests and lockfiles to confirm resolved library version consistency
grep -E "@sentry/nextjs" package.json */package.json 2>/dev/null || true

Accept when:
- All application runtime tiers initialize error tracking via @sentry/nextjs configuration modules without build or runtime exceptions.
- Unhandled exceptions in UI boundaries and API procedures invoke Sentry.captureException with associated diagnostic metadata.
- Lock-version verification verifies that the resolved version of @sentry/nextjs matches repository constraints and approved documentation.

## Enforcement

- Verified by: Automated static analysis and code review checks confirming proper import and initialization of @sentry/nextjs across entry points.
- Verified by: Continuous integration pipeline validation confirming configuration and build-time instrumentation completion.
- Violation handling: Build and continuous integration pipelines fail if runtime entry points lack required error monitoring configurations.
- Violation handling: Pull requests introducing unhandled API error paths or bypassing standardized exception reporting are blocked pending remediation.
- Exception process: Submit an architectural exception request documenting alternative exception capture mechanisms or compliance constraints.
- Exception process: Obtain formal review and sign-off from the technical lead prior to deploying unmonitored services.