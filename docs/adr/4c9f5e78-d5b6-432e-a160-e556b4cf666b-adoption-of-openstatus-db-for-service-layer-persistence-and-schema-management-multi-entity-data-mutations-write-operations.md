---
version: 2
name: 4c9f5e78-d5b6-432e-a160-e556b4cf666b-adoption-of-openstatus-db-for-service-layer-persistence-and-schema-management-multi-entity-data-mutations-write-operations
description: "Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Multi Entity Data Mutations Write Operations"
metadata:
  actual:
    adr_id: 4c9f5e78-d5b6-432e-a160-e556b4cf666b
    status: accepted
    candidate_group_id: ac7c8bf1-716e-4288-bcba-c365b3097035
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/db for Service-Layer Persistence and Schema Management: Multi Entity Data Mutations Write Operations

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/**`

## Context

- Domain service operations across the platform require consistent data access patterns, unified transaction boundaries, and shared relational schema definitions.
- Dispersed database logic without a centralized module leads to fragmented connection lifecycles, duplicated table models, and fragile cross-domain mutations.
- The codebase establishes @openstatus/db as the authoritative workspace module providing database query handles, transaction contexts, and entity schemas across service packages.

## Problem Statement

Domain services requiring database persistence risk diverging in schema declarations, connection management, and transactional guarantees if each service establishes its own database access strategy. An authoritative architectural standard is needed to centralize database entity schemas and enforce standardized transactional execution patterns across all service operations.

## Decision

1. MUST: Multi-entity data mutations and write operations MUST execute within a managed transaction context handle provided by the database client.

## Policy Block

- MUST Multi-entity data mutations and write operations MUST execute within a managed transaction context handle provided by the database client.

In scope:
- All domain services, handlers, and business logic modules that perform reads, writes, updates, or transactional mutations on persistent relational data.

Out of scope:
- Stateless utility functions, in-memory calculation helpers, and third-party external API clients that do not interact with persistent relational storage.

## Rationale

- Evidence across 120 service files demonstrates uniform adoption of @openstatus/db for database queries, schema references, and transactional mutations.
- Centralizing database definitions ensures that entity models remain synchronized with database migrations, preventing schema divergence across different service handlers.
- Encapsulating database connectivity within a standardized module enables consistent transaction coordination and uniform error propagation across all business logic.

## Consequences

Positive:
- Unified schema definitions eliminate schema drift and type discrepancies across domain services.
- Consistent transaction contexts ensure atomic operations and data integrity across multi-entity persistence workflows.
- Centralized database client management simplifies connection pool configuration, tracing, and operational monitoring.

Negative:
- Changes to core schema definitions in the shared module necessitate coordinated verification across all consuming service packages.
- Service operations are structurally bound to the relational query semantics and conventions enforced by the shared database module.

## Alternatives

- Decentralized database access with local schema definitions per service module (rejected)
  Rejected because: Creating independent connections and local schema definitions per service causes schema drift, redundant connection pools, and lack of transaction coordination across services.
  When valid: Valid only in completely independent microservices with physically isolated data stores.
- Direct database access via raw query strings without a centralized schema module (rejected)
  Rejected because: Raw queries lack compile-time schema validation, type safety, and centralized migration tracking across service handlers.
  When valid: Valid in legacy systems where dynamic relational schemas cannot be represented via static typing.

## Risks

- Centralizing database schemas in a shared module couples service domain handlers to a single schema distribution boundary.
  Mitigation: Maintain explicit semantic versioning, automated contract tests, and modular schema subpaths within the shared workspace module.
  Owner: engineering team
- Unbounded transaction contexts in service operations can cause database connection exhaustion under high concurrency.
  Mitigation: Enforce strict query limits, scoped transactional boundaries, and automated linting against long-running operations inside transactions.
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
- Consumers should structure service operations to accept transactional context handles to allow nested operations to participate in existing transactions without re-opening connections.
- Entity schemas and table relationships should be exported from dedicated subpaths within the shared database module to avoid circular dependency trees.

## Continuation Context


Verify commands:
- Inspect the project configuration to discover the automated static analysis script, then execute it to ensure all database imports originate from the designated module.
- Locate and run the test suite defined in the project configuration to verify database transactions and query operations function against the canonical schema.

Accept when:
- Static analysis verifies zero direct database connections or duplicate table definitions exist outside the designated database module.
- All service-layer data mutations and queries execute successfully within automated test suites without schema mismatch errors.

## Enforcement

- Verified by: Continuous integration automated linting and static analysis checking import paths across service packages.
- Verified by: Peer code review on all pull requests introducing new persistent entity mutations or queries.
- Violation handling: Static analysis build failures block merging if unapproved database connection libraries or bypass imports are detected.
- Violation handling: Code review changes requested requiring migration to the canonical database module.
- Exception process: Engineers must submit an architectural exception request detailing why the shared database module cannot satisfy the specific persistence requirement.