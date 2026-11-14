---
version: 2
name: 9df13e90-5d6c-4032-b4d7-7f84c39c3be7-adoption-of-openstatus-db-src-schema-module-for-centralized-service-layer-domain-and-plan-schemas-service-limit-quota-evaluations-reference-plan
description: "Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Limit Quota Evaluations Reference Plan"
metadata:
  actual:
    adr_id: 9df13e90-5d6c-4032-b4d7-7f84c39c3be7
    status: accepted
    candidate_group_id: 01f978a3-302d-4828-9836-80d54041a4a9
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
      - src/workspace/**
---

# Adoption of @openstatus/db/src/schema Module for Centralized Service Layer Domain and Plan Schemas: Service Limit Quota Evaluations Reference Plan

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/import/**`
- `src/workspace/**`

## Context

- Service layer components require accurate representations of database entities and plan configurations to validate requests and compute plan resource limits.
- Multiple service modules depend on centralized database tables and plan schemas to govern domain entities and import thresholds.
- Consolidating domain schema declarations within the internal database schema module establishes a single source of truth across the workspace.

## Problem Statement

Without a centralized module for database and plan schemas, individual service packages risk defining conflicting data representations and divergent plan limit thresholds, creating data validation inconsistencies and increasing maintenance overhead across the workspace.

## Decision

1. MUST: Service limit and quota evaluations MUST reference plan thresholds and attributes derived directly from centralized plan schema definitions in @openstatus/db.

## Policy Block

- MUST Service limit and quota evaluations MUST reference plan thresholds and attributes derived directly from centralized plan schema definitions in @openstatus/db.

In scope:
- Service layer modules defining input validation, data structures, or plan limit calculations related to database entities.
- Internal workspace packages that consume shared data models from the database package.

Out of scope:
- Isolated client-facing applications operating without direct workspace package access to internal database schemas.
- External third-party integration adapters that require proprietary transformation models decoupled from internal persistence entities.

## Rationale

- Reusing shared schema modules across services guarantees that validation logic and quota checks align with persistence definitions.
- Centralizing plan schemas ensures that service-level limit enforcement operates on authoritative tier configurations.
- Workspace module imports allow immediate type propagation when database structures are updated, preventing silent contract mismatches.

## Consequences

Positive:
- Eliminates redundant data model definitions across service packages.
- Guarantees type-level consistency between database persistence models and service-level validation schemas.
- Centralizes plan definitions and operational limit constraints in a single authoritative module.

Negative:
- Couples service layer compilation to changes in the database schema module structure.
- Modifications to shared database schema definitions can cause cascading compilation errors across consuming service modules.

## Alternatives

- Independent service-level schema declarations (rejected)
  Rejected because: Declaring schema definitions separately inside service packages introduces schema drift risk and requires manual synchronization when database definitions evolve.
  When valid: Valid only when services operate across separate network or repository boundaries that preclude shared workspace package dependencies.
- Code generation of service schemas from database models at build time (rejected)
  Rejected because: Adds build-pipeline orchestration overhead and intermediate artifact management when direct workspace module consumption satisfies type safety requirements.
  When valid: Valid when services are implemented in heterogeneous programming languages requiring language-agnostic code generation.

## Risks

- A breaking change in the database schema module can simultaneously break multiple downstream service modules.
  Mitigation: Utilize automated type checking across all workspace packages to detect breaking schema changes during development and continuous integration.
  Owner: engineering team
- Over-exposing database internal structures to service consumers could lead to unintentional API contract leakage.
  Mitigation: Compose service-specific validation wrappers around shared entities instead of exposing raw persistence types directly to external clients.
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
- Consumers should locate the shared database schema exports in the workspace and import entity schemas and plan definitions directly into service validation and limit logic.
- Composed validation schemas should wrap shared entity fields with service-specific input constraints rather than declaring decoupled data shapes.

## Continuation Context


Verify commands:
- Discover and execute the repository static type checker and linting suite to ensure that all service schema imports resolve to the shared database schema module without unresolved paths.
- Discover and execute the workspace test runner to verify that service-level validation schemas and quota limit checks conform to the shared database and plan schemas.

Accept when:
- All service modules consuming database entities and plan definitions import them directly from the centralized database schema module.
- The project static analysis and test validation suites pass with zero errors across all workspace packages that consume the shared database schema.

## Enforcement

- Verified by: Automated workspace dependency and type checking in continuous integration pipelines.
- Verified by: Peer architectural code reviews evaluating import paths and schema composition patterns.
- Violation handling: Pull requests containing localized database schema redefinitions or duplicate plan structures are blocked at continuous integration.
- Violation handling: Flagged modules must refactor schema dependencies to import from the centralized database schema module before approval.
- Exception process: Exceptions require documented technical justification submitted via pull request review and approval from architectural module maintainers.
- Exception process: All approved exceptions must specify an isolation strategy preventing unvalidated data transfer into persistence layers.