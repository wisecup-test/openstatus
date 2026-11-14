---
version: 2
name: 80b2216f-0f62-4044-86da-5ed973adc9b3-adoption-of-openstatus-db-module-for-shared-schema-and-persistence-route-handlers-generating-dynamic-endpoints-metadata
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata"
metadata:
  actual:
    adr_id: 80b2216f-0f62-4044-86da-5ed973adc9b3
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Route Handlers Generating Dynamic Endpoints Metadata

Status: proposed
Date: 2025-02-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`

## Context

- Applications within the workspace require direct access to domain data entities and relational schema structures when serving dynamic content and metadata.
- Distributing duplicate database schemas or ad-hoc database client initializations across separate application directories leads to schema drift and inconsistent connection pooling.
- The project established @openstatus/db and @openstatus/db/src/schema as the unified workspace module for database client connections and entity schema definitions.
- Route endpoints and metadata generation handlers import @openstatus/db alongside web runtime utilities to query persistent data directly.

## Problem Statement

Applications requiring direct access to domain persistence models risk schema drift, duplicate query abstractions, and inconsistent database client lifecycle management when database definitions are authored independently across application boundaries. A centralized module is required to standardize database connectivity, provide canonical schema definitions, and enforce a single source of truth for persistent entity models across all consuming applications.

## Decision

1. SHOULD: Route handlers generating dynamic endpoints or metadata SHOULD resolve incoming request boundaries via runtime header utilities before querying @openstatus/db with tenant-scoped parameters.

## Policy Block

- SHOULD Route handlers generating dynamic endpoints or metadata SHOULD resolve incoming request boundaries via runtime header utilities before querying @openstatus/db with tenant-scoped parameters.

In scope:
- Server-rendered endpoints, route handlers, and metadata generation modules requiring access to persistent domain data.
- Workspace packages and applications that execute database queries or consume shared entity schema definitions.

Out of scope:
- Client-rendered components and browser-executed scripts with no direct database connectivity.
- Pure utility libraries that operate independently of persistence structures and domain schemas.

## Rationale

- Centralizing database connectivity and schema definitions within @openstatus/db establishes a single authoritative source of truth for all domain entities across workspace applications.
- Importing schema definitions from @openstatus/db/src/schema prevents entity structure drift and guarantees compile-time type safety across independent server endpoints.
- Co-locating connection management within a shared workspace module enables unified connection lifecycle handling across diverse application entry points.

## Consequences

Positive:
- Guarantees schema consistency and compile-time type checking across all applications consuming persistent domain entities.
- Eliminates duplicate database client instantiations and redundant schema declarations across workspace packages.
- Simplifies schema migrations and entity evolution by isolating structural definitions to a single package.

Negative:
- Couples server endpoints directly to the workspace database package, requiring synchronized builds when schema definitions change.
- Increases the blast radius of schema alterations across all consuming routes and metadata handlers.

## Alternatives

- Independent local schema definitions per application (rejected)
  Rejected because: Creating independent schemas per application leads to entity drift, duplicate maintenance overhead, and runtime type divergence across workspace boundaries.
  When valid: Valid only in fully decoupled polyglot microservices with independently managed datastores.
- Remote API gateway isolation for all database interactions (rejected)
  Rejected because: Introducing an intermediary service layer for simple metadata generation and server routes adds unnecessary operational complexity and network latency.
  When valid: Valid when applications run across heterogeneous infrastructure without direct network access to the primary database.

## Risks

- Breaking changes in @openstatus/db/src/schema propagate compile errors to all consuming application endpoints.
  Mitigation: Employ automated type-checking across all workspace packages during continuous integration before merging schema updates.
  Owner: Engineering Team
- Exhaustion of database connection pools due to unmanaged client connections across multiple server routes.
  Mitigation: Encapsulate connection pooling logic and client reuse within @openstatus/db to prevent leaking connections.
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
- Consumers should import database client instances and schema structures through the primary export entry points provided by @openstatus/db rather than deep-linking into unexported internal files.
- When constructing query filters inside server route handlers, extract request parameters from request headers and pass them as sanitized inputs into queries constructed through the shared database module.

## Continuation Context


Verify commands:
- Discover the workspace dependency manifest and execute the project type-checking and linting suites across all consuming applications to verify that all @openstatus/db imports resolve without compilation errors.
- Locate and run the automated test suite for applications consuming @openstatus/db to verify that database-dependent routes execute queries successfully against configured test datastores.

Accept when:
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace-wide type verification and import boundary linting.
- Verified by: Peer code reviews validating that database interactions import canonical schema definitions exclusively from @openstatus/db.
- Violation handling: Pull requests containing locally declared database schemas or unapproved direct database drivers outside @openstatus/db are blocked at continuous integration.
- Violation handling: Violations identified during code review must be refactored to consume @openstatus/db exports before merge approval.
- Exception process: Teams requiring specialized external datastore connections must submit an architectural exception proposal detailing why the centralized database module cannot accommodate the use case.