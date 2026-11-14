---
version: 2
name: 9f84e694-08c5-46a4-b6a3-0fe3aa64aebf-adoption-of-openstatus-db-for-centralized-database-schema-and-transactional-persistence-read-queries-across-entity-tables-utilize
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Read Queries Across Entity Tables Utilize"
metadata:
  actual:
    adr_id: 9f84e694-08c5-46a4-b6a3-0fe3aa64aebf
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Read Queries Across Entity Tables Utilize

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/v1/**`

## Context

- Server route handlers require consistent access to relational database schemas and transactional operations across endpoints.
- Directly defining database connections or schemas within individual route handlers leads to schema drift, transaction inconsistency, and connection management issues.
- The project utilizes a dedicated shared workspace package @openstatus/db alongside @openstatus/db/src/schema to encapsulate database entities and query execution.

## Problem Statement

Without a centralized database module and schema package, individual route handlers risk introducing diverging entity definitions, uncoordinated schema migrations, and non-atomic composite writes across interrelated tables.

## Decision

1. MUST: Read queries across entity tables MUST utilize fluent query builder methods provided by the @openstatus/db client rather than raw database driver queries.

## Policy Block

- MUST Read queries across entity tables MUST utilize fluent query builder methods provided by the @openstatus/db client rather than raw database driver queries.

In scope:
- Route handlers and server services requiring database queries, schema validation, and atomic state mutations
- Backend modules interacting with persistent data models

Out of scope:
- In-memory cache retrieval and route context lookup
- Third-party client integrations that do not interact with the primary database

## Rationale

- Centralizing database schemas in @openstatus/db/src/schema ensures a single source of truth for database models across server routes.
- Utilizing transaction primitives from @openstatus/db guarantees atomic consistency when creating or updating interdependent entities.
- Enforcing a unified data access module prevents connection proliferation and inconsistent query patterns across endpoints.

## Consequences

Positive:
- Ensures relational schema consistency across all server route implementations
- Guarantees atomic consistency during composite entity mutations through unified transaction handling
- Centralizes database configuration, migration control, and connection pooling in a single module

Negative:
- Route handlers are tightly coupled to the export contracts and schema definitions of @openstatus/db
- Modifications to database entities require updating and rebuilding the shared database module before consuming in server routes

## Alternatives

- Direct inline database drivers and raw SQL queries within individual route handlers (rejected)
  Rejected because: Eliminates unified type safety, duplicates connection pooling logic, and risks schema divergence across endpoints
  When valid: Only in isolated legacy scripts or standalone migration utilities without shared package dependencies
- Independent repository pattern abstractions wrapping each entity in isolated server services (deferred)
  Rejected because: Introduces unnecessary abstraction overhead when query builder and transaction primitives from the shared database module provide sufficient separation
  When valid: If domain logic complexity increases to require domain-driven design aggregates decoupled from the persistence layer

## Risks

- Breaking changes in @openstatus/db/src/schema can cascade into compile-time or runtime failures across route handlers
  Mitigation: Validate all consumer route types and database integration tests in the continuous integration pipeline prior to merging schema modifications
  Owner: Backend Engineering Team
- Long-running multi-entity transactions within route handlers may cause database connection starvation or lock contention
  Mitigation: Keep transactional blocks minimal by preparing input structures prior to opening transactions and enforcing query execution timeout limits
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
- Inspect the shared database module export contracts to ensure transactional helper methods and entity schema identifiers are consumed consistently across all route endpoints.
- Construct all validated domain objects and payload transformations prior to initiating database transaction blocks to minimize connection holding time.

## Continuation Context


Verify commands:
- Discover and run the repository type-checking script to ensure all @openstatus/db schema references and transaction query calls compile cleanly.
- Discover and execute the repository test runner targeting route integration suites to verify atomic database transactions and schema interactions.
- Discover and run the project linter to ensure no unapproved direct database imports or raw driver calls bypass @openstatus/db.

Accept when:
- Type checking completes with zero diagnostic errors across all route handlers importing @openstatus/db.
- Integration tests pass verifying that multi-entity write operations commit atomically and roll back completely upon failure.
- Static analysis confirms all database operations route through @openstatus/db without unauthorized direct database connections.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository type checks and integration test suites
- Verified by: Peer code review verifying adherence to transactional patterns and direct consumption of @openstatus/db/src/schema
- Violation handling: Pull requests containing direct database driver usage or bypasses of @openstatus/db will be blocked from merging until corrected.
- Exception process: Exceptions for specialized data migration scripts or external analytical queries require architectural review and approval from backend lead maintainers.