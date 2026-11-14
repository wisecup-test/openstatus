---
version: 2
name: 49eda051-35dc-4742-a9de-d3f70be16d5a-libsql-client-embedded-replica-database-client-adoption-embedded-replica-database-clients-specify-syncinterval
description: "@libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Specify Syncinterval"
metadata:
  actual:
    adr_id: 49eda051-35dc-4742-a9de-d3f70be16d5a
    status: accepted
    candidate_group_id: 841dda31-b2d0-4b1e-abd1-ce373de207ce
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Embedded Replica Database Client Adoption: Embedded Replica Database Clients Specify Syncinterval

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/**`

## Context

- The application persistence architecture requires low-latency database access while maintaining synchronized state with a central remote database.
- Database operations including schema migrations, data seeding, and runtime data synchronization must share a coherent connection and replication configuration.
- Evidence demonstrates the adoption of @libsql/client with embedded replica configuration linking local replica files to remote endpoints via background synchronization.

## Problem Statement

Connecting distributed services directly to a central remote database introduces network latency overhead on read queries, while maintaining isolated local databases leads to split-brain states and divergence from the primary persistent store.

## Decision

1. SHOULD: Embedded replica database clients SHOULD specify a syncInterval parameter to automate periodic background replication against the primary database.

## Policy Block

- SHOULD Embedded replica database clients SHOULD specify a syncInterval parameter to automate periodic background replication against the primary database.

In scope:
- Database connection initialization, schema migration, database seeding, and data synchronization modules within the database package.

Out of scope:
- Client-side user interface components, external third-party service adapters without persistent storage, and stateless utility functions.

## Rationale

- Adopting @libsql/client provides first-class support for embedded replica synchronization, allowing local disk read speeds with automatic remote replication.
- Configuring createClient with explicit sync interval, remote sync URL, and authentication token standardizes the replication lifecycle across migration, synchronization, and seeding workflows.
- Evidence across multiple database package modules confirms consistent reliance on @libsql/client for database connectivity and schema management.

## Consequences

Positive:
- Provides local read latency by querying an embedded local replica file directly on disk.
- Maintains centralized state synchronization with remote services through configured periodic sync intervals.
- Ensures uniform database connectivity semantics across migration, seeding, and runtime synchronization tasks.

Negative:
- Background replication introduces potential read lag and eventual consistency windows between local replicas and the remote primary database.
- Requires persistent local filesystem storage for the local replica database file alongside credentials for the remote sync endpoint.
- Periodic synchronization cycles consume local background I/O and network bandwidth.

## Alternatives

- Direct remote client-server database connection without local embedded replicas (rejected)
  Rejected because: Introduces network latency overhead on every query and eliminates offline read availability.
  When valid: Valid when deployment environments forbid local filesystem persistence or local disk storage is unavailable.
- Purely local SQLite file database without remote replication synchronization (rejected)
  Rejected because: Prevents data distribution across instances and fails to centralize shared persistent state.
  When valid: Valid for standalone single-instance tools or isolated local development fixtures.

## Risks

- Data divergence or stale reads occurring when remote synchronization fails or experiences intermittent network partitions.
  Mitigation: Enforce strict synchronization intervals and implement error handlers for network sync failures to detect divergence.
  Owner: engineering team
- Ephemeral or unpersisted local replica files causing full re-synchronization cycles on service container restarts.
  Mitigation: Configure deployment volume mounts and file permissions to preserve the local replica across application restarts.
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
- Encapsulate database client creation in a centralized database package module to prevent divergent replication configurations across features.
- Ensure environment configuration schemas strictly validate the presence of database remote endpoint URLs and authentication tokens before client instantiation.

## Continuation Context


Verify commands:
- Discover the repository test runner from the project manifests and execute the database integration test suite.
- Discover and execute the repository type-checking and linting scripts to verify database client instantiation conformance.

Accept when:
- Database client initialization successfully instantiates local embedded replica instances with remote synchronization parameters.
- Database migration and seeding workflows execute without connection errors against the configured client.
- Repository verification suites and integration checks confirm connectivity and schema synchronization pass without regression.

## Enforcement

- Verified by: Static analysis rules validating that database connection modules utilize @libsql/client with approved client configuration patterns.
- Verified by: Continuous integration pipelines executing database verification and synchronization suites.
- Verified by: Architecture review gates during code review for any new persistence modules.
- Violation handling: Pull requests introducing unapproved database client libraries or direct un-replicated database connections are blocked in automated checks.
- Violation handling: Non-compliant database instantiation code must be refactored to consume the centralized client configuration.
- Exception process: Propose an architectural deviation request documenting why local embedded replica synchronization cannot satisfy data consistency or storage constraints.
- Exception process: Obtain sign-off from database architecture leads prior to introducing alternative database drivers.