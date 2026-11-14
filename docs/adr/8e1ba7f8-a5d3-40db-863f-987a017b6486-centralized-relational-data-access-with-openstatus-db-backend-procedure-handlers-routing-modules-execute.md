---
version: 2
name: 8e1ba7f8-a5d3-40db-863f-987a017b6486-centralized-relational-data-access-with-openstatus-db-backend-procedure-handlers-routing-modules-execute
description: "Centralized Relational Data Access with @openstatus/db: Backend Procedure Handlers Routing Modules Execute"
metadata:
  actual:
    adr_id: 8e1ba7f8-a5d3-40db-863f-987a017b6486
    status: accepted
    candidate_group_id: 0e1a37eb-0f9b-4ab0-9e8c-680ac9ec4729
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/auth/**
      - src/router/**
---

# Centralized Relational Data Access with @openstatus/db: Backend Procedure Handlers Routing Modules Execute

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/auth/**`
- `src/router/**`

## Context

- The application backend coordinates diverse business domain entities including monitors, status pages, workspaces, users, and billing subscriptions across multiple API routes.
- Procedures require consistent compile-time type safety, structured relational traversal, and tenant data isolation to prevent cross-workspace data leakage.
- A shared internal workspace package @openstatus/db centralizes relational schemas, table constants, and query builder interfaces across the entire monorepo.

## Problem Statement

Decentralized, untyped database operations risk connection pool exhaustion, schema drift, and critical cross-tenant data leaks when multiple procedures independently query and mutate relational entities without standard client lifecycles or tenant constraints.

## Decision

1. MUST: All backend procedure handlers and routing modules MUST execute database queries, entity mutations, and relational traversals exclusively through database clients and schema definitions provided by @openstatus/db.

## Policy Block

- MUST All backend procedure handlers and routing modules MUST execute database queries, entity mutations, and relational traversals exclusively through database clients and schema definitions provided by @openstatus/db.

In scope:
- All backend API procedures, route handlers, and authentication helpers executing data queries or mutations against domain entities.
- Any data access logic requiring multi-tenant scoping by workspace or user ownership.

Out of scope:
- External HTTP and third-party SaaS API clients that do not interface with the primary relational database.
- Specialized analytical data warehouses and time-series endpoints that communicate via dedicated analytical service clients.

## Rationale

- Centralizing schema models and database clients within @openstatus/db guarantees consistent data modeling, strict type checking, and unified connection management across all API endpoints.
- Passing database client instances through procedure execution contexts enables deterministic mocking during tests and simplifies transaction propagation.
- Enforcing multi-tenant workspace identifiers in relational query filters prevents unauthorized data exposure across workspace boundaries.

## Consequences

Positive:
- Eliminates runtime data mapping bugs through end-to-end type safety spanning schema definitions, queries, and API response objects.
- Standardizes multi-tenant data isolation by consistently applying workspace scoping constraints across procedure contexts.
- Reduces boilerplate for complex entity graphs by providing declarative nested relational queries.

Negative:
- Deeply nested relational queries can unintentionally over-fetch columns or join tables if query scopes are not carefully constrained.
- Engineers must become proficient in both relational query syntax and SQL query builder paradigms provided by the database package.
- High coupling between router handlers and internal database schema definitions requires coordinated schema migrations across the repository.

## Alternatives

- Decentralized direct connection initialization in individual route files (rejected)
  Rejected because: Fragments connection lifecycle management, risks database connection pool exhaustion across serverless and long-running runtime environments, and breaks unified schema typing.
  When valid: Isolated standalone scripts that run outside the primary application runtime and require no access to shared domain relations.
- Class-based repository layer wrapping every entity and query variation (rejected)
  Rejected because: Introduces substantial boilerplate and unnecessary abstraction overhead when a type-safe relational query builder already exposes composable schema operations.
  When valid: Complex enterprise domains requiring strict decoupling from relational storage backends to support multiple heterogeneous persistence engines.

## Risks

- Omission of tenant isolation filters in complex query predicates leading to cross-workspace data leakage.
  Mitigation: Enforce workspace identifier filtering in procedure preconditions and validate tenant scoping through automated integration test suites.
  Owner: engineering team
- Performance degradation caused by deep nested relation loading in high-traffic read queries.
  Mitigation: Explicitly declare required nested relations and field projections rather than fetching unbounded entity relation trees.
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
- Construct relational queries with explicit column and relation constraints to minimize payload sizes and prevent unindexed joins.
- Ensure all mutations and conditional queries on tenant-owned entities include workspace identifiers extracted from authenticated procedure contexts.

## Continuation Context


Verify commands:
- Discover and run the project static type analysis verification script from the repository manifest to verify all queries comply with database schema definitions.
- Execute the repository test validation command discovered from the project configuration to verify multi-tenant isolation and query correctness.
- Run the project structural linting check discovered from the build configuration to verify that no direct database connection initializations bypass the centralized database package.

Accept when:
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

## Enforcement

- Verified by: Automated continuous integration pipelines executing static type analysis and test suites on all pull requests.
- Verified by: Mandatory peer code reviews validating tenant isolation filters on all query and mutation definitions.
- Verified by: Static analysis lint checks disallowing direct database driver imports outside the designated database package.
- Violation handling: Pull requests introducing direct connection pools or unvalidated query filters are automatically blocked by continuous integration.
- Violation handling: Identified data access violations must be refactored to use context-injected database clients and typed schemas before merge.
- Exception process: Submit an architectural review request explaining why the centralized database abstraction cannot support the specialized persistence or performance requirement.
- Exception process: Obtain explicit approval from senior engineering leadership before introducing direct connection logic or external data access adapters.
- Exception process: Document the exception and security review results alongside dedicated automated tests validating safety and isolation.