---
version: 2
name: b7667f43-0bfe-471b-9c60-455254fcc654-sentry-nextjs-exception-capture-and-boundary-tracing-invocations-sentry-captureexception-triggered-remote-procedure
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Invocations Sentry Captureexception Triggered Remote Procedure"
metadata:
  actual:
    adr_id: b7667f43-0bfe-471b-9c60-455254fcc654
    status: accepted
    candidate_group_id: a868f887-5323-4bac-b14e-89765e49358b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/trpc/**
---

# @sentry/nextjs Exception Capture and Boundary Tracing: Invocations Sentry Captureexception Triggered Remote Procedure

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/lib/trpc/**`

## Context

- Client applications require centralized exception tracking to detect and debug uncaught runtime errors in production environments.
- Remote procedure call failures and unhandled React component tree exceptions represent distinct failure modes that both require operational telemetry.
- Inconsistent error reporting leads to silent client failures and missing diagnostic context during incidents.

## Problem Statement

Production failures in client applications can occur silently without centralized observability unless uncaught user interface errors and network procedure call failures are intercepted and transmitted with contextual diagnostic metadata.

## Decision

1. SHOULD: Invocations of Sentry.captureException triggered by remote procedure call failures SHOULD attach contextual execution metadata including the request pathway under the structured extra context attribute.

## Policy Block

- SHOULD Invocations of Sentry.captureException triggered by remote procedure call failures SHOULD attach contextual execution metadata including the request pathway under the structured extra context attribute.

In scope:
- Global application error boundary components.
- Shared remote procedure call client error interceptors and links.

Out of scope:
- Internal utility functions with localized recovery logic.
- Unit test suites mocking network client transport.

## Rationale

- Directly dispatching exceptions via Sentry.captureException from centralized client boundaries ensures that unhandled UI crashes and API communication errors are consistently reported.
- Enriching telemetry payloads with execution pathway metadata enables engineering teams to correlate failures with specific application routes and procedure endpoints.

## Consequences

Positive:
- Centralizes runtime exception tracking across both UI rendering failures and remote procedure call errors.
- Attaches structured execution path metadata to operational alerts, accelerating incident diagnosis.
- Ensures client-side rendering failures are captured even when user interface rendering halts.

Negative:
- Couples client runtime boundaries and network interceptor logic directly to the external telemetry SDK.
- Requires disciplined sanitization of metadata to prevent unintentional capture of sensitive information in error extras.

## Alternatives

- Relying solely on local console warning streams for client error reporting (rejected)
  Rejected because: Console logs do not persist across user sessions and provide no centralized aggregation or alerting for production triage.
  When valid: During early local prototyping prior to remote environment deployment.
- Generic uncontextualized exception capturing across all application layers (rejected)
  Rejected because: Capturing exceptions without request or component route metadata impairs debugging and root-cause isolation.
  When valid: When operational constraints prohibit capturing contextual metadata due to strict payload size limitations.

## Risks

- Failure in telemetry transport or SDK invocation could disrupt error boundary recovery rendering.
  Mitigation: Ensure error boundary handlers fail safely and do not throw additional exceptions if the telemetry network transport fails.
  Owner: engineering team
- Inadvertent inclusion of sensitive data or personally identifiable information within error context metadata.
  Mitigation: Sanitize and restrict captured extra metadata fields to operational route identifiers rather than user-provided request parameters.
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
- Configure boundary error capture inside client lifecycle hooks to prevent rendering loops while ensuring uncaught render errors are dispatched.
- Pass operational metadata such as invocation paths within the extra field rather than serializing sensitive raw payloads.

## Continuation Context


Verify commands:
- Discover the repository script runner and execute the static analysis check for tracing boundary compliance.
- Run the test suite target responsible for validating client error boundary exception dispatching.

Accept when:
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

## Enforcement

- Verified by: Automated static analysis checks enforcing telemetry dispatch in boundary handlers.
- Verified by: Peer code review of changes touching application error boundaries or remote client link layers.
- Violation handling: Automated build checks fail if designated global error boundaries or client links catch exceptions without invoking telemetry capture.
- Exception process: Submit an architectural exception request documenting alternative telemetry or isolated sandboxing requirements for review by the platform team.