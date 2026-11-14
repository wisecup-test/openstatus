---
version: 2
name: b5c00cc4-6508-47f7-b029-e959df807e42-drizzle-orm-sqlite-core-relational-schema-and-indexing-for-integration-boundaries-verify-locked-version-drizzle-orm-repository
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Verify Locked Version Drizzle Orm Repository"
metadata:
  actual:
    adr_id: b5c00cc4-6508-47f7-b029-e959df807e42
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Verify Locked Version Drizzle Orm Repository

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- Integration and monitoring domains process high volumes of event-driven records, status transitions, and audit logs scoped by workspace and entity boundaries
- Relational persistence requires consistent indexing patterns to avoid sequential scans during real-time timeline evaluations and historical lookups
- Drizzle ORM is used across all database schema definitions to model SQLite tables and composite index boundaries

## Problem Statement

High-frequency integration events, status reports, and audit logs require fast, tenant-isolated lookups without degrading into table scans across large chronological datasets.

## Decision

1. MUST: Verify the locked version of drizzle-orm in the repository resolution artifact prior to using or modifying schema APIs.

## Policy Block

- MUST Verify the locked version of drizzle-orm in the repository resolution artifact prior to using or modifying schema APIs.

In scope:
- All relational database schema definitions modeling integration entities, event logs, monitor runs, and audit trails
- All table definitions establishing tenancy boundaries or temporal status tracking

Out of scope:
- Transient in-memory data structures not backed by relational persistence
- External third-party API payloads prior to database ingestion

Exceptions:
- EXC-001: Tables with fewer than one thousand rows that are fully cached in memory

## Rationale

- Evidence across 31 schema files demonstrates consistent adoption of drizzle-orm/sqlite-core for defining relational tables alongside composite indexes
- Coupling workspace identifiers with chronological timestamps in indexes guarantees multi-tenant isolation and prevents full table scans on event histories
- Enforcing uniqueness via schema declarations eliminates race conditions during integration status recording

## Consequences

Positive:
- Deterministic low-latency query performance for tenant-scoped event logs, monitor runs, and status reports
- Compile-time validation of indexing paths aligned with TypeScript schema definitions
- Structural prevention of duplicate event entries and status conflicts via unique composite constraints

Negative:
- Write performance overhead increases linearly with the number of indexes on high-frequency event ingestion tables
- Index maintenance increases persistent storage requirements for historical integration logs and audit trails

## Alternatives

- Manual unindexed SQLite table definitions with application-level filtering (rejected)
  Rejected because: Fails to guarantee deterministic performance under load and causes table scans on event histories
  When valid: Prototyping non-relational or single-tenant transient scratchpads
- Raw SQL migration scripts without schema-driven TypeScript index declarations (rejected)
  Rejected because: Loses compile-time type safety and decouples query definitions from relational constraints
  When valid: Migrating legacy databases prior to adopting an ORM framework

## Risks

- Excessive indexing on high-frequency event ingestion tables causing increased write latency and storage bloat
  Mitigation: Monitor index size and write latency, pruning redundant secondary indexes on write-heavy tables
  Owner: Database Engineering Team
- Suboptimal composite index column ordering leading to ineffective index usage during multi-column filters
  Mitigation: Review query execution plans to verify composite index column ordering matches application access patterns
  Owner: Application Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure composite index column ordering places equality filters first followed by range or sort columns such as creation timestamps
- Group related schema definitions by domain boundary while sharing common identifier conventions across tables

## Continuation Context


Verify commands:
- Discover and execute the project database schema check script to validate index definitions
- Discover and execute the project test runner to verify schema constraint and query performance tests

Accept when:
- All schema definitions declare explicit indexes for tenant and event queries
- Schema verification checks pass without missing index warnings or type errors
- Integration event lookups adhere to indexed column ordering

## Enforcement

- Verified by: Automated static schema analysis during continuous integration
- Verified by: Peer review verification of new and updated database schema definitions
- Violation handling: Schema additions without required composite tenant and event indexes will fail automated review checks
- Violation handling: Pull requests introducing unindexed temporal queries will be blocked until schema indexing is added
- Exception process: Submit an architectural exception request detailing workload profiles and storage constraints
- Exception process: Obtain approval from database and platform maintainers prior to merging unindexed integration schemas