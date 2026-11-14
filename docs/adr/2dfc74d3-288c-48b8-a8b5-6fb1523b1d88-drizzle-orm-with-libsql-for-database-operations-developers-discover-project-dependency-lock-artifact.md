---
version: 2
name: 2dfc74d3-288c-48b8-a8b5-6fb1523b1d88-drizzle-orm-with-libsql-for-database-operations-developers-discover-project-dependency-lock-artifact
description: "drizzle-orm with LibSQL for Database Operations: Developers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 2dfc74d3-288c-48b8-a8b5-6fb1523b1d88
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Developers Discover Project Dependency Lock Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/**`

## Context

- The database package requires unified data access abstractions that support both stateless HTTP-based query execution and synchronized embedded replicas.
- Direct database connections in serverless or distributed deployment topologies benefit from decoupled transport adapters, separating schema definitions from runtime database drivers.
- Data access operations must share a centralized schema definition across query construction, database seeding routines, and workspace validation rules.
- Synchronization between local embedded replica storage and remote primary databases requires dedicated configuration for replication intervals and credential management.

## Problem Statement

Managing database queries and schema definitions across distributed deployment targets introduces friction when transport modes differ between stateless HTTP execution and local replica synchronization. Without a standardized persistence library and driver adapter layer, query logic fragments across connection implementations, risking schema divergence and inconsistent data synchronization.

## Decision

1. MUST: Developers MUST discover the project dependency lock artifact and resolve the exact installed versions of drizzle-orm, drizzle-orm/libsql, and @libsql/client before introducing or modifying database interaction code.

## Policy Block

- MUST Developers MUST discover the project dependency lock artifact and resolve the exact installed versions of drizzle-orm, drizzle-orm/libsql, and @libsql/client before introducing or modifying database interaction code.

In scope:
- Database connection initialization and query construction within the database workspace package
- Local embedded database replication and background synchronization workers
- Database seeding routines and schema validation utilities

Out of scope:
- Client-side frontend rendering components without direct persistence requirements
- External third-party API integration clients that do not communicate with the relational database
- Ephemeral memory caches operating independently of the primary relational persistence store

## Rationale

- Adopting drizzle-orm provides compile-time type safety for database operations while eliminating object-relational mapping overhead.
- Decoupling database connectivity into drizzle-orm/libsql/http for stateless workloads and @libsql/client embedded replication for stateful synchronization allows the application to optimize for edge latency and local read throughput.
- Centralizing table definitions in a shared schema module ensures unified type contracts across queries, migrations, seed routines, and plan limit validations.

## Consequences

Positive:
- Unified schema definitions eliminate query discrepancies between seeding workflows and production data access.
- Embedded local replicas with background synchronization significantly decrease read latency for edge or local environments.
- Driver modularity allows switching between HTTP transport and embedded local database access without altering query syntax.

Negative:
- Dual-driver maintenance between HTTP and embedded synchronization increases operational complexity.
- Background synchronization intervals introduce potential eventual consistency lag between replicas and the primary database.
- Developers must learn drizzle-orm query syntax and schema declaration conventions.

## Alternatives

- Heavyweight Object-Relational Mapping Framework (rejected)
  Rejected because: Traditional heavy ORMs introduce significant runtime bundle overhead, complex query generation, and poor cold-start characteristics in serverless or edge runtime environments.
  When valid: Valid in monolithic architectures where deep entity relationship lifecycle management and legacy database compatibility outweigh bundle size and cold-start speed.
- Raw SQL Queries with Direct Database Driver Calls (rejected)
  Rejected because: Direct SQL string execution lacks automated TypeScript type inference and increases the risk of schema drift between seed scripts, validation logic, and runtime queries.
  When valid: Valid in minimal single-purpose microservices with static schema structures and no shared domain logic.

## Risks

- Replication synchronization delay could cause stale reads when reading from local replica database instances.
  Mitigation: Configure the synchronization interval appropriately for workload tolerances and route consistency-sensitive writes and reads to the primary database.
  Owner: engineering team
- Schema drift between validation schemas and database table definitions leading to runtime validation failures.
  Mitigation: Derive workspace validation schemas directly from shared database schema definitions and enforce validation tests in verification pipelines.
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
- Discover the database workspace directory within the repository structure to locate existing database client configurations and schema definitions.
- Inspect environment variable configuration modules to verify required connection parameters including database URLs and authentication tokens before instantiating database clients.
- Ensure embedded replica instances specify an isolated local storage path and background synchronization interval in alignment with environment infrastructure constraints.

## Continuation Context


Verify commands:
- Discover and run the project test suite using the project build tool discovered from the repository manifest.
- Discover and run the project type checking script to verify schema and query type safety across all database modules.
- Discover and run the project linter and validation checks to ensure compliance with database import and module boundary rules.

Accept when:
- All database queries, seed operations, and client connections compile with zero type errors against the shared schema.
- Database synchronization clients successfully initialize with configured local storage parameters and remote synchronization endpoints.
- Automated tests covering database interactions and workspace schema validations pass without regression.

## Enforcement

- Verified by: Automated continuous integration pipeline executing type checks and test suites
- Verified by: Peer code review verifying adherence to database client instantiation standards and schema binding rules
- Violation handling: Continuous integration failure blocking code merge upon type mismatch or test failure
- Violation handling: Mandatory pull request remediation when unapproved database drivers or unmapped SQL queries are identified
- Exception process: Submit an architectural review request detailing performance bottlenecks that necessitate bypassing the standard drizzle-orm client abstraction
- Exception process: Document temporary exceptions with an expiration timeline and risk mitigation strategy approved by lead architects