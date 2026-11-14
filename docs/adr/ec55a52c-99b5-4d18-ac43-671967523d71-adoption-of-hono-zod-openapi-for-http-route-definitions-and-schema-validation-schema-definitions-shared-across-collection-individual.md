---
version: 2
name: ec55a52c-99b5-4d18-ac43-671967523d71-adoption-of-hono-zod-openapi-for-http-route-definitions-and-schema-validation-schema-definitions-shared-across-collection-individual
description: "Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Schema Definitions Shared Across Collection Individual"
metadata:
  actual:
    adr_id: ec55a52c-99b5-4d18-ac43-671967523d71
    status: accepted
    candidate_group_id: 36ec8bb5-66f0-4c12-909f-e2501da343e4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @hono/zod-openapi for HTTP Route Definitions and Schema Validation: Schema Definitions Shared Across Collection Individual

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/**`

## Context

- The HTTP API server defines multiple route endpoints handling resource queries and mutations across domain entities.
- Route handlers require strict runtime validation for request parameters and response payloads while maintaining synchronized OpenAPI documentation.
- Static analysis reveals widespread adoption of @hono/zod-openapi across API route modules for endpoint registration, schema validation, and context extraction.

## Problem Statement

Without a unified routing and schema validation library, API route handlers risk schema drift between runtime data structures, compile-time types, and external API documentation, leading to contract violations and inconsistent context handling.

## Decision

1. SHOULD: Schema definitions SHOULD be shared across collection and individual entity route handlers using schema array transformations rather than duplicated inline definitions.

## Policy Block

- SHOULD Schema definitions SHOULD be shared across collection and individual entity route handlers using schema array transformations rather than duplicated inline definitions.

In scope:
- HTTP route handler modules and endpoint definitions within the API server
- Request and response validation pipelines for REST API services

Out of scope:
- Standalone background workers, scripts, or operational utilities operating outside HTTP request lifecycles
- Internal database schema definitions and data access layers

## Rationale

- The codebase establishes schema-driven endpoint handling across API route modules using @hono/zod-openapi to provide consistent runtime type validation and compile-time contract safety.
- Extracting contextual boundaries through context getters decouples route logic from ambient state, ensuring clean tenant isolation and predictable request handling.
- Parsing persistence layer outputs through structured schemas prevents leaking internal database columns or unformatted serialized attributes to external clients.

## Consequences

Positive:
- Guarantees strict runtime validation of all incoming requests and outgoing responses against defined schemas.
- Enables automated generation and synchronization of OpenAPI documentation directly from type-safe route definitions.
- Enforces clean isolation of request-scoped dependencies such as workspace context across all route handlers.

Negative:
- Introduces schema validation runtime overhead on every request and response serialization pass.
- Requires maintaining explicit schema transformations when persistence models diverge from public API contracts.

## Alternatives

- Standard Hono routing with decoupled manual schema validation (rejected)
  Rejected because: Lacks integrated OpenAPI contract generation and increases boilerplate by separating route registration from validation schemas.
  When valid: Valid for lightweight microservices or internal endpoints that do not publish public OpenAPI specifications.
- Imperative route handling with manual payload assertions and untyped context (rejected)
  Rejected because: Fails to enforce compile-time safety across endpoint boundaries and permits silent schema drift between internal models and external responses.
  When valid: Valid for experimental prototypes where rapid iteration takes precedence over schema stability.

## Risks

- Performance degradation resulting from redundant schema parsing and complex payload transformations on high-throughput endpoints.
  Mitigation: Benchmark serialization throughput and use lightweight schema transformations while avoiding deeply nested parsing loops.
  Owner: Backend Engineering
- Schema drift or breaking contract changes occurring when upstream dependencies update without lock validation.
  Mitigation: Enforce strict dependency lockfile grounding and comprehensive contract regression tests in continuous integration.
  Owner: API Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define reusable entity schemas centrally and use composition methods to derive collection schemas and partial update schemas across route handlers.
- Inject request-scoped dependencies through framework context middleware and retrieve them in route handlers via context accessors.

## Continuation Context


Verify commands:
- Discover the repository dependency manifest and lock artifact, and execute the project typecheck and test verification scripts to confirm route schemas compile without errors.
- Discover and run the project static analysis and linting scripts to verify compliance with route definition and schema parsing standards.
- Discover and run the project test execution script to verify that route handlers validate request parameters and response bodies according to defined contracts.

Accept when:
- All route handlers compile cleanly with complete type inference derived from declared schema definitions.
- API test suites pass with all endpoints rejecting invalid input payloads and returning schema-compliant responses.
- All contextual state access inside route handlers conforms to framework context getter patterns.

## Enforcement

- Verified by: Continuous integration static analysis and typechecking pipelines
- Verified by: Peer code review for all new and modified HTTP route handlers
- Verified by: Automated contract and schema validation test suites
- Violation handling: Pull requests violating schema validation or route conventions are blocked by continuous integration
- Violation handling: Non-compliant route implementations must be refactored to use standard route schemas and context accessors before merging
- Exception process: Submit an architectural exception request detailing the technical constraint preventing schema validation.
- Exception process: Obtain approval from the API architecture leads before deploying unvalidated or custom route handlers.