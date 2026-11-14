---
version: 2
name: 7b2cfa06-e94f-4c66-8748-b6d63502e611-sentry-nextjs-adoption-for-cross-runtime-error-tracking-and-observability-telemetry-instrumentation-not-capture-transmit-sensitive
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Telemetry Instrumentation Not Capture Transmit Sensitive"
metadata:
  actual:
    adr_id: 7b2cfa06-e94f-4c66-8748-b6d63502e611
    status: accepted
    candidate_group_id: 002e37f3-4d0a-434f-ae9a-10f3bc6c6348
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Telemetry Instrumentation Not Capture Transmit Sensitive

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/**`

## Context

- The application architecture executes across multiple distinct runtimes including client browsers, server processes, and edge workers, creating fragmented failure domains.
- Distributed communication channels and user interface rendering lifecycles require unified exception logging and tracing to diagnose production failures effectively.
- Standardizing on a dedicated telemetry library avoids disparate ad-hoc logging implementations and consolidates error alerting into a single monitoring infrastructure.

## Problem Statement

Managing application failures across heterogenous execution runtimes without unified error monitoring leads to unobserved crashes, inconsistent error telemetry, and delayed incident resolution. A coordinated exception tracking architecture is required to capture unhandled client rendering errors, edge execution issues, and remote procedure call failures.

## Decision

1. MUST_NOT: Telemetry instrumentation MUST NOT capture or transmit sensitive credentials, authorization tokens, or internal environment configuration values.

## Policy Block

- MUST_NOT Telemetry instrumentation MUST NOT capture or transmit sensitive credentials, authorization tokens, or internal environment configuration values.

In scope:
- Application user interface error boundaries and fallback handlers.
- Client, server, and edge execution initialization workflows.
- Remote procedure call client middleware and error handling handlers.

Out of scope:
- Third-party dependency internal execution stacks outside application boundary hooks.
- Static assets and styling definitions.
- Local development utility scripts not deployed to production environments.

## Rationale

- Adopting @sentry/nextjs provides dedicated hooks for framework lifecycles across client, server, and edge targets, ensuring coherent telemetry coverage.
- Centralizing error capture with Sentry.captureException inside shared remote procedure call interceptors and error boundaries guarantees uniform metadata attachment without proliferating ad-hoc logging.
- Consolidating monitoring into an established error tracking library eliminates custom telemetry plumbing while providing real-time alerting.

## Consequences

Positive:
- Provides consistent exception telemetry and trace correlation across client, server, and edge execution contexts.
- Automates capture of rendering anomalies and remote communication errors with contextual execution metadata.
- Reduces mean time to detection and resolution for defects occurring in distributed production environments.

Negative:
- Adds external dependency overhead to client-side bundles and edge worker deployment packages.
- Couples application error handling routines and error boundaries to the telemetry vendor API interface.
- Introduces operational risk of alert fatigue or quota exhaustion if high-frequency errors are not properly grouped or filtered.

## Alternatives

- Standard runtime console logging without external error aggregation (rejected)
  Rejected because: Console logging fails to aggregate errors centrally, provides no proactive alerting, and loses client-side failure diagnostics in production environments.
  When valid: Local isolated test sandboxes or standalone command-line scripts.
- Custom HTTP error dispatcher service (rejected)
  Rejected because: Building and maintaining bespoke cross-runtime telemetry collectors introduces substantial operational overhead and lacks advanced crash analytics.
  When valid: Strict air-gapped environments prohibiting third-party cloud telemetry providers.

## Risks

- Telemetry initialization failure causing uncaught runtime exceptions during application startup.
  Mitigation: Isolate initialization routines within runtime lifecycle hooks and ensure graceful fallbacks if configuration is unavailable.
  Owner: Engineering Team
- Inadvertent transmission of sensitive personal data or security credentials in error contexts.
  Mitigation: Implement sanitization transforms in telemetry configurations and conduct code reviews on error metadata attachments.
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
- Configure distinct initialization entry points to service client, server, and edge execution runtimes without leaking runtime-specific dependencies.
- Attach contextual operation metadata to Sentry.captureException calls within remote procedure call error callbacks to aid root cause analysis.

## Continuation Context


Verify commands:
- Discover the repository build script from the dependency manifest and execute the compilation pipeline to verify that telemetry instrumentation compiles without type errors across all runtime targets.
- Discover and run the project test suite to verify that error boundary components and remote procedure call handlers correctly execute exception capture calls.
- Discover the project static analysis configuration and run the repository linter to verify compliance with telemetry conventions and absence of direct console logging.

Accept when:
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

## Enforcement

- Verified by: Continuous integration automated test suite executions.
- Verified by: Automated linting and static analysis rules.
- Verified by: Peer code review for pull requests modifying error boundaries or network clients.
- Violation handling: Pull requests omitting required error capture calls in error boundaries will be blocked during review.
- Violation handling: Build failures triggered by missing or invalid telemetry configurations must be remediated prior to merging.
- Exception process: Submit an architectural review request detailing why error capture or telemetry initialization must be bypassed for a specific isolated component.