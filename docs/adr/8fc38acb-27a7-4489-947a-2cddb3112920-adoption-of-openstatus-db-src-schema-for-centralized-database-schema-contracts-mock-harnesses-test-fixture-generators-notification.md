---
version: 2
name: 8fc38acb-27a7-4489-947a-2cddb3112920-adoption-of-openstatus-db-src-schema-for-centralized-database-schema-contracts-mock-harnesses-test-fixture-generators-notification
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Mock Harnesses Test Fixture Generators Notification"
metadata:
  actual:
    adr_id: 8fc38acb-27a7-4489-947a-2cddb3112920
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Mock Harnesses Test Fixture Generators Notification

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/slack`

Applies to:
- `src/**`

## Context

- Notification dispatch services require access to persistent entity structures and schema definitions to validate and format outbound payloads.
- Automated mock harnesses and test utilities require structural parity with production payloads to ensure reliable integration verification.
- The codebase establishes internal module boundaries where @openstatus/db/src/schema acts as the centralized schema definition provider across workspace packages.

## Problem Statement

Decentralized or duplicate schema definitions across notification integrations and their corresponding mock harnesses risk data contract drift, silent payload parsing failures, and divergence between test fixtures and production database records. A centralized module contract is required to govern schema consumption across dispatchers and test harnesses.

## Decision

1. MUST: Mock harnesses and test fixture generators for notification integrations MUST consume identical entity schemas from @openstatus/db/src/schema to guarantee fixture parity with production handlers.

## Policy Block

- MUST Mock harnesses and test fixture generators for notification integrations MUST consume identical entity schemas from @openstatus/db/src/schema to guarantee fixture parity with production handlers.

In scope:
- Notification integration packages consuming database entities or notification payloads.
- Mock utilities, test fixtures, and testing harnesses simulating notification data structures.

Out of scope:
- External client payload schemas specific to third-party endpoints that do not map to internal database records.
- Standalone utility modules that operate independently of database entities or notification data models.

## Rationale

- Importing @openstatus/db/src/schema establishes a single canonical source of truth for notification data structures across both runtime handlers and mock harnesses.
- Shared schema imports eliminate manual type synchronization between database models, notification handlers, and test fixtures.
- Centralizing database schemas within @openstatus/db/src/schema enables compiler-enforced contract verification across workspace boundaries.

## Consequences

Positive:
- Enforces strict data model alignment between runtime notification handlers and mock test harnesses.
- Surfaces breaking database schema alterations at compile time across all dependent notification packages.
- Eliminates redundant entity type declarations across discrete notification integration modules.

Negative:
- Couples notification integration packages directly to the internal database schema module.
- Modifications to shared database models necessitate coordinated validation and rebuilds across dependent notification packages.

## Alternatives

- Local schema duplication within each notification integration package (rejected)
  Rejected because: Duplicating schema models locally introduces data contract drift and maintenance overhead between persistent database entities and outbound notification handlers.
  When valid: Valid only in isolated standalone repositories unable to link against shared workspace packages.
- Publishing database schemas as an independent external registry package (rejected)
  Rejected because: Introduces release coordination overhead and version latency for schemas co-developed within the internal workspace.
  When valid: Valid when notification services are hosted in independent external repositories requiring public semantic versioning.

## Risks

- Database schema refactorings in @openstatus/db/src/schema may inadvertently break dependent notification payload parsing.
  Mitigation: Enforce automated workspace-wide type checking and test execution on all pull requests modifying schema definitions.
  Owner: Engineering Team
- Deep imports into internal package directory paths could expose unstable implementation details.
  Mitigation: Restrict imported identifiers to exported schema interfaces and models, avoiding unexported helper functions.
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
- When introducing new notification entity structures, export the corresponding schemas from @openstatus/db/src/schema prior to consumption in notification integration modules.
- Ensure mock utilities and test fixture generators reference exported schema definitions from @openstatus/db/src/schema to preserve contract parity with runtime handlers.

## Continuation Context


Verify commands:
- Discover the workspace configuration to locate validation scripts and run the static type checker across notification packages.
- Discover and execute the module dependency linting script to verify that database schema imports originate solely from @openstatus/db/src/schema.
- Execute the discovered test execution script for notification packages to verify mock harness compatibility against schema definitions.

Accept when:
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

## Enforcement

- Verified by: Continuous integration pipelines verifying static types and module import boundaries.
- Verified by: Code review verification confirming that database entity structures are imported from @openstatus/db/src/schema rather than declared locally.
- Violation handling: Build and verification scripts will reject modules declaring local duplicates of database schema structures.
- Violation handling: Discovered violations must be refactored to import canonical contracts from @openstatus/db/src/schema prior to integration.
- Exception process: Exceptions require written approval from backend architecture maintainers.
- Exception process: Any approved exception must document why shared module consumption is infeasible and provide an automated verification plan for preventing schema drift.