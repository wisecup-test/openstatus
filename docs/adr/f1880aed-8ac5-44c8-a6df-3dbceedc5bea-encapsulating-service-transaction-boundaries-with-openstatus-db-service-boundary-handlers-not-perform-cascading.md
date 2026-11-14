---
version: 2
name: f1880aed-8ac5-44c8-a6df-3dbceedc5bea-encapsulating-service-transaction-boundaries-with-openstatus-db-service-boundary-handlers-not-perform-cascading
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Not Perform Cascading"
metadata:
  actual:
    adr_id: f1880aed-8ac5-44c8-a6df-3dbceedc5bea
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Not Perform Cascading

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/**`

## Context

- Domain services coordinate complex business workflows that span primary entities, join tables, and auxiliary tracking records.
- Decentralized mutation logic across handlers risks orphaned relation records, partial update failures, and data drift during entity deletions and updates.
- Across 50 service definitions in the codebase, database transaction boundaries and schema operations are encapsulated directly within service boundary modules.

## Problem Statement

Managing multi-entity domain mutations across relational schemas without explicit service transaction boundaries leads to orphaned join-table records, partial write failures, and inconsistent state transitions across the application.

## Decision

1. MUST_NOT: Service boundary handlers MUST NOT perform cascading deletions or join-table mutations outside of a shared transactional client context.

## Policy Block

- MUST_NOT Service boundary handlers MUST NOT perform cascading deletions or join-table mutations outside of a shared transactional client context.

In scope:
- Service definition modules performing business logic, entity orchestration, or persistence operations.
- Mutations spanning multiple relational entities, join tables, or cascading lifecycle states.

Out of scope:
- Pure client-side utilities and presentation-tier rendering logic.
- Stateless computation modules that do not access persistent state.

## Rationale

- Encapsulating atomic transactions inside service boundary handlers prevents partial state writes when updating or deleting dependent domain entities.
- Standardizing on @openstatus/db transaction and schema utilities across all service definitions establishes consistent patterns for database lifecycle handling.
- Direct schema integration at the service boundary ensures compile-time type verification while keeping presentation tiers decoupled from persistence mechanics.

## Consequences

Positive:
- Atomic consistency is guaranteed across complex multi-entity domain mutations and cascading cleanups.
- Domain logic and relational operations remain centralized within dedicated service boundary handlers rather than leaking into transport layers.
- Schema-driven type safety provides compile-time guarantees for input queries and mutation values across services.

Negative:
- Service handlers become directly coupled to database schema representations and transaction lifecycles.
- Extended transaction blocks that inadvertently include external side effects can lead to connection pool saturation.

## Alternatives

- Direct uncoordinated database operations across API route handlers without service boundaries (rejected)
  Rejected because: Exposing raw database operations directly to presentation or transport layers duplicates transactional coordination logic and risks data inconsistencies across multi-entity cascades.
  When valid: Only in prototype scripts where service boundaries and data integrity requirements do not apply.
- Repository abstraction layer decoupling service handlers from @openstatus/db schema definitions (rejected)
  Rejected because: Introducing an intermediary repository abstraction layer introduces excessive boilerplate without providing tangible benefit given the tight coupling required for relational transaction management.
  When valid: When the application must support multiple heterogeneous database backends simultaneously.

## Risks

- Long-running service operations within transactions can cause database connection exhaustion or lock contention.
  Mitigation: Perform input validation, authentication, and external network interactions before opening database transactions.
  Owner: Service Engineering Team
- Direct coupling of service boundary handlers to schema entities can make schema refactoring labor-intensive across multiple services.
  Mitigation: Utilize schema-derived types and centralized schema exports to detect breaking changes through static type analysis.
  Owner: Service Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Sequence service operations to validate inputs and resolve authorization context prior to acquiring database transactions to minimize lock durations.
- Group cascading deletions and join table updates explicitly within the transaction block to preserve relational integrity across domain boundaries.

## Continuation Context


Verify commands:
- Discover the workspace test execution script from the repository manifest and run test suites covering service boundaries.
- Execute the project static analysis and type checking workflow to verify type compatibility across service handlers and database schema definitions.

Accept when:
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

## Enforcement

- Verified by: Automated continuous integration checks executing service integration test suites and static type verification.
- Verified by: Peer code review verifying that multi-table state modifications are contained within transactional boundaries.
- Violation handling: Pull requests containing uncoordinated multi-table mutations outside transaction boundaries are blocked from merging.
- Violation handling: Identified non-transactional multi-entity writes must be refactored into atomic transaction blocks before deployment.
- Exception process: Exceptions for non-transactional operations must be documented with an architectural rationale and approved by the service engineering lead.