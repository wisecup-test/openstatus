---
version: 2
name: e0c82cae-0863-4cfb-8a78-31d003e14db6-direct-primary-datastore-access-via-internal-database-for-probe-ingestion-and-authentication-private-location-service-perform-probe-authentication
description: "Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Private Location Service Perform Probe Authentication"
metadata:
  actual:
    adr_id: e0c82cae-0863-4cfb-8a78-31d003e14db6
    status: accepted
    candidate_group_id: 01844da4-922e-415b-8a8a-f2dcfc9a0024
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
---

# Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Private Location Service Perform Probe Authentication

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `internal/server/**`
- `internal/tinybird/**`
- `internal/workflows/**`

## Context

- Private location probe instances poll for assigned monitors and stream ingestion telemetry back to the server using token-based authentication headers.
- Accurate monitor execution requires immediate reflection of monitor updates, deletions, and probe token revocations without propagation delay.
- The service utilizes the internal/database module to execute parameterized relational queries directly against the primary datastore on inbound requests.

## Problem Statement

Inbound requests from private location probes require validation of authorization tokens and resolution of monitor configurations. Introducing an intermediate cache layer introduces synchronization complexity and risks serving stale monitor assignments or accepting revoked credentials, whereas direct primary datastore access must be structured consistently across all ingestion protocols.

## Decision

1. MUST: The private location service MUST perform probe authentication and ingest metadata retrieval directly against the primary datastore using the internal/database module rather than introducing an intermediate caching tier.

## Policy Block

- MUST The private location service MUST perform probe authentication and ingest metadata retrieval directly against the primary datastore using the internal/database module rather than introducing an intermediate caching tier.

In scope:
- Private location RPC endpoints handling probe authentication, monitor polling, and metric ingestion.
- Server components querying relational models through the internal/database package.

Out of scope:
- Downstream analytics event forwarding to external analytical datastores.
- Outbound webhook notifications and workflow dispatch clients.

## Rationale

- Direct queries against the primary relational datastore guarantee strong consistency for token revocation and monitor lifecycle states.
- Consolidating query binding to internal/database models ensures type safety and predictable struct mapping across all ingest protocols.
- Avoiding an intermediate caching tier eliminates cache invalidation race conditions and reduces operational infrastructure complexity.

## Consequences

Positive:
- Immediate consistency for probe access authorization and monitor activation status.
- Simplified architecture with no distributed or local cache invalidation pipelines to maintain.
- Uniform ingest context resolution across distinct network monitoring protocols.

Negative:
- Direct relational database query overhead on every inbound probe polling and ingest request.
- Primary datastore connection pool utilization scales with inbound probe request frequency.

## Alternatives

- In-Memory Cache Layer for Probe Tokens and Monitor Configurations (rejected)
  Rejected because: Introducing an in-memory caching tier creates cache invalidation lag for revoked tokens and deleted monitors across distributed instances.
  When valid: When probe request throughput exceeds primary datastore connection limits and eventual consistency for token revocation is acceptable.
- Distributed Key-Value Cache Between Handlers and Primary Datastore (rejected)
  Rejected because: Adds operational infrastructure overhead, network serialization latency, and cache synchronization complexity without necessity at current probe traffic volumes.
  When valid: When read traffic from distributed probes threatens primary datastore availability and requires shared cache invalidation events.

## Risks

- Increased query volume on the primary datastore from high-frequency probe ingest requests.
  Mitigation: Ensure index coverage on token and relation lookup columns, monitor connection pool saturation, and optimize query execution plans.
  Owner: Platform Engineering
- Ingest request latency degradation due to synchronous primary datastore network round-trips.
  Mitigation: Consolidate lookup queries within shared context helpers, minimize joined columns, and enforce database query timeouts.
  Owner: Backend Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Consolidate common monitor and private location lookups into shared context helper functions to avoid divergent SQL query patterns across distinct ingestion protocol handlers.
- Configure primary datastore connection pool sizing and statement timeouts to accommodate synchronous per-request validation during probe traffic spikes.

## Continuation Context


Verify commands:
- Discover and run the repository test suite covering server request handlers and database integration.
- Discover and run the project static analysis and linting checks to ensure compliance with database query conventions.
- Discover and execute database migration verification scripts against the local development datastore.

Accept when:
- All probe handler integration tests pass against primary datastore queries with valid and invalid tokens.
- Ingest context retrieval binds database records to typed domain structures and returns unauthenticated errors on invalid tokens.
- Static analysis checks complete with zero policy violations.

## Enforcement

- Verified by: Automated continuous integration test suites executing integration tests against the database layer.
- Verified by: Peer code reviews verifying that new probe endpoints use internal/database models and parameterized queries.
- Violation handling: Pull requests bypassing internal/database patterns or introducing uncoordinated in-memory caches will be blocked during review.
- Violation handling: Static analysis and test failures will halt continuous integration pipelines.
- Exception process: Any proposal to introduce caching for probe authentication must submit an architectural RFC demonstrating primary datastore saturation and an explicit invalidation mechanism approved by the lead architect.