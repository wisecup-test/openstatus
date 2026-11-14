---
version: 2
name: 0a75ca61-15d1-415e-b33b-cb60135fab54-adoption-of-openstatus-upstash-for-distributed-caching-and-rate-limiting-keys-written-distributed-cache-layer-define
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Keys Written Distributed Cache Layer Define"
metadata:
  actual:
    adr_id: 0a75ca61-15d1-415e-b33b-cb60135fab54
    status: accepted
    candidate_group_id: c89369f1-016e-48d6-a1e7-23a0bcf0bd22
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/feedback/docs/**
      - src/lib/**
---

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Keys Written Distributed Cache Layer Define

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/feedback/docs/**`
- `src/lib/**`

## Context

- Web application endpoints and background health verification modules require shared state management, request throttling, and ephemeral key-value persistence.
- Operating within stateless request lifecycles necessitates a centralized caching and rate limiting mechanism that communicates over connectionless transport.
- Multiple application modules independently require access to shared rate limiting and cache primitives for request throttling and feedback record deduplication.

## Problem Statement

Serverless and edge web application handlers require low-latency, distributed key-value storage and rate limiting across decoupled execution contexts without maintaining persistent server connections.

## Decision

1. MUST: Keys written to the distributed cache layer MUST define explicit time-to-live expiration intervals to prevent unbounded storage growth and stale state leakage.

## Policy Block

- MUST Keys written to the distributed cache layer MUST define explicit time-to-live expiration intervals to prevent unbounded storage growth and stale state leakage.

In scope:
- Web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

Out of scope:
- Long-term relational data persistence and transactional database storage.
- Client-side browser state management and client-rendered interface caching.

## Rationale

- The @openstatus/upstash library centralizes distributed key-value persistence and rate limiting into a unified module adopted across API routes and utility libraries.
- Standardizing on a dedicated caching and throttling module ensures consistent key generation, expiration handling, and distributed rate limiting across serverless request boundaries.
- Consolidating stateful caching primitives into a shared interface avoids divergent caching implementations across endpoints.

## Consequences

Positive:
- Ensures consistent rate limiting and cache state across ephemeral and serverless execution boundaries.
- Decouples transient state management from individual application server lifecycles.
- Consolidates connection management, expiration policies, and key-value client utilities within a single internal library.

Negative:
- Network round-trips to the distributed key-value service introduce minor latency compared to local in-process memory.
- Application endpoints become dependent on the availability and credential configuration of the remote backing service.

## Alternatives

- In-memory process-local cache and rate limiting (rejected)
  Rejected because: Process memory is not synchronized across horizontal worker instances or serverless invocations, causing inconsistent rate limiting and fragmented cache state.
  When valid: Single-process standalone daemons where cross-instance state coordination is unnecessary.
- Direct relational database storage for rate limiting and temporary caching (rejected)
  Rejected because: Connection pool saturation and query latency make transactional database storage unsuitable for high-frequency key-value operations with short expiration windows.
  When valid: Audit logs or durable transactional records requiring strict persistence guarantees rather than transient caching.

## Risks

- Service interruption or network partitioning of the remote key-value store can block request handling or degrade rate limiting enforcement.
  Mitigation: Implement resilient fallback handling and error logging that define explicit fail-open or fail-closed behavior for each operational domain.
  Owner: Engineering Team
- Cache key collisions across distinct feature domains sharing the key-value namespace.
  Mitigation: Enforce domain-scoped key prefixing conventions across all cache access utilities.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

## Continuation Context


Verify commands:
- Discover the project verification script defined in the repository manifest and execute it to verify type checking and test suite compliance for modules consuming the shared cache library.
- Discover and run the project static analysis and linting script to ensure no unauthorized direct external cache client connections bypass the designated internal module.

Accept when:
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and linting verification scripts.
- Verified by: Peer code review verifying that distributed cache and rate limiting operations route exclusively through the approved module.
- Violation handling: Pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated.
- Violation handling: Non-compliant cache operations or unhandled cache exceptions flagged during review must be refactored prior to merge.
- Exception process: Submit an architectural review request documenting why the designated key-value module cannot satisfy required throughput or data durability constraints, subject to approval from lead maintainers.