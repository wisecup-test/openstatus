---
version: 2
name: fd66e517-0717-4831-a26c-55752762ffae-adoption-of-openstatus-db-shared-workspace-module-for-persistence-and-schema-access-consumers-discover-project-dependency-resolution-artifact
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: fd66e517-0717-4831-a26c-55752762ffae
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Consumers Discover Project Dependency Resolution Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/subscriptions`

Applies to:
- `src/**`

## Context

- Background notification dispatching and subscriber delivery channels require persistent access to subscriber settings, page associations, and delivery status transitions.
- Multiple modules within the subscriptions domain execute database queries and state updates against shared relational entities.
- Consuming database entities across asynchronous dispatch handlers necessitates strong type consistency and centralized schema definitions.

## Problem Statement

Managing database queries and state updates across disparate notification dispatch and channel delivery modules without a centralized schema authority causes schema divergence, duplicate model definitions, and uncoordinated connection pooling.

## Decision

1. MUST: Consumers MUST discover the project dependency resolution artifact and verify the exact resolved version of internal and external dependencies prior to implementation.

## Policy Block

- MUST Consumers MUST discover the project dependency resolution artifact and verify the exact resolved version of internal and external dependencies prior to implementation.

In scope:
- Service packages requiring database queries, schema models, or entity state persistence.
- Event dispatch and notification channel modules managing subscriber records and status updates.

Out of scope:
- Client-side user interface packages lacking direct database access privileges.
- Stateless utility functions operating exclusively on in-memory parameters.

## Rationale

- Adopting a shared internal database module guarantees that entity schemas and query contracts remain uniform across all background dispatch and channel handlers.
- Centralizing database client instantiation avoids fragmented connection pools and simplifies connection lifecycle management across workspace packages.
- Direct use of shared schema definitions enables compile-time verification of relational queries and update mutations.

## Consequences

Positive:
- Centralizes connection management, schema definitions, and migration logic in a single authoritative workspace module.
- Guarantees compile-time type safety across database operations, preventing runtime field mismatches in asynchronous dispatch routines.
- Eliminates duplicate entity definitions and query builders across service boundaries.

Negative:
- Domain service packages remain tightly coupled to the internal database module schema and query interface contracts.
- Breaking schema modifications in the shared database module necessitate synchronized updates and deployments across all consumer packages.

## Alternatives

- Direct database client instantiation and independent schema definitions per service package (rejected)
  Rejected because: Instantiating isolated database connections and duplicate schemas per package introduces connection exhaustion, schema divergence, and uncoordinated data migrations across the repository.
  When valid: Valid only in completely decoupled microservices that operate with dedicated, independent data stores.
- Decoupled repository interfaces with abstracted data access layers in each service package (rejected)
  Rejected because: Introduces excessive interface boilerplate and maintenance overhead across monorepo packages when the shared typed schema already provides compile-time safety.
  When valid: Valid when domain logic must remain entirely decoupled from specific persistence drivers or when supporting polyglot persistence engines.

## Risks

- Schema modifications within the shared database module may inadvertently break queries in dependent service modules.
  Mitigation: Enforce workspace-wide type checking and automated regression testing across all consuming packages during continuous integration.
  Owner: Engineering Team
- Unindexed or unbounded queries in high-volume notification dispatchers could exhaust shared database connection capacity.
  Mitigation: Utilize explicit column selection, index-backed filter predicates, and deterministic sorting to bound query execution latency.
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
- Import database query clients and relational schemas strictly from the authorized exports of the shared database module.
- Structure relational queries with deterministic ordering and explicit relational inclusions to ensure idempotency across asynchronous event processing.

## Continuation Context


Verify commands:
- Discover and run the repository static analysis and linting scripts to verify that persistence imports originate exclusively from the authorized shared database module.
- Discover and execute the automated test suite to confirm that database query and mutation operations fulfill schema and ordering contracts.
- Execute the project type-checking script to ensure all schema entities and query parameter objects conform to the shared database module definitions.

Accept when:
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

## Enforcement

- Verified by: Automated static analysis and module boundary linting in the continuous integration pipeline.
- Verified by: Peer code review for persistence queries and schema modifications.
- Verified by: Automated workspace type checking across all consuming packages.
- Violation handling: Pull requests introducing unauthorized persistence imports or duplicate schemas fail automated build and lint checks.
- Violation handling: Violations identified during code review require refactoring to use the shared database module prior to merge approval.
- Exception process: Exceptions to direct consumption of the shared database module require documented architectural review outlining why the shared schema is insufficient.
- Exception process: Temporary workarounds must include an issue tracker reference and a target reconciliation milestone.