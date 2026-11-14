---
version: 2
name: 6ec37df3-1e61-471f-9c4a-d81a50c034ca-libsql-client-adoption-embedded-replica-client-configurations-initialize-libsql
description: "@libsql/client Adoption: Embedded Replica Client Configurations Initialize Libsql"
metadata:
  actual:
    adr_id: 6ec37df3-1e61-471f-9c4a-d81a50c034ca
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Embedded Replica Client Configurations Initialize Libsql

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/**`

## Context

- The data tier requires database client infrastructure supporting both remote query operations and local embedded replica synchronization.
- Database lifecycle operations including schema migrations, fixture seeding, and runtime queries require consistent connection handling.
- Standardizing on a single client library avoids fragmented connection pooling and divergent configuration patterns across operational workflows.

## Problem Statement

Maintaining disparate database connection mechanisms across schema migrations, seed scripts, and runtime querying introduces operational drift and complicates embedded replica synchronization.

## Decision

1. MUST: Embedded replica client configurations MUST initialize @libsql/client via createClient specifying local database storage URI, remote synchronization URL, authentication token, and synchronization interval parameters.

## Policy Block

- MUST Embedded replica client configurations MUST initialize @libsql/client via createClient specifying local database storage URI, remote synchronization URL, authentication token, and synchronization interval parameters.

In scope:
- Database modules responsible for migrations, data seeding, and runtime query execution.
- Embedded replica synchronization routines connecting to remote database instances.

Out of scope:
- In-memory mock implementations used in unit testing that do not interface with relational storage.
- Stateless service consumers communicating exclusively through external application-level protocols.

## Rationale

- Standardizing on @libsql/client provides a uniform client API capable of handling remote database connections and local replica synchronization.
- Configuring client instances with synchronization parameters enables local edge read performance while maintaining consistency with upstream storage.
- Aligning migration lifecycles, seed execution, and runtime queries under a single client foundation eliminates behavioral divergence across environments.

## Consequences

Positive:
- Unifies connection lifecycle management across migrations, test seeding, and runtime query operations.
- Enables low-latency read operations via local embedded replicas with background synchronization.
- Centralizes database credential management and connection initialization patterns.

Negative:
- Couples the persistence architecture to the specific replication protocol and operational semantics of the client library.
- Requires managing local filesystem replica persistence and accounting for synchronization latency.

## Alternatives

- Generic embedded database client without remote replication (rejected)
  Rejected because: Lacks automated background synchronization against remote primary database endpoints.
  When valid: Valid in isolated single-node environments where remote synchronization is unnecessary.
- Direct remote network database connection pool driver (rejected)
  Rejected because: Incurs network round-trip latency on all query operations and precludes local replica caching.
  When valid: Valid when application runtimes reside within the same low-latency zone as the primary database cluster.

## Risks

- Replication latency between local replica storage and the upstream primary database during synchronization intervals.
  Mitigation: Calibrate synchronization interval settings according to consistency requirements and route write operations directly upstream.
  Owner: Database Engineering Team
- Local filesystem corruption or capacity exhaustion affecting the embedded replica storage.
  Mitigation: Monitor storage capacity for local replica volumes and execute integrity validations during application startup.
  Owner: Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct client configurations through centralized runtime configuration modules rather than inline credential literals.
- Configure local replica storage paths and synchronization interval parameters to balance query freshness against background network usage.

## Continuation Context


Verify commands:
- Discover and run the project type-checking script to verify @libsql/client interface compatibility.
- Discover and run the automated test suite to validate client connectivity and migration execution.
- Discover and run the linting suite to ensure database client imports conform to project boundary constraints.

Accept when:
- Static type checks verify interface compatibility for all client initializations without diagnostics.
- All database connectivity, migration, and replication test suites pass successfully.
- Repository dependency manifests contain no unapproved alternative relational database drivers.

## Enforcement

- Verified by: Continuous integration pipelines verifying static types and running test suites.
- Verified by: Pull request architectural code reviews validating client initialization calls.
- Violation handling: Automated build pipeline failure upon detection of non-compliant database client dependencies.
- Violation handling: Mandatory pull request revisions required before merging non-compliant data access changes.
- Exception process: Submit an architectural review request to the database engineering team specifying technical requirements that cannot be met by the standardized client library.