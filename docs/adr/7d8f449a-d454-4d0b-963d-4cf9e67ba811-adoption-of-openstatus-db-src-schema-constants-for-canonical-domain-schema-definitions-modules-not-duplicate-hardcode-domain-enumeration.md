---
version: 2
name: 7d8f449a-d454-4d0b-963d-4cf9e67ba811-adoption-of-openstatus-db-src-schema-constants-for-canonical-domain-schema-definitions-modules-not-duplicate-hardcode-domain-enumeration
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Modules Not Duplicate Hardcode Domain Enumeration"
metadata:
  actual:
    adr_id: 7d8f449a-d454-4d0b-963d-4cf9e67ba811
    status: accepted
    candidate_group_id: 8d8829c5-85f5-4150-b1de-f3946448d484
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
      - src/lib/**
---

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Modules Not Duplicate Hardcode Domain Enumeration

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/checker/cron/**`
- `src/lib/**`

## Context

- Multiple application modules and background job processors require access to canonical domain constants and schema definitions.
- Subsystems including probe runners, mock utilities, and scheduled task handlers must validate domain objects such as monitor regions against identical schemas.
- Inconsistent local definitions of domain constants across boundary layers lead to runtime contract failures and validation divergence.

## Problem Statement

Divergent or duplicated declarations of core domain constants and validation schemas across distributed application layers lead to contract drift, runtime parsing errors, and inconsistent validation behavior.

## Decision

1. MUST_NOT: Modules MUST NOT duplicate or hardcode domain enumeration values or identifier schemas outside the centralized schema constants definition.

## Policy Block

- MUST_NOT Modules MUST NOT duplicate or hardcode domain enumeration values or identifier schemas outside the centralized schema constants definition.

In scope:
- All application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

Out of scope:
- Independent third-party integration adapters that do not manipulate internal domain entities or region definitions.

## Rationale

- Adopting a centralized schema constants module guarantees that all subsystems share an identical understanding of domain primitives such as regions and monitor states.
- Consuming shared constants directly within validation schemas enables immediate compile-time and runtime detection of contract changes across all consumer modules.

## Consequences

Positive:
- Eliminates data contract drift between database models and runtime validation boundaries
- Centralizes domain enumerations and region primitives in a single authoritative source of truth
- Ensures consistent runtime validation and error reporting across probes, mock runners, and background execution services

Negative:
- Tightly couples application validation layers to the internal database schema package structure
- Changes to shared schema constants can trigger cascading rebuilds or type-checking requirements across all consuming packages

## Alternatives

- Local re-declaration of domain constants and schema primitives within each service or module (rejected)
  Rejected because: Introduces schema drift, inconsistent validation rules, and maintenance overhead across consumers
  When valid: When modules operate in an entirely isolated repository with zero shared runtime contracts
- Exporting constants through ambient global type declarations without runtime validation schemas (rejected)
  Rejected because: Fails to provide runtime validation guarantees and type-safe validation boundary parsing
  When valid: When the architecture does not require runtime schema parsing or validation at service boundaries

## Risks

- Breaking changes to shared schema constants can simultaneously invalidate multiple application layers
  Mitigation: Enforce semantic versioning and deprecation lifecycles on shared schema constant modifications
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
- Import canonical schema definitions and enumerations from the shared schema constants module when constructing composite validation schemas.
- Avoid circular dependencies by ensuring the schema constants module remains independent of consuming application layer logic.

## Continuation Context


Verify commands:
- Discover and execute the repository module boundary lint checks to ensure all domain constants are imported from the designated shared module.
- Discover and run the project type validation and test suites to verify that schema composition resolves without errors across all consumer packages.

Accept when:
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

## Enforcement

- Verified by: Automated static analysis and module boundary lint rules in continuous integration.
- Verified by: Peer code review of data modeling and validation schema declarations.
- Violation handling: Automated continuous integration checks reject pull requests containing duplicated domain constant declarations.
- Violation handling: Violations must be refactored to import from the canonical shared constants module.
- Exception process: Submit an architectural exception request detailing why domain definitions must diverge from the centralized schema constants module.
- Exception process: Obtain approval from the domain architecture review team prior to merging.