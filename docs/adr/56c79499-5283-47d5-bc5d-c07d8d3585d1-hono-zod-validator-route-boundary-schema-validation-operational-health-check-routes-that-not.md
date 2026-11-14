---
version: 2
name: 56c79499-5283-47d5-bc5d-c07d8d3585d1-hono-zod-validator-route-boundary-schema-validation-operational-health-check-routes-that-not
description: "@hono/zod-validator Route Boundary Schema Validation: Operational Health Check Routes That Not"
metadata:
  actual:
    adr_id: 56c79499-5283-47d5-bc5d-c07d8d3585d1
    status: accepted
    candidate_group_id: df547031-70ee-405b-a1af-65a995e2e494
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# @hono/zod-validator Route Boundary Schema Validation: Operational Health Check Routes That Not

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/screenshot-service`

Applies to:
- `src/**`

## Context

- The service defines HTTP ingress routes that receive external requests and trigger asynchronous tasks, storage operations, and database updates.
- Incoming HTTP payloads must be validated against expected schema contracts before downstream processing begins.
- The codebase integrates route definitions with boundary validation middleware to reject malformed inputs at the transport boundary.

## Problem Statement

HTTP service endpoints that process external payloads without boundary validation risk executing downstream operations against malformed or invalid inputs, leading to runtime failures and inconsistent data persistence.

## Decision

1. SHOULD: Operational health check routes that do not accept request payloads SHOULD be defined without payload validation middleware.

## Policy Block

- SHOULD Operational health check routes that do not accept request payloads SHOULD be defined without payload validation middleware.

In scope:
- HTTP route handlers defining public or inter-service API boundaries accepting structured input payloads.

Out of scope:
- Internal utility functions and domain service methods operating entirely within service internals.
- Health check routes that process no request payload.

## Rationale

- Using @hono/zod-validator ensures that payload validation occurs before route handler execution, enforcing an architectural boundary against malformed inputs.
- Declarative route middleware simplifies maintenance by centralizing schema definition and validation error responses at the service boundary.

## Consequences

Positive:
- Guarantees runtime payload validation and type safety prior to invoking business logic and persistence updates.
- Standardizes endpoint boundary declarations with declarative schema contracts at the routing layer.
- Prevents malformed requests from triggering downstream resource operations such as storage uploads or database mutations.

Negative:
- Tight coupling between route boundary declarations and the specific validation middleware library.
- Additional CPU overhead on ingress handling during schema parsing and validation of complex payloads.

## Alternatives

- Inline schema parsing within route handler bodies without validation middleware (rejected)
  Rejected because: Intermingles boundary schema validation with domain execution logic and duplicates validation error handling across individual endpoints
  When valid: Dynamic payload structures whose schemas depend on runtime state evaluated inside the handler
- Manual imperative type assertions and property checks on raw request inputs (rejected)
  Rejected because: Increases maintenance burden and creates vulnerabilities to unhandled schema divergence and runtime exceptions
  When valid: Lightweight isolated scripts where introducing dependency on schema validation libraries is prohibitive

## Risks

- Validation schema divergence between upstream callers and downstream service handlers.
  Mitigation: Maintain shared schema definitions across service boundaries to prevent schema drift between callers and consumers.
  Owner: Backend Engineering Team
- Performance degradation during ingress handling when parsing deep or complex payload schemas.
  Mitigation: Keep boundary validation schemas flat and bounded to minimize parsing overhead on high-throughput routes.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Bind validation middleware directly within route registration calls to ensure atomic schema enforcement at ingress.
- Expose dedicated status and operational routes independently of payload-validating route pipelines.

## Continuation Context


Verify commands:
- Discover the project build and type checking configuration to execute static analysis across all service boundary files.
- Discover the project test suite runner and execute automated boundary validation test suites against defined HTTP routes.

Accept when:
- All service endpoints reject invalid payloads with client error status codes prior to executing downstream tasks.
- Static type analysis verifies zero type mismatches between validated route payloads and downstream business logic handlers.

## Enforcement

- Verified by: Continuous integration checks executing static type validation and automated boundary test suites.
- Verified by: Peer review of endpoint route registrations and associated schema definitions.
- Violation handling: Pull requests introducing unvalidated HTTP ingress routes fail automated verification checks.
- Violation handling: Code review blocks merge until schema validation middleware is bound to the route definition.
- Exception process: Exceptions must be submitted via architectural review detailing why standard boundary middleware cannot be applied.