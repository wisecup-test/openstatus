---
version: 2
name: 3b4b61f9-afe5-4533-8f36-c918be922e60-chronark-zod-bird-client-adoption-for-analytical-datastore-queries-query-parameters-passed-analytical-pipe-endpoints
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Query Parameters Passed Analytical Pipe Endpoints"
metadata:
  actual:
    adr_id: 3b4b61f9-afe5-4533-8f36-c918be922e60
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Query Parameters Passed Analytical Pipe Endpoints

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `src/**`

## Context

- Analytical pipe endpoints require structured query parameters including timestamps, periods, counters, and resource identifiers.
- Direct unvalidated HTTP access to datastore endpoints risks runtime parameter mismatch, unformatted responses, and credential exposure.
- Centralizing analytical datastore queries behind @chronark/zod-bird Client provides type-safe query interfaces integrated with runtime schema validation.
- Local development and test environments require consistent behavior when datastore credentials are unconfigured or when dry-run operations are requested.

## Problem Statement

Analytical datastore pipe queries frequently fail or produce silent data inconsistencies when request parameters deviate from the expected endpoint schema or when credentials are missing across varied runtime environments. Without a standardized datastore client layer enforcing runtime schema validation and safe credential fallback handling, services risk dispatching malformed queries and breaking downstream consumers.

## Decision

1. MUST: All query parameters passed to analytical pipe endpoints MUST be parsed and validated against schema objects prior to request dispatch.

## Policy Block

- MUST All query parameters passed to analytical pipe endpoints MUST be parsed and validated against schema objects prior to request dispatch.

In scope:
- All data access operations querying remote analytical datastore pipes within the workspace.
- Client module configurations managing datastore authentication credentials and endpoint URLs.

Out of scope:
- Transactional relational database operations governed by distinct ORM or schema layers.
- Client-side browser applications communicating with internal API gateways rather than primary datastores.

## Rationale

- Encapsulating datastore access within @chronark/zod-bird Client enforces typed contract definitions across analytical query boundaries.
- Validating query parameters using schema objects guarantees parameter integrity before network dispatch, reducing endpoint failures.
- Providing graceful fallback handling for missing tokens enables non-production workflows to run without requiring live datastore credentials.

## Consequences

Positive:
- Ensures end-to-end type safety and runtime validation for analytical pipe inputs.
- Eliminates duplicate HTTP client configuration and endpoint resolution logic across services.
- Prevents catastrophic application crashes in unauthenticated or local environments through controlled warning logs and empty response fallbacks.

Negative:
- Couples datastore communication to an external client library and schema validation framework.
- Requires maintaining synchronized query parameter schemas alongside remote pipe definition changes.

## Alternatives

- Direct unvalidated fetch calls to analytical datastore REST endpoints (rejected)
  Rejected because: Lacks compile-time and runtime validation, leading to silent query errors and fragmented credential handling.
  When valid: Simple exploratory scripts requiring zero dependencies where query schemas never change.
- Generic HTTP client with manual query string formatting (rejected)
  Rejected because: Requires boilerplate parameter serialization and manual error handling across every query invocation.
  When valid: Environments where specialized client libraries cannot be introduced due to binary footprint constraints.

## Risks

- Silent data omission when unconfigured credentials return empty datasets rather than explicit failures in production.
  Mitigation: Ensure production environment configurations enforce mandatory token presence checks during service bootstrap.
  Owner: Engineering Team
- Schema drift between local parameter validation schemas and remote pipe definitions.
  Mitigation: Automate schema synchronization or contract verification against datastore endpoints during test runs.
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
- Configure client instances with explicit environment token resolution and default base URL fallbacks.
- Define reusable schema objects for shared query parameters including periods, timestamps, and record counters.

## Continuation Context


Verify commands:
- Discover the workspace test execution script from the repository manifest and execute all analytical datastore client unit tests.
- Discover the static type analysis script from the repository configuration and verify type compatibility across all datastore client schemas and queries.

Accept when:
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

## Enforcement

- Verified by: Continuous integration test suites verifying client schema validation and error handling
- Verified by: Peer code review for all modules introducing or updating analytical datastore queries
- Violation handling: Pull requests bypassing the central client or omitting schema validation are blocked from merging.
- Violation handling: Direct raw HTTP calls to analytical endpoints must be refactored to use the client interface.
- Exception process: Submit an architectural review request documenting why the standardized client abstraction cannot support the specialized datastore integration.