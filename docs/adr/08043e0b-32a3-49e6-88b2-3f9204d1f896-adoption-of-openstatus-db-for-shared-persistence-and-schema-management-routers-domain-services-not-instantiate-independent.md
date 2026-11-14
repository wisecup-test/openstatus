---
version: 2
name: 08043e0b-32a3-49e6-88b2-3f9204d1f896-adoption-of-openstatus-db-for-shared-persistence-and-schema-management-routers-domain-services-not-instantiate-independent
description: "Adoption of @openstatus/db for Shared Persistence and Schema Management: Routers Domain Services Not Instantiate Independent"
metadata:
  actual:
    adr_id: 08043e0b-32a3-49e6-88b2-3f9204d1f896
    status: accepted
    candidate_group_id: 4e974e32-9858-4cf2-979b-8d161892d22e
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/db for Shared Persistence and Schema Management: Routers Domain Services Not Instantiate Independent

Status: proposed
Date: 2025-02-17
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`

## Context

- API routing endpoints require persistent data access and consistent entity schema definitions across distinct domain routers.
- Direct or decentralized database client instantiations risk uncoordinated connection management and divergent entity schema declarations.
- The codebase centralizes relational schema definitions and database operations into the dedicated internal workspace module @openstatus/db, injected into router execution context.

## Problem Statement

Routing handlers spanning different domain boundaries need reliable access to shared persistent storage and authoritative entity schemas without duplicating connection pooling logic or decoupling validation schemas from underlying relational structures.

## Decision

1. MUST_NOT: Routers and domain services MUST NOT instantiate independent database connection clients or establish unmanaged connection pools outside the shared @openstatus/db module.

## Policy Block

- MUST_NOT Routers and domain services MUST NOT instantiate independent database connection clients or establish unmanaged connection pools outside the shared @openstatus/db module.

In scope:
- API route handlers, router definitions, and domain controllers requiring database interactions or relational schema references.

Out of scope:
- Stateless utility modules, pure calculation functions, and client-side view components that do not interface with persistence layers.

## Rationale

- Centralizing persistence logic within @openstatus/db guarantees a single source of truth for table schemas and query abstractions across routing endpoints.
- Injecting the database client via request context isolates network connection lifecycles from individual route implementations.
- Reusing shared schemas from @openstatus/db ensures type consistency between stored relational records and endpoint validation layers.

## Consequences

Positive:
- Unified database connection lifecycle and pool coordination across all API routing boundaries.
- Compile-time type safety derived directly from centralized relational schema declarations.
- Elimination of duplicate query logic and divergent schema definitions across packages.

Negative:
- Modifications to shared table schemas require coordinated updates and dependency resolution across all consuming modules.
- Couples API route implementations directly to the query interface and abstraction boundaries defined by @openstatus/db.

## Alternatives

- Direct database client instantiation within individual route files (rejected)
  Rejected because: Creates uncoordinated connection pooling, exhausts database connection limits, and scatters credential configuration across route handlers.
  When valid: Valid only in isolated standalone maintenance scripts operating outside the application runtime.
- Independent per-router schema declarations (rejected)
  Rejected because: Leads to schema divergence, inconsistent data representations, and increased maintenance overhead when database models evolve.
  When valid: Valid only when interfacing with unmanaged external third-party services where internal schemas do not apply.

## Risks

- Breaking schema modifications in the shared database module can induce cascading compile failures across multiple consuming routers.
  Mitigation: Execute workspace-wide type checks and schema compatibility verification suites across all consumers before merging modifications.
  Owner: Engineering Team
- Context propagation failure can result in missing database client references during route execution.
  Mitigation: Enforce strict context initialization checks and middleware validation that assert database client presence prior to router dispatch.
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
- Discover the shared workspace manifest and inspect the entry points for @openstatus/db to identify exported schema objects and query client interfaces.
- Ensure router context middleware is registered in the request processing pipeline so that route handlers receive the initialized database client via context.

## Continuation Context


Verify commands:
- Discover and execute the project automated test suite to validate database interactions and router handlers.
- Discover and run the project type checking and static analysis scripts to ensure compliant schema imports across all routers.
- Discover and execute the project linting scripts to verify that direct database client instantiations are absent from router modules.

Accept when:
- The discovered automated test suite passes with all database-dependent route handlers executing queries through context.
- Static analysis and type checking scripts complete with zero errors related to schema imports from @openstatus/db.
- Static inspection confirms no unauthorized database client instantiations exist outside the centralized persistence package.

## Enforcement

- Verified by: Continuous integration pipelines executing automated test and type checking suites on every code change.
- Verified by: Peer code review verifying that all database queries use context-injected clients and schemas from @openstatus/db.
- Violation handling: Pull requests containing independent database connections or local schema copies will fail continuous integration gates and be blocked from merge.
- Exception process: Architectural review with the engineering team documenting technical justification and lifecycle management plans for any non-standard data access.