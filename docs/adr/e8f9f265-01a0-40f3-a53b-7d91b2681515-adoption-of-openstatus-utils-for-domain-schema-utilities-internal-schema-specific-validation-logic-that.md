---
version: 2
name: e8f9f265-01a0-40f3-a53b-7d91b2681515-adoption-of-openstatus-utils-for-domain-schema-utilities-internal-schema-specific-validation-logic-that
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Internal Schema Specific Validation Logic That"
metadata:
  actual:
    adr_id: e8f9f265-01a0-40f3-a53b-7d91b2681515
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Internal Schema Specific Validation Logic That

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `src/schema/**`

## Context

- Database schema modules across distinct domain boundaries require common validation logic, data sanitization primitives, and shared constants.
- Without a centralized utility module, individual domain schemas duplicate schema helpers and validation patterns, leading to divergent validation behavior and maintenance overhead.
- The codebase establishes @openstatus/utils as a shared internal library across schema definitions to provide standardized validation primitives and domain constants.

## Problem Statement

Defining database schemas and input validation across multiple domain entities risks code duplication and inconsistent constraint enforcement when each domain creates ad-hoc utility functions and URL validation schemas. An architectural decision is required to formalize the adoption of a shared utility library across domain schema modules.

## Decision

1. SHOULD_NOT: Internal schema-specific validation logic that has no reuse potential outside a single domain entity SHOULD NOT be exported to @openstatus/utils.

## Policy Block

- SHOULD_NOT Internal schema-specific validation logic that has no reuse potential outside a single domain entity SHOULD NOT be exported to @openstatus/utils.

In scope:
- Database schema definitions, constants, and validation modules requiring shared utility functions or common validation primitives.
- Domain packages defining cross-cutting constants or schemas across domain entities.

Out of scope:
- Self-contained domain entities and private table configurations with zero cross-module utility reuse.
- Standalone services operating outside the shared monorepo workspace dependencies.

Exceptions:
- EXC-20-001: A domain schema requires an isolated utility incompatible with the shared workspace utility interfaces.

## Rationale

- Centralizing shared constants and validation primitives in @openstatus/utils ensures uniform validation rules across all domain entities.
- Reusing established validation helpers reduces redundant code and limits the surface area for validation vulnerabilities in database schemas.
- Standardizing on a single internal utility package establishes clear dependency boundaries between domain schema definitions and cross-cutting utility helpers.

## Consequences

Positive:
- Consistent input validation and sanitization across all database schema domain packages.
- Elimination of redundant schema helpers and duplicate domain constants across domain modules.
- Single locus for updating cross-cutting validation schemas and utility functions.

Negative:
- Domain schema modules depend on the internal utility library, requiring coordinated updates when utility signatures change.
- Potential for utility bloat if domain-specific logic is erroneously elevated into the shared library.

## Alternatives

- Implement local utility functions and validation schemas independently within each domain schema module. (rejected)
  Rejected because: Leads to code duplication, divergent validation rules, and elevated maintenance overhead across domain schemas.
  When valid: Viable only in completely decoupled micro-repositories that share no codebase dependencies.
- Rely exclusively on external third-party utility libraries without an internal abstraction layer. (rejected)
  Rejected because: Fails to provide domain-specific primitives and constants tailored to project data models.
  When valid: Viable when domain models require no specialized validation primitives or cross-cutting project constants.

## Risks

- Breaking changes in shared utility functions or schemas can propagate regressions across multiple domain schema modules.
  Mitigation: Enforce contract testing and continuous integration verification across all consuming packages when updating shared utilities.
  Owner: Database Engineering Team
- Accumulation of domain-specific logic in the shared utility library over time, degrading module cohesion.
  Mitigation: Conduct strict architectural code reviews to ensure only genuinely cross-cutting primitives are introduced into the shared package.
  Owner: Architecture Review Committee

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the package export map and module declaration manifests within the repository to inspect all shared utility functions and schemas exported by @openstatus/utils.
- Ensure that schema validation modules integrate shared utility schemas uniformly without introducing circular workspace dependency graphs.

## Continuation Context


Verify commands:
- # Discover and execute project test suite across database schema packages
- # Discover repository static analysis and type checking scripts, then execute them against schema modules
- # Discover dependency analysis scripts to verify proper workspace linking for @openstatus/utils

Accept when:
- All database schema and validation modules successfully compile and pass static analysis with all shared utility imports resolved from @openstatus/utils.
- No duplicate validation primitives or redundant domain helper implementations exist across schema modules.
- All automated test suites for schema and validation logic pass with zero errors.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type checking and linting workflows.
- Verified by: Mandatory peer code review for all pull requests modifying schema definitions or shared utilities.
- Violation handling: Pull requests introducing redundant schema utilities or circumventing @openstatus/utils are blocked from merging.
- Violation handling: Violations identified during code review require refactoring to consume the centralized shared utility exports.
- Exception process: Submit an architectural exception request detailing why the shared utility package cannot satisfy the module requirements.
- Exception process: Approval must be granted by the lead architect or schema domain maintainer prior to merge.