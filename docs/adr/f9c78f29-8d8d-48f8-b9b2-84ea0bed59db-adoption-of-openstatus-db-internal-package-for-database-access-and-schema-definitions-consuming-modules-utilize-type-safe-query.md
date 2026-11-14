---
version: 2
name: f9c78f29-8d8d-48f8-b9b2-84ea0bed59db-adoption-of-openstatus-db-internal-package-for-database-access-and-schema-definitions-consuming-modules-utilize-type-safe-query
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Consuming Modules Utilize Type Safe Query"
metadata:
  actual:
    adr_id: f9c78f29-8d8d-48f8-b9b2-84ea0bed59db
    status: accepted
    candidate_group_id: 6a59cfe8-1e88-4c01-871a-1db91c9d4ce9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Consuming Modules Utilize Type Safe Query

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/**`
- `src/lib/**`

## Context

- Application routes and scheduled routines require consistent access to relational database operations and table entities.
- Maintaining separate database connection configurations and schema definitions across different modules risks contract divergence and connection pool exhaustion.
- An internal workspace package provides centralized database client initialization and shared schema definitions for consuming applications.

## Problem Statement

Decentralized database connections and fragmented schema definitions across diverse API endpoints lead to redundant schema maintenance, connection pool proliferation, and inconsistent data validation contracts.

## Decision

1. SHOULD: Consuming modules SHOULD utilize type-safe query builder operations provided by the shared database client rather than authoring raw, untyped query strings.

## Policy Block

- SHOULD Consuming modules SHOULD utilize type-safe query builder operations provided by the shared database client rather than authoring raw, untyped query strings.

In scope:
- All application route handlers, scheduled tasks, and domain utility modules that query relational storage or reference database entity types.

Out of scope:
- Client-side browser code and external third-party integrations that do not interact with relational persistence.

## Rationale

- Concentrating schema definitions within @openstatus/db/src/schema guarantees that all consumers share identical relational representations and validation models.
- Reusing a centralized managed database client through @openstatus/db ensures consistent connection lifecycle management across serverless and scheduled runtime environments.
- Evidence shows six core files across background task handlers and internal API routes uniformly depending on @openstatus/db and its schema exports.

## Consequences

Positive:
- Establishes a unified source of truth for relational schema definitions, constants, and table models across the workspace.
- Prevents connection pool proliferation and configuration drift across serverless route handlers and scheduled tasks.
- Guarantees compile-time type safety across database query executions and domain utility operations.

Negative:
- Modifications to core database schemas require workspace-wide type verification to prevent downstream compilation failures.
- Application routes and domain utilities remain dependent on the internal workspace database module export structure.

## Alternatives

- Localized database schemas and ad-hoc client connections in individual application directories (rejected)
  Rejected because: Causes duplicate schema declarations, fragmented connection pools, and data contract drift across routes.
  When valid: Only valid in isolated, throwaway prototyping scripts with no shared domain dependencies.
- Direct HTTP or RPC communication to a standalone remote persistence service (rejected)
  Rejected because: Introduces unnecessary operational complexity and network latency for closely integrated workspace services.
  When valid: Valid when persistence services must be isolated across separate infrastructure and organizational boundaries.

## Risks

- Breaking schema changes in the shared database module immediately impact multiple dependent routes across the monorepo.
  Mitigation: Enforce comprehensive automated test coverage and type validation across all workspace packages prior to merging schema modifications.
  Owner: Backend Engineering Team
- Coupling application routes to internal subpath import structures can complicate future package refactoring.
  Mitigation: Maintain stable package entrypoints and encapsulate internal schema directory layouts behind standardized exports.
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
- Ensure newly introduced database table schemas and relational models are defined within the shared database module and exported through public module subpaths before importing them into endpoint handlers.
- Verify that all required environment configuration values for database client initialization are validated prior to executing database queries.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis and type verification scripts to confirm that all database client and schema imports resolve through @openstatus/db.
- Discover and run the repository test suite to verify that database access routines and schema validations pass without regression.

Accept when:
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

## Enforcement

- Verified by: Automated continuous integration pipelines executing workspace static analysis and type validation.
- Verified by: Peer code review verifying that persistence logic and schema references use the designated internal database package.
- Violation handling: Pull requests introducing localized database clients or direct unmanaged connections are blocked from merging until brought into compliance.
- Exception process: Exceptions require submission of an architectural review request and documented approval from the backend engineering lead for standalone scripts requiring isolated connection parameters.