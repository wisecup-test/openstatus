---
version: 2
name: 568e31f9-58cf-4d85-9f0d-b3058eff0e72-drizzle-orm-declarative-schema-indexing-and-tenant-boundary-isolation-database-schema-definitions-drizzle-orm-explicitly
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly"
metadata:
  actual:
    adr_id: 568e31f9-58cf-4d85-9f0d-b3058eff0e72
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- Relational database tables serve as the persistence boundary for multi-tenant configurations, real-time monitoring streams, and external service health records.
- High-frequency telemetry ingestion and concurrent API reads require explicit index definitions to prevent table scans on partitioned entities.
- Declarative schema definitions within the persistence layer must define relational constraints directly alongside table definitions to maintain consistency across migrations.

## Problem Statement

Without centralized and declarative indexing conventions in database schemas, multi-tenant queries and real-time event lookups risk executing unindexed full table scans, degrading read latency and compromising transactional isolation. Declarative index and uniqueness specifications must be co-located with table definitions to guarantee consistent query execution paths and prevent duplicate entries across integration entities.

## Decision

1. MUST: Database schema definitions using Drizzle ORM MUST explicitly declare index and uniqueness constraints within table configuration callbacks for all multi-tenant partition keys, external entity integrations, and timestamped query boundaries.

## Policy Block

- MUST Database schema definitions using Drizzle ORM MUST explicitly declare index and uniqueness constraints within table configuration callbacks for all multi-tenant partition keys, external entity integrations, and timestamped query boundaries.

In scope:
- All persistent database table definitions in the data tier.
- Relational entities representing multi-tenant resources, event telemetry, external service components, and status reports.

Out of scope:
- In-memory ephemeral cache stores.
- Stateless client-side data structures and transit payload models.

Exceptions:
- EXC-001: Single-row configuration tables or static lookup enumerations where table scans incur negligible operational cost.

## Rationale

- Declarative schema definitions via Drizzle ORM co-locate indexing strategies with data models, ensuring migration generation reflects current query requirements.
- Consistent indexing on tenant partitioning keys guarantees that multi-tenant read requests remain isolated and index-backed.
- Compound uniqueness constraints across integration identifiers and temporal intervals prevent race conditions during concurrent telemetry ingestion.

## Consequences

Positive:
- Guaranteed sub-millisecond index-assisted query routing for tenant-scoped read paths and event streams.
- Compile-time synchronization between relational schema types and index definitions.
- Prevention of duplicate records across multi-column business identity constraints.

Negative:
- Additional storage overhead and memory consumption for maintaining secondary and composite indices.
- Increased write latency during high-throughput ingestion of real-time monitoring events.

## Alternatives

- Imperative SQL migration scripts without declarative ORM index definitions (rejected)
  Rejected because: Decouples schema indexing metadata from application domain types and increases the risk of unindexed foreign key lookups.
  When valid: Valid when legacy database schemas cannot be managed via ORM schema declarations.
- Relying exclusively on primary key lookups without secondary tenant indices (rejected)
  Rejected because: Results in full table scans when filtering real-time event sequences or tenant partitions.
  When valid: Valid for strictly key-value access models that never execute partitioned or range queries.

## Risks

- Index sprawl causing degraded write performance on high-frequency event ingestion tables.
  Mitigation: Review index access patterns regularly and avoid single-column index declarations when existing composite index prefixes already provide identical coverage.
  Owner: Data Engineering Team
- Schema drift between declarative schema index definitions and physical database catalogs.
  Mitigation: Run automated migration diff verification in continuous integration pipelines to detect unapplied schema modifications.
  Owner: Data Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Declare table indices within the second argument configuration callback of schema builders using typed table column references.
- Place the tenant partition column as the leading element in composite index definitions to maximize index selectivity for tenant-scoped queries.

## Continuation Context


Verify commands:
- Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
- Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.

Accept when:
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

## Enforcement

- Verified by: Continuous integration automated schema verification and static type checking.
- Verified by: Mandatory peer review on all schema definitions and migration diffs.
- Violation handling: Pull requests containing unindexed tenant foreign keys or unindexed temporal range query columns are blocked.
- Violation handling: Violations detected in continuous integration prevent deployment to staging and production environments.
- Exception process: Submit an architectural RFC outlining the data access pattern and justification for omitting index definitions.
- Exception process: Obtain formal approval from the data architecture team prior to merging schema modifications.