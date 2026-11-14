---
version: 2
name: 736d5398-1396-4cf8-9ec7-b076cd4fe8b8-openstatus-db-in-memory-entity-indexing-and-relational-assembly-handlers-maintain-composite-string-keys-index
description: "@openstatus/db In-Memory Entity Indexing and Relational Assembly: Handlers Maintain Composite String Keys Index"
metadata:
  actual:
    adr_id: 736d5398-1396-4cf8-9ec7-b076cd4fe8b8
    status: accepted
    candidate_group_id: 828efdcd-90f3-452e-9a8c-a21e70de4bee
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# @openstatus/db In-Memory Entity Indexing and Relational Assembly: Handlers Maintain Composite String Keys Index

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/**`

## Context

- Backend service operations frequently interact with primary datastore tables through @openstatus/db to service composite domain requests involving related entities
- Relational aggregation at the datastore level across polymorphic or multi-entity structures can produce complex join queries or trigger query cascades when handling batch operations
- Service modules decouple flat batch queries against primary tables from composite entity synthesis by maintaining in-memory hash maps during request execution
- Transactional workflows and list endpoints require rapid key-based entity retrieval to perform status correlation, order adjustments, and conflict validation without additional datastore roundtrips

## Problem Statement

As service handlers assemble composite domain entities from the primary datastore, querying related tables independently often leads to repetitive query execution or complex SQL joins that degrade database throughput. The architecture requires a consistent, low-latency mechanism to correlate related entities and validate state transitions within service transactions while minimizing database roundtrip overhead.

## Decision

1. MAY: Handlers MAY maintain composite string keys in the index map when correlating entities across multi-column identifiers or partitioned time windows.

## Policy Block

- MAY Handlers MAY maintain composite string keys in the index map when correlating entities across multi-column identifiers or partitioned time windows.

In scope:
- Domain service operations executing within the services layer that query or mutate primary datastore entities
- Batch operations, list endpoints, and transactional updates requiring correlation across related datastore records

Out of scope:
- Direct single-record primary key lookups where no associated entities require correlation
- Analytical queries delegated to external specialized analytics datastores

## Rationale

- Evidence across twenty-six service modules demonstrates that pairing batch @openstatus/db queries with in-memory map lookups prevents repetitive query execution against the primary datastore
- Using hash maps for key-based correlation in the application layer simplifies SQL query generation, keeping datastore queries focused on single-table scans or simple filtered batches
- In-memory grouping allows fine-grained domain validation and schema parsing to occur deterministically before assembling nested response objects

## Consequences

Positive:
- Eliminates repetitive query cascades by batching datastore access into single table queries followed by local constant-time map resolution
- Simplifies datastore query complexity by avoiding multi-table joins and polymorphic relation queries in SQL
- Enforces deterministic domain schema parsing on individual entities prior to relational assembly

Negative:
- Increases heap memory consumption in the service runtime due to storing intermediate batch result sets in hash maps
- Requires manual maintenance of relational stitching logic within service handlers rather than relying on database-level joins

## Alternatives

- Deep relational SQL joins within the datastore query layer (rejected)
  Rejected because: Generates complex and brittle join queries across polymorphic relations, increasing datastore execution overhead and complicating partial record updates
  When valid: When aggregating strictly normalized schemas with fixed relational cardinality that require atomic datastore-level constraints
- External distributed caching layer for intra-request relational resolution (rejected)
  Rejected because: Introduces external network latency and distributed cache invalidation complexity for short-lived entity lookups that only exist for the duration of a single service request
  When valid: When caching computationally expensive aggregates shared across multiple concurrent user sessions

## Risks

- Unbounded memory consumption if batch queries return unexpectedly large result sets into in-memory maps
  Mitigation: Enforce query pagination and upper limits on batch query inputs within domain service contracts
  Owner: Backend Engineering Team
- Stale or inconsistent in-memory indexes if mutable state changes mid-transaction across independent maps
  Mitigation: Scope index maps strictly to single execution scopes and synchronize updates within transaction blocks
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
- Populate index maps immediately following batch datastore selection, using entity identifier attributes as primary keys and parsed schema objects as map values
- When grouping one-to-many relationships, initialize map entries with empty collections and append parsed child records as batch rows are iterated

## Continuation Context


Verify commands:
- Discover the project verification script declared in the workspace configuration and run the service test suite to validate in-memory aggregation behavior
- Discover the workspace static analysis and type check command and execute across all service modules to verify datastore client typing
- Discover and execute the lint verification task from the repository task runner to check compliance with query patterns

Accept when:
- All service unit and integration tests executing batch aggregation and map indexing pass without errors
- Static analysis and type checking pass across all service packages interacting with the datastore module
- No iterative datastore query patterns within loops are detected in service handler implementations

## Enforcement

- Verified by: Continuous integration pipelines executing automated unit and integration tests
- Verified by: Peer code review verifying that datastore queries avoid query iteration in loops and properly structure in-memory indexes
- Verified by: Static analysis checks verifying schema parsing on datastore results
- Violation handling: Pull requests exhibiting iterative datastore queries inside loops or missing index structures must be revised before merge approval
- Violation handling: Services with excessive heap growth during batch operations must introduce query bounds and pagination
- Exception process: Architectural exceptions for custom SQL joins or alternate caching mechanisms must be documented in a service RFC and approved by the lead backend engineer