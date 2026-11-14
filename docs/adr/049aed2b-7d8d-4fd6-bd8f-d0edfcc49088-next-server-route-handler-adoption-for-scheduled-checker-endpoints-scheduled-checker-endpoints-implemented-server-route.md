---
version: 2
name: 049aed2b-7d8d-4fd6-bd8f-d0edfcc49088-next-server-route-handler-adoption-for-scheduled-checker-endpoints-scheduled-checker-endpoints-implemented-server-route
description: "next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Checker Endpoints Implemented Server Route"
metadata:
  actual:
    adr_id: 049aed2b-7d8d-4fd6-bd8f-d0edfcc49088
    status: accepted
    candidate_group_id: f2ddf12e-7bcf-45e4-a2e3-65606ce03474
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Route Handler Adoption for Scheduled Checker Endpoints: Scheduled Checker Endpoints Implemented Server Route

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/checker/cron/**`

## Context

- Periodic background tasks and health checks must run across multiple fixed cadence intervals to ensure system reliability.
- Serverless hosting architectures favor HTTP-triggered entry points over long-running background worker processes.
- A unified server framework module is required to define consistent request handling and response contracts across all scheduled routes.

## Problem Statement

Implementing recurring background checker tasks without a standardized dispatch pattern risks inconsistent failure handling, duplicated scheduling logic, and tight coupling between execution intervals and task implementation. A uniform server route handler interface is needed to expose interval-based endpoints that delegate work to shared orchestration and telemetry modules.

## Decision

1. MUST: Scheduled checker endpoints MUST be implemented as server route handlers using the next/server module, delegating execution logic to the shared internal _cron module.

## Policy Block

- MUST Scheduled checker endpoints MUST be implemented as server route handlers using the next/server module, delegating execution logic to the shared internal _cron module.

In scope:
- HTTP route handlers designated for recurring checker tasks and scheduled cron intervals.
- Server-side endpoint handlers coordinating periodic background evaluation.

Out of scope:
- Interactive client-facing API routes serving synchronous user requests.
- Persistent background worker services operating outside HTTP route execution boundaries.

## Rationale

- Adopting next/server route handlers standardizes the HTTP interface for external scheduler triggers across all cadence intervals.
- Delegating task logic to a shared internal _cron module prevents duplication of execution harnesses and timeout handling.
- Coordinating error reporting through _sentry guarantees consistent diagnostic telemetry across all recurring execution cycles.

## Consequences

Positive:
- Centralizes recurring task orchestration, execution wrappers, and error handling in a single reusable internal module.
- Enables stateless interval triggering via HTTP requests without maintaining dedicated process supervision infrastructure.
- Standardizes error monitoring across all scheduling cadences through a common telemetry interface.

Negative:
- Requires external scheduler infrastructure to dispatch periodic HTTP invocations to trigger task execution.
- Execution duration is constrained by serverless HTTP route timeout boundaries established by the runtime environment.

## Alternatives

- Inline execution logic in independent interval route handlers (rejected)
  Rejected because: Duplication of task orchestration, error capture, and execution wrappers across cadences leads to configuration drift and inconsistent failure handling.
  When valid: One-off diagnostic routes with unique payload shapes and non-standard execution lifecycles.
- Persistent background daemon process for scheduling (rejected)
  Rejected because: Requires stateful process supervision and continuous host infrastructure incompatible with stateless serverless route execution.
  When valid: Sub-second continuous streaming tasks or dedicated message queue consumer loops requiring persistent socket connections.

## Risks

- Execution timeout if a scheduled checker run exceeds server route duration limits
  Mitigation: Isolate heavy processing into decoupled queue dispatches or tune route segment timeout configurations via next/server runtime options.
  Owner: engineering team
- Unauthenticated triggering of scheduled route endpoints by unauthorized external callers
  Mitigation: Enforce shared secret authorization headers or signature verification within the route handler wrapper.
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
- Configure each interval endpoint with a dedicated cadence path while routing execution through the common runner abstraction.
- Ensure error tracking contexts capture the originating interval identifier before flushing telemetry through the monitoring module.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis and typecheck suites to verify route handler compliance with next/server export contracts.
- Discover and run the project test suite to validate route dispatch and error handling behavior across scheduled intervals.

Accept when:
- All cadence-specific route handlers compile without type errors against the locked server module interfaces.
- Each scheduled route handler correctly delegates execution to the internal cron runner module and reports unhandled exceptions.
- Project verification test suites pass for all scheduled endpoint routes.

## Enforcement

- Verified by: Automated pull request continuous integration checks
- Verified by: Architectural code review
- Violation handling: Pull request rejection on inline runner logic or missing error instrumentation
- Violation handling: Blocking merge until routes conform to standard dispatch contracts
- Exception process: Submit an architectural review request documenting technical constraints requiring non-standard route handling.
- Exception process: Approval required from engineering leadership prior to merging bespoke scheduled endpoints.