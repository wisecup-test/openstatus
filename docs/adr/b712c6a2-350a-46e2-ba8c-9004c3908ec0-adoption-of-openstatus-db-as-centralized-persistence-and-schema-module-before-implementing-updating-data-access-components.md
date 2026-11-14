---
version: 2
name: b712c6a2-350a-46e2-ba8c-9004c3908ec0-adoption-of-openstatus-db-as-centralized-persistence-and-schema-module-before-implementing-updating-data-access-components
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Before Implementing Updating Data Access Components"
metadata:
  actual:
    adr_id: b712c6a2-350a-46e2-ba8c-9004c3908ec0
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Before Implementing Updating Data Access Components

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/**`

## Context

- The application requires reliable, type-safe persistence operations across multiple runtime contexts, including authentication adapters, API route handlers, routing proxies, and operational maintenance scripts.
- Uncoordinated database connection management across disparate application entry points leads to connection exhaustion, divergent schema models, and runtime connection leaks.
- A centralized internal workspace module provides unified database client instantiation, query builder mechanics, and relational schema exports for all dependent services.

## Problem Statement

Direct, decentralized database driver instantiation and duplicated schema definitions across individual application handlers cause connection exhaustion, runtime schema divergence, and inconsistent transaction management.

## Decision

1. MUST: Before implementing or updating data access components, the consumer MUST inspect the repository lock artifact to determine the exact resolved versions of persistence packages and verify API compatibility against official documentation.

## Policy Block

- MUST Before implementing or updating data access components, the consumer MUST inspect the repository lock artifact to determine the exact resolved versions of persistence packages and verify API compatibility against official documentation.

In scope:
- All application route handlers, authentication integrations, proxies, and operational scripts requiring relational persistence or schema definitions.

Out of scope:
- Client-side presentation components, static generation pipelines, and third-party analytical ingestion clients that do not access primary relational persistence.

Exceptions:
- EXC-20-001: Ingestion pipelines utilize dedicated external analytical storage engines with independent telemetry transport drivers.

## Rationale

- Pervasive usage across nine distinct application files confirms that @openstatus/db functions as the single authoritative data access layer.
- Consolidating connection configurations and query building inside an internal package prevents connection pool leaks and reduces boilerplate across service boundaries.
- Centralizing schema entities guarantees compile-time validation for workspace isolation, user profile persistence, and authentication adapters.

## Consequences

Positive:
- Single source of truth for database client instantiation, pooling parameters, and relational schema definitions.
- Enforces compile-time type safety across API route handlers, authentication flows, and operational scripts.
- Prevents database connection proliferation and pool exhaustion across serverless and server-rendered execution contexts.

Negative:
- Consuming modules are tightly coupled to the schema and migration lifecycle of the shared database package.
- Monorepo workspace dependency changes require synchronized builds across all consuming application packages.
- Careful export boundary management is required to avoid leaking server-side connection drivers into edge or client-side runtime environments.

## Alternatives

- Direct Database Driver Instantiation in Consumer Modules (rejected)
  Rejected because: Instantiating database drivers and connection pools within each application module causes connection exhaustion, fragments connection pooling logic, and risks schema divergence.
  When valid: Valid only in standalone, isolated single-script prototypes with no shared domain entities.
- Remote Data Access Gateway Service (deferred)
  Rejected because: Introducing an intermediary HTTP or gRPC data access microservice introduces network overhead and operational complexity that is unnecessary for monorepo-managed applications sharing a database.
  When valid: Valid when heterogeneous runtime platforms unable to execute native language drivers require database access.

## Risks

- Breaking schema modifications in the shared database module could cascade failures across all consuming application components.
  Mitigation: Enforce automated compile-time contract checks and schema migration validations within the continuous integration pipeline.
  Owner: Engineering Team
- Monolithic package exports may inadvertently bundle heavy database drivers into edge or minimal runtime environments.
  Mitigation: Maintain distinct subpath export targets within the database module to separate lightweight schema types from heavier runtime client bundles.
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
- Import schema models and database client instances strictly from designated subpath exports of the internal database package.
- Ensure serverless and edge runtime execution boundaries utilize appropriate connection pooling and adapter configurations exposed by the database module.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and type-checking suites across all workspace packages to verify compliance with @openstatus/db import boundaries.
- Discover and run the project automated integration test suite to validate persistence queries and schema integrity.
- Discover and execute the dependency boundary inspection script to confirm that no unapproved database drivers are imported in application packages.

Accept when:
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

## Enforcement

- Verified by: Automated continuous integration pull request validation checks.
- Verified by: Static analysis and dependency boundary enforcement rules.
- Verified by: Peer architectural code reviews of data access logic.
- Violation handling: Pull requests introducing unauthorized database driver dependencies or bypassing the centralized package are blocked from merging.
- Violation handling: Violations detected in codebase audits require immediate refactoring to route data access through the approved package.
- Exception process: Submit an architectural exception request detailing runtime constraints that prevent consumption of the centralized database package.
- Exception process: Obtain formal review and documented sign-off from designated architectural deciders prior to deploying custom persistence clients.