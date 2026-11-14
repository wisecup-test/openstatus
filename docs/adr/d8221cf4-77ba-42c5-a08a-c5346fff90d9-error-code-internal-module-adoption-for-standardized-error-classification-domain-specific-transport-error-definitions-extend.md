---
version: 2
name: d8221cf4-77ba-42c5-a08a-c5346fff90d9-error-code-internal-module-adoption-for-standardized-error-classification-domain-specific-transport-error-definitions-extend
description: "error-code Internal Module Adoption for Standardized Error Classification: Domain Specific Transport Error Definitions Extend"
metadata:
  actual:
    adr_id: d8221cf4-77ba-42c5-a08a-c5346fff90d9
    status: accepted
    candidate_group_id: 3d87529b-a4d8-4737-9420-07ad10e3ad86
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# error-code Internal Module Adoption for Standardized Error Classification: Domain Specific Transport Error Definitions Extend

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/error`

Applies to:
- `src/**`

## Context

- The error subsystem defines foundational error structures, transport-specific HTTP errors, schema validation errors, and diagnostic utilities.
- Decentralized or inconsistent error classification across distinct error classes complicates client error handling and diagnostic tracing.
- Static analysis demonstrates consistent dependency across all error classes and utility modules on a centralized internal error-code module.
- Third-party schema validation libraries integrate into the error subsystem, requiring structured coordination between schema validation failures and centralized error identifiers.

## Problem Statement

Without a centralized module governing error identity, disparate error classes such as base errors, transport-specific errors, and schema validation errors risk introducing duplicate, conflicting, or inconsistent error classifications across the system. This fragmentation degrades runtime diagnostics, complicates downstream error handling for API consumers, and increases maintenance overhead across the error domain.

## Decision

1. SHOULD: Domain-specific and transport-specific error definitions SHOULD extend the base error class while preserving error-code categorization.

## Policy Block

- SHOULD Domain-specific and transport-specific error definitions SHOULD extend the base error class while preserving error-code categorization.

In scope:
- All error class definitions, error subclasses, and error handling utilities within the error subsystem.
- New specialized domain or transport error implementations introduced to the codebase.

Out of scope:
- Third-party library internal error hierarchies operating strictly within external boundaries.
- Isolated testing mocks and transient scripts that do not integrate into the core application error pipeline.

Exceptions:
- EXC-20-001: Integrating external third-party library error objects that cannot be mapped directly to internal error codes

## Rationale

- Centralizing error codes into an internal module guarantees a single authoritative taxonomy across base errors, transport errors, and schema errors.
- Coupling error classes and diagnostic utilities to the error-code module ensures consistent logging and error reporting without brittle string matching.
- Static analysis confirms full convergence across all error modules referencing the error-code module, demonstrating an intentional architectural constraint.

## Consequences

Positive:
- Establishes a single authoritative source of truth for error classification and code definitions.
- Enforces consistent error identity across transport boundaries and schema validation boundaries.
- Facilitates uniform diagnostic logging, error tracing, and automated client error handling.

Negative:
- Introduces a centralized dependency requiring coordinated updates whenever new error classifications are introduced.
- Requires all specialized error implementations to conform strictly to the centralized error code taxonomy rather than defining independent identifiers.

## Alternatives

- Decentralized error code definitions embedded within individual error classes (rejected)
  Rejected because: Introduces identifier collisions, inconsistent client payloads, and eliminates uniform governance over system-wide error classification.
  When valid: Permissible only in isolated standalone utility scripts lacking consumer-facing error contracts.
- Direct exposure of third-party validation library native error types without internal error code binding (rejected)
  Rejected because: Couples consumers directly to third-party library error structures and fragments the error taxonomy across domain and validation failures.
  When valid: Permissible in initial early-stage prototyping environments without structured domain error contracts.

## Risks

- Frequent additions to the centralized error code module causing merge conflicts during concurrent feature development.
  Mitigation: Partition error codes logically by domain namespace within the centralized module to minimize concurrent edit contention.
  Owner: Engineering Team
- Inconsistent external library versioning leading to mismatched error payload structures in schema validation error handlers.
  Mitigation: Enforce strict lock artifact resolution and automated schema error verification suites in continuous integration.
  Owner: Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When authoring specialized error classes, import the centralized error-code module and assign the appropriate registered error code during instance construction.
- Configure diagnostic utilities to inspect constructor metadata and bound error codes to ensure uniform structured telemetry.

## Continuation Context


Verify commands:
- Locate and execute the repository static analysis and type verification suite to confirm all error classes import and reference registered error codes.
- Discover and run the package test runner to execute unit test suites covering base errors, HTTP errors, schema errors, and diagnostic utilities.

Accept when:
- Static analysis and type verification pass without unresolved error-code references across all error modules.
- Unit test suites for all specialized error classes and diagnostic utilities execute and pass with zero failures.

## Enforcement

- Verified by: Continuous integration pipelines executing automated static analysis and type verification suites.
- Verified by: Mandatory peer review on pull requests modifying error definitions or error classification modules.
- Violation handling: Pull requests containing ad-hoc inline error codes or unapproved error classifications will be blocked by continuous integration checks.
- Violation handling: Flagged code must be refactored to register and reference error identities from the centralized error-code module.
- Exception process: Submit an architecture exception request documenting why external or specialized error classifications cannot be registered in the centralized module.
- Exception process: Obtain approval from the architecture review board prior to merging non-standard error structures.