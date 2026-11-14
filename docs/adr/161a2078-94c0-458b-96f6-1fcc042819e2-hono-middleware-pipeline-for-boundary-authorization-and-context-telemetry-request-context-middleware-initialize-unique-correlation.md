---
version: 2
name: 161a2078-94c0-458b-96f6-1fcc042819e2-hono-middleware-pipeline-for-boundary-authorization-and-context-telemetry-request-context-middleware-initialize-unique-correlation
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Request Context Middleware Initialize Unique Correlation"
metadata:
  actual:
    adr_id: 161a2078-94c0-458b-96f6-1fcc042819e2
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Request Context Middleware Initialize Unique Correlation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/**`

## Context

- Service boundaries in workflow applications expose HTTP endpoints for scheduling, incident resolution, and operational actions that require strict perimeter authorization.
- Inconsistent authentication handling across route handlers introduces security vulnerabilities and code duplication.
- Observability requirements necessitate capturing consistent request identifiers, timestamps, and wide-event telemetry across all incoming HTTP requests prior to handler dispatch.

## Problem Statement

Exposing workflow execution and maintenance endpoints without a standardized boundary interception layer leads to fragmented security validation, missing correlation metadata, and duplicated credential verification logic across individual route handlers.

## Decision

1. MUST: Request context middleware MUST initialize a unique correlation request identifier and contextual event state on the execution context before delegating to the next middleware in the pipeline.

## Policy Block

- MUST Request context middleware MUST initialize a unique correlation request identifier and contextual event state on the execution context before delegating to the next middleware in the pipeline.

In scope:
- HTTP route definitions, sub-routers, and workflow trigger endpoints exposed at service boundaries.
- Boundary endpoints requiring pre-flight secret authentication or contextual telemetry enrichment.

Out of scope:
- Internal shared library modules, database schema definitions, and non-HTTP asynchronous workers.

Exceptions:
- EXC-24-001: A route is explicitly designated as a public health check or diagnostic probe requiring no credential verification.

## Rationale

- The codebase demonstrates consistent usage of wildcard middleware registration across multiple service routes to enforce secret header matching and request-level telemetry context.
- Consolidating authorization checks into boundary middleware ensures unauthenticated requests are halted before entering core business logic.
- Chaining context-enrichment middleware guarantees uniform emission of canonical request logs across all executed routes.

## Consequences

Positive:
- Centralizes boundary authorization and context telemetry across route trees, eliminating boilerplate in endpoint handlers.
- Guarantees consistent request tracing and wide-event context capture across all incoming HTTP interactions.
- Provides a defense-in-depth enforcement mechanism against unauthorized invocations at the boundary level.

Negative:
- Couples route security policies directly to framework router hierarchy and registration order.
- Errors thrown within early middleware handlers can short-circuit downstream telemetry logging if not caught within error boundaries.

## Alternatives

- Per-endpoint manual authorization checks and context initialization (rejected)
  Rejected because: Introduces severe code duplication, increases the likelihood of omission on newly created routes, and complicates telemetry consistency.
  When valid: Only valid in isolated prototypes with a single route where a middleware pipeline is not supported.
- External reverse-proxy or gateway-only authentication and telemetry (rejected)
  Rejected because: Removes defense-in-depth within internal service processes, prevents fine-grained in-process request logging, and couples route access control to external deployment infrastructure.
  When valid: Valid when services reside entirely in private mesh environments with dedicated sidecar proxies terminating all perimeter credentials.

## Risks

- Developers might mount new route paths outside the middleware tree, bypassing authorization.
  Mitigation: Ensure route registration tests specifically verify that newly appended routes inherit the wildcard middleware pipeline.
  Owner: Engineering Team
- Unhandled exceptions in downstream handlers might bypass post-execution telemetry aggregation.
  Mitigation: Wrap downstream handler invocations in exception-handling structures within the middleware to capture and log error events.
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
- Middleware registration order must ensure that correlation identifiers and telemetry context wrappers precede authorization filters, ensuring rejected requests generate observable audit trails.
- Context variables such as request identifiers and wide event objects must be propagated using framework execution context storage rather than global state.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
- Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.

Accept when:
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

## Enforcement

- Verified by: Automated integration and contract tests verifying that unauthorized requests yield 401 Unauthorized responses.
- Verified by: Static analysis and peer review of route registration trees.
- Violation handling: Pull requests containing route definitions that bypass perimeter middleware or duplicate auth checks inside handlers are blocked during code review.
- Exception process: Exceptions for unauthenticated routes such as public health checks must be documented in code review and approved by the engineering security lead.