---
version: 2
name: 2f68143d-3f27-4665-971d-2704a03a92be-drizzle-orm-and-libsql-database-access-layer-database-persistence-schema-definitions-migrations-query
description: "Drizzle ORM and LibSQL Database Access Layer: Database Persistence Schema Definitions Migrations Query"
metadata:
  actual:
    adr_id: 2f68143d-3f27-4665-971d-2704a03a92be
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Database Persistence Schema Definitions Migrations Query

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/**`

## Context

- The codebase requires structured, type-safe data persistence, schema management, and migration execution for relational database workloads.
- Distributed or edge-capable runtime environments necessitate flexible database connectivity supporting both remote HTTP connections and local embedded replicas with background synchronization.
- Multiple database operations including operational querying, schema migrations, embedded replica sync, and seeding require a consistent ORM and driver interface.

## Problem Statement

Managing relational data across disparate execution contexts—such as transactional service queries, schema migrations, background replica synchronization, and seeding routines—risks inconsistency and type safety regressions if disparate database drivers and query mechanisms are employed. A unified, type-safe persistence and client layer is required to standardize relational interactions.

## Decision

1. MUST: All database persistence, schema definitions, migrations, and query operations MUST use Drizzle ORM with the LibSQL driver adapter.

## Policy Block

- MUST All database persistence, schema definitions, migrations, and query operations MUST use Drizzle ORM with the LibSQL driver adapter.

In scope:
- Relational database access, querying, migrations, local replica synchronization, and data seeding in database packages.

Out of scope:
- Non-relational in-memory transient caches or external non-SQL persistence stores.

## Rationale

- Adopting Drizzle ORM in conjunction with LibSQL provides full TypeScript type safety directly inferred from schema definitions with minimal runtime overhead.
- The LibSQL client interface natively supports embedded replica synchronization and HTTP connectivity, enabling local-first low-latency reads alongside remote database synchronization.
- Centralizing migrations and schema definitions through dedicated Drizzle ORM LibSQL adapters ensures consistent schema evolution across environments.

## Consequences

Positive:
- End-to-end TypeScript type safety across queries, mutations, migrations, and schema declarations.
- Support for local embedded replica databases with automated synchronization minimizes read latency in distributed environments.
- Thin abstraction layer minimizes runtime execution overhead compared to heavyweight object-relational mapping frameworks.

Negative:
- Schema alterations require explicit Drizzle migration generation and execution steps.
- Couples persistence workflows to LibSQL-compatible dialects and driver interfaces.
- Embedded replica synchronization introduces operational complexity regarding sync intervals and file-system replication state.

## Alternatives

- Prisma ORM with standard database drivers (rejected)
  Rejected because: Heavier runtime engine binary and higher execution overhead, with less native ergonomics for LibSQL embedded replica synchronization.
  When valid: Valid when adopting monolithic enterprise applications requiring extensive automated administration interfaces and broad multi-database dialect abstraction.
- Raw SQL queries using generic database client drivers (rejected)
  Rejected because: Lack of compile-time schema type safety, manual mapping overhead, and fragmented migration management.
  When valid: Valid for lightweight services with minimal query surfaces where a zero-dependency mapping footprint is strictly required.

## Risks

- Local embedded replica desynchronization or file locking issues during concurrent process access.
  Mitigation: Ensure sync intervals, replica file permissions, and single-writer constraints are properly configured and validated through environment settings.
  Owner: engineering team
- Breaking API changes across Drizzle ORM and LibSQL client version upgrades.
  Mitigation: Enforce strict lock-file resolution and verify API stability across driver and ORM packages prior to version increments.
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
- Define all relational entities and schema tables in centralized schema modules, deriving TypeScript types directly from schema models.
- Initialize database clients through centralized configuration modules that inject validated environment variables for connection URLs, authentication tokens, and replication intervals.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script to validate that all database queries, migrations, and schema definitions conform to Drizzle ORM and LibSQL client types.
- Discover and run the project's test suite to verify database connection initialization, schema migration execution, and query operations.

Accept when:
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

## Enforcement

- Verified by: Continuous integration automated check pipelines executing type verification and database integration tests.
- Verified by: Peer code review confirming all database access utilizes Drizzle ORM constructs and designated LibSQL client abstractions.
- Violation handling: Pull requests introducing raw unmapped database queries or unapproved driver dependencies are blocked from merging.
- Exception process: Formal architectural review and approval from database maintainers documenting technical justification for bypassing Drizzle ORM.