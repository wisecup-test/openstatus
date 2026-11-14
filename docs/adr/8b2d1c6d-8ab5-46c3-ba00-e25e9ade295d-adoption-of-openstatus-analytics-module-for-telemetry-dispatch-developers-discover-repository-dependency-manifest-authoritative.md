---
version: 2
name: 8b2d1c6d-8ab5-46c3-ba00-e25e9ade295d-adoption-of-openstatus-analytics-module-for-telemetry-dispatch-developers-discover-repository-dependency-manifest-authoritative
description: "Adoption of @openstatus/analytics Module for Telemetry Dispatch: Developers Discover Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 8b2d1c6d-8ab5-46c3-ba00-e25e9ade295d
    status: accepted
    candidate_group_id: db333959-f17e-4609-93f0-608d127c3a05
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/analytics Module for Telemetry Dispatch: Developers Discover Repository Dependency Manifest Authoritative

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`

## Context

- Playground API route handlers process user requests and produce diagnostic reports that require asynchronous telemetry tracking.
- Multiple route handlers import and initialize @openstatus/analytics using setupAnalytics and analytics.track within background post-response lifecycle blocks.
- Telemetry failures must be isolated from primary request handling so that network or persistence anomalies do not disrupt response delivery.

## Problem Statement

API route handlers require a standardized, decoupled mechanism for recording user interaction and operational telemetry without impacting response latency or coupling individual routes to downstream telemetry sinks.

## Decision

1. MUST: Developers MUST discover the repository dependency manifest and authoritative lock artifact to verify the exact resolved version of @openstatus/analytics and its peer dependencies before implementing or modifying analytics integration.

## Policy Block

- MUST Developers MUST discover the repository dependency manifest and authoritative lock artifact to verify the exact resolved version of @openstatus/analytics and its peer dependencies before implementing or modifying analytics integration.

In scope:
- API route handlers requiring asynchronous telemetry and event instrumentation.

Out of scope:
- Internal utility functions and pure algorithmic logic that do not handle HTTP requests or boundary telemetry.

## Rationale

- Adopting @openstatus/analytics standardizes client initialization through setupAnalytics and tracking through analytics.track across route handlers.
- Executing telemetry within post-response execution blocks isolates telemetry failures from request resolution and avoids adding network latency to client responses.
- Grounded evidence shows consistent usage across route handlers with explicit error isolation to protect operational stability.

## Consequences

Positive:
- Centralizes telemetry instrumentation and schema definitions across route handlers.
- Prevents telemetry dispatch failures from degrading user-facing API performance or crashing responses.
- Maintains consistent event tracking structures across playground API endpoints.

Negative:
- Requires route handlers to manage client initialization and execution asynchronously.
- Introduces a dependency on the internal shared analytics module across API entrypoints.

## Alternatives

- Inline telemetry reporting directly communicating with external telemetry endpoints within each route handler (rejected)
  Rejected because: Couples individual route handlers directly to external network endpoints, duplicates configuration across handlers, and prevents unified event schema enforcement.
  When valid: Valid only in isolated prototype scripts where internal shared modules cannot be resolved.
- Synchronous telemetry tracking blocking HTTP response delivery (rejected)
  Rejected because: Introduces network latency to client-facing response times and risks failing client requests if the telemetry destination experiences degraded availability.
  When valid: Valid only when strict transactional auditing requires telemetry persistence prior to returning an HTTP response.

## Risks

- Uncaught exceptions within asynchronous telemetry blocks could trigger unhandled promise rejections.
  Mitigation: Enforce explicit error trapping and logging around analytics client initialization and dispatch calls.
  Owner: engineering team
- Discrepancies between resolved package versions in lock files and module imports could cause runtime initialization errors.
  Mitigation: Enforce lock-file version grounding before modifying or invoking module interfaces.
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
- Wrap telemetry client initialization and event tracking within post-response lifecycle handlers to avoid adding latency to critical-path HTTP responses.
- Provide fallback error logging when analytics tracking fails so that telemetry failures do not interrupt primary execution flow.

## Continuation Context


Verify commands:
- Discover the repository test runner script from the package configuration and execute the unit test suite covering route telemetry.
- Discover the workspace static analysis and type check script to verify that module imports and tracking event structures conform to the internal analytics schema.
- Discover and run the project code linter to verify compliance with exception handling and asynchronous execution rules.

Accept when:
- All unit and integration tests covering route telemetry pass with zero errors.
- Static type analysis verifies that all calls to setupAnalytics and analytics.track match the module signature without type suppression.
- Code analysis confirms that telemetry dispatches in route handlers are enclosed in exception-handling structures within post-response execution blocks.

## Enforcement

- Verified by: Continuous integration checks executing the project test suites and type checkers.
- Verified by: Code review verifying that telemetry dispatches occur within post-response execution blocks with appropriate error isolation.
- Violation handling: Continuous integration failure for type mismatches or unhandled promise rejections.
- Violation handling: Rejection of code changes that introduce direct unencapsulated telemetry calls or bypass the standard analytics module.
- Exception process: Submit an architectural review request detailing the necessity of an alternative telemetry dispatch mechanism.