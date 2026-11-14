---
version: 2
name: be20b8ad-42a5-471c-b38a-a2498a7880eb-adoption-of-openstatus-db-shared-workspace-package-for-centralized-data-access-and-schemas-application-modules-import-schema-definitions-query
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Modules Import Schema Definitions Query"
metadata:
  actual:
    adr_id: be20b8ad-42a5-471c-b38a-a2498a7880eb
    status: accepted
    candidate_group_id: 8a25c2df-024a-4181-8d98-527a4ff68ed7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Modules Import Schema Definitions Query

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/**`
- `src/trpc/**`

## Context

- Server-side routes and background execution handlers require access to persistent data stores and structured database schemas.
- Fragmented database access patterns risk diverging schema definitions and multiple uncoordinated connection pools across application modules.
- Multiple application entry points import data access utilities and relational schema definitions from the shared workspace package @openstatus/db.

## Problem Statement

Direct, decentralized database configuration across multiple server handlers leads to connection pool exhaustion, schema drifts, and duplicated data model logic. A centralized data access and schema module is required to standardize database interactions across application services.

## Decision

1. MUST: Application modules MUST import schema definitions and query builders via official module entry points provided by @openstatus/db.

## Policy Block

- MUST Application modules MUST import schema definitions and query builders via official module entry points provided by @openstatus/db.

In scope:
- All server-side application route handlers and background execution entry points requiring database access
- Shared procedure callers requiring database queries or relational schema validation

Out of scope:
- Client-side browser code and components operating outside server execution environments
- Third-party services that communicate exclusively through external protocol APIs

## Rationale

- Evidence across application routes establishes uniform consumption of @openstatus/db for data access and schema definitions.
- Centralizing database connection management and schema declarations inside a single shared package eliminates duplicate connection overhead and schema drift.
- Reusing shared table definitions and query interfaces maintains structural integrity across asynchronous jobs and synchronous request endpoints.

## Consequences

Positive:
- Consistent schema definitions and centralized database connectivity across all server endpoints.
- Reduced duplication of database client instantiation logic across server routes and background tasks.
- Streamlined maintenance and schema updates managed within a single workspace dependency.

Negative:
- Consuming routes are tightly coupled to the contract and release cycle of the shared database package.
- Schema changes within the shared database module risk breaking changes across all consuming services.

## Alternatives

- Decentralized database connection creation and local schema definitions in each route (rejected)
  Rejected because: Causes duplicate connection pools, higher memory consumption, and schema drift across handlers
  When valid: Isolated single-purpose scripts that execute outside the application boundary
- Direct database access via external network services without a shared internal library (rejected)
  Rejected because: Introduces network serialization overhead and eliminates type-safe schema sharing
  When valid: Heterogeneous polyglot microservices where shared language modules cannot be distributed

## Risks

- Breaking changes in the shared schema propagate immediately to all consuming application endpoints.
  Mitigation: Enforce contract compatibility checks and non-breaking migration practices within the shared package.
  Owner: engineering team
- Unbounded connection acquisition across multiple consuming handlers exhausting database server connection limits.
  Mitigation: Configure and monitor centralized connection pool thresholds within the shared database module.
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
- Locate the workspace configuration to understand resolution paths for internal packages and ensure dependency linkages are properly registered.
- Reference database schemas and query client instances through exported subpaths rather than attempting direct file imports.

## Continuation Context


Verify commands:
- Discover and run the repository static analysis and type verification scripts to validate that all database imports resolve to @openstatus/db.
- Discover and run the project automated test suite to ensure server routes querying the shared database package execute without regressions.

Accept when:
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

## Enforcement

- Verified by: Automated continuous integration pipeline executing typecheck and test verification suites
- Verified by: Peer code reviews verifying adherence to shared package usage and schema access conventions
- Violation handling: Pull requests containing direct database driver instantiations or unapproved local schema definitions are blocked from merging until corrected.
- Exception process: Submit an architectural review request detailing technical constraints requiring custom database configurations or standalone connections.