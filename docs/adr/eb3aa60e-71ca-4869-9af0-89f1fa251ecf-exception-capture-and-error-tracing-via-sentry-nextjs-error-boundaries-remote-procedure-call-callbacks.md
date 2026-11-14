---
version: 2
name: eb3aa60e-71ca-4869-9af0-89f1fa251ecf-exception-capture-and-error-tracing-via-sentry-nextjs-error-boundaries-remote-procedure-call-callbacks
description: "Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks"
metadata:
  actual:
    adr_id: eb3aa60e-71ca-4869-9af0-89f1fa251ecf
    status: accepted
    candidate_group_id: 0aa494d9-538b-4b0c-a5ed-a28a60aec767
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/trpc/**
---

# Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/trpc/**`

## Context

- Client-side runtime failures and remote procedure call communication errors require unified capture and alerting to prevent silent production outages.
- Standardizing on @sentry/nextjs across client error boundaries and remote procedure call client handlers enables centralized error tracing and operational visibility.
- Capturing procedure path metadata alongside client exceptions provides actionable diagnostic tracing for debugging remote client-server interactions.

## Problem Statement

Uncaught client runtime crashes and remote procedure call failures risk going unnoticed in production environments without automated aggregation, alerting, and contextual diagnostic metadata. A unified exception capturing strategy is required to capture stack traces and procedure paths without fragmenting error monitoring across disparate logging mechanisms.

## Decision

1. MUST_NOT: Error boundaries and remote procedure call error callbacks MUST NOT suppress or swallow uncaught exceptions without recording them through Sentry.captureException.

## Policy Block

- MUST_NOT Error boundaries and remote procedure call error callbacks MUST NOT suppress or swallow uncaught exceptions without recording them through Sentry.captureException.

In scope:
- All client-side global error boundary components handling uncaught runtime exceptions.
- All remote procedure call client communication error handling callbacks and link configurations.

Out of scope:
- Server-side internal utility functions with dedicated domain error handling and fallback logic.
- Expected business validation errors handled locally through user interface feedback states.

## Rationale

- The repository evidence demonstrates consistent adoption of @sentry/nextjs for client-side exception capture in both global error boundaries and RPC client error hooks.
- Attaching execution metadata such as procedure path to Sentry.captureException bridges frontend client state with backend service tracing during failure scenarios.
- Centralizing exception tracking on an established tracing SDK prevents silent failures and simplifies production debugging across client application boundaries.

## Consequences

Positive:
- Centralized aggregation and automated real-time alerting for uncaught application exceptions across frontend interfaces and RPC clients.
- Consistent diagnostic context across distributed client calls through standardized procedure path metadata in error reports.
- Early capture of fatal client runtime crashes directly within standard framework error boundaries before interface disruption propagates.

Negative:
- Network overhead introduced by transmitting exception payloads and stack traces from client runtimes to remote observability collectors.
- Potential risk of leaking sensitive user or payload data within extra context dictionaries unless sanitization is strictly enforced.
- Tight coupling of error boundaries and communication client error handlers to the specific error reporting client API.

## Alternatives

- Console logging only without an external error monitoring client (rejected)
  Rejected because: Local logging fails to aggregate client runtime exceptions, provides no operational alerting, and loses critical production stack traces and context.
  When valid: Isolated local development or minimal prototyping environments with no production deployment requirements.
- Generic window error event listeners without structured error boundary capture (rejected)
  Rejected because: Window-level listeners capture errors after React component state has unmounted, losing component stack traces and procedure context.
  When valid: Environments lacking component-level error boundaries or standardized client-server RPC transports.

## Risks

- Accidental inclusion of sensitive parameters or user tokens within the extra metadata dictionary.
  Mitigation: Implement client-side payload sanitization and data scrubbing before attaching extra metadata to error reports.
  Owner: Engineering Team
- Telemetry flooding or quota exhaustion caused by repeated error loops in client rendering.
  Mitigation: Configure appropriate sample rates and rate limiting within the telemetry client initialization.
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
- Invoke Sentry.captureException directly inside component lifecycle hooks or effect handlers dedicated to uncaught error capture.
- When handling remote procedure call client failures, pass structured metadata containing the target procedure path within the extra context argument.
- Ensure sensitive authentication credentials or personally identifiable information are omitted or scrubbed prior to dispatching exception metadata.

## Continuation Context


Verify commands:
- Discover the repository dependency manifest and lock artifact, and verify the resolved version of @sentry/nextjs.
- Discover and execute the project static analysis and lint command to verify compliance with Sentry.captureException rules across error boundaries and RPC clients.
- Discover and run the project test suite to verify that error boundary and procedure error handler test cases successfully dispatch error tracing calls.

Accept when:
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

## Enforcement

- Verified by: Static analysis scanning codebases to ensure error boundaries and RPC client error callbacks invoke the designated exception capture API.
- Verified by: Automated test suites validating that simulated client exceptions trigger expected error telemetry calls with contextual metadata.
- Verified by: Pull request peer reviews verifying that new remote procedure call handlers include contextual procedure paths.
- Violation handling: Continuous integration pipelines reject pull requests introducing error boundary catches or RPC error listeners that omit Sentry.captureException.
- Violation handling: Static analysis flags unhandled error boundary implementations or missing diagnostic extra fields as build-blocking violations.
- Violation handling: Engineering leads require remediation before approving deployments containing silent exception swallows.
- Exception process: Submit an architecture review request detailing technical constraints preventing standard exception capture in the specified boundary.
- Exception process: Document compensating observability measures such as secondary telemetry streams or structured server-side logging.
- Exception process: Obtain explicit approval from the core engineering team before merging code that omits standard exception reporting.