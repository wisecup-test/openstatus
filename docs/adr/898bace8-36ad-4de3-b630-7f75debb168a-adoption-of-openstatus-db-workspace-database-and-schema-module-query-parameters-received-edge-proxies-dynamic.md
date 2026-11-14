---
version: 2
name: 898bace8-36ad-4de3-b630-7f75debb168a-adoption-of-openstatus-db-workspace-database-and-schema-module-query-parameters-received-edge-proxies-dynamic
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Query Parameters Received Edge Proxies Dynamic"
metadata:
  actual:
    adr_id: 898bace8-36ad-4de3-b630-7f75debb168a
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Query Parameters Received Edge Proxies Dynamic

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/**`

## Context

- The status page application requires relational persistence across API route handlers, routing proxies, authentication adapters, and metadata endpoints.
- Managing independent database connections and disparate schema declarations across application boundaries increases the risk of connection pool exhaustion and data model divergence.
- The codebase establishes a centralized internal workspace library, @openstatus/db, exporting database client instances and relational schema definitions.

## Problem Statement

Distributing database access logic and schema declarations across multiple routing, authentication, and proxy layers without a centralized abstraction causes duplicated database drivers, uncoordinated connection lifecycles, and schema drift. A formalized decision is required to mandate that all relational data interactions and schema references within the application utilize the shared @openstatus/db workspace module.

## Decision

1. MUST: Query parameters received by edge proxies or dynamic routes MUST undergo validation against schemas compatible with the shared database definitions before executing database operations.

## Policy Block

- MUST Query parameters received by edge proxies or dynamic routes MUST undergo validation against schemas compatible with the shared database definitions before executing database operations.

In scope:
- All application route handlers, middleware proxies, authentication providers, and metadata generators requiring database access or schema definitions.
- Workspace packages that participate in shared relational data access.

Out of scope:
- Stateless client-side interface components that communicate exclusively through backend endpoints.
- Static content rendering utilities that operate without relational database persistence.

Exceptions:
- EXC-001: A specialized worker requires an isolated transactional connection pool for heavy background processing.

## Rationale

- Centralizing database access within @openstatus/db ensures unified connection pooling and consistent database driver configuration across all application routes.
- Exporting relational schemas from a single shared module ensures compile-time type safety across database queries, authentication adapters, and input validation schemas.
- The IR evidence demonstrates consistent adoption across seven critical files covering routing, authentication, proxy inspection, and sitemap generation.

## Consequences

Positive:
- Guarantees a single source of truth for relational schemas and database client configuration.
- Eliminates connection pool bloat by preventing ad-hoc database client initializations across separate route handlers.
- Maintains compile-time type safety from the database schema through to query execution and authentication adapter bindings.

Negative:
- Creates a direct architectural dependency between application handlers and the internal database package.
- Schema alterations in the shared database package require coordinated compilation and deployment across all consuming routes.

## Alternatives

- Direct database client instantiation per application route or module (rejected)
  Rejected because: Creating independent client instances leads to connection pool exhaustion, schema divergence, and uncoordinated migrations across application boundaries.
  When valid: When prototyping isolated single-file scripts or external serverless functions completely disconnected from the primary data store.
- Service-oriented HTTP or RPC abstraction layer between consumers and database (deferred)
  Rejected because: Adds network latency and service orchestration complexity when a shared internal module already provides direct type-safe query execution in the monorepo.
  When valid: When polyglot services written in different programming languages require access to the persistence store without sharing runtime libraries.

## Risks

- Breaking schema changes in the shared module propagate immediately to all consuming route handlers and proxy components.
  Mitigation: Enforce automated workspace type checking and schema contract validation across all consumer packages prior to deploying database migrations.
  Owner: engineering team
- High-frequency proxy middleware queries could saturate shared database connection pools under elevated traffic loads.
  Mitigation: Configure connection pooling parameters within the shared client and enforce query caching headers at network boundaries.
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
- Configure database query execution using the exported database instance and verify that schema references resolve from the schema subpath.
- Ensure that schema changes are accompanied by repository-wide type verification across all dependent application layers.

## Continuation Context


Verify commands:
- Discover and run the workspace type-checking script to ensure all references to @openstatus/db and its schema exports compile cleanly.
- Discover and run the project static analysis linting script to validate module import boundaries and ensure no unauthorized persistence libraries are imported.

Accept when:
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

## Enforcement

- Verified by: Automated workspace type checking during continuous integration.
- Verified by: Static analysis import boundary rules restricting database driver imports to the designated database module.
- Verified by: Peer code review for all pull requests modifying persistence or schema consumption.
- Violation handling: Pull requests containing direct imports of database drivers or bypassing the shared database package will fail automated continuous integration checks.
- Violation handling: Architectural violations flagged during code review must be refactored to import from @openstatus/db before merging.
- Exception process: Exceptions must be submitted via an architectural exception request detailing connection requirements and resource impacts.
- Exception process: Approval from the lead architect and database administrator is required prior to introducing standalone database connections.