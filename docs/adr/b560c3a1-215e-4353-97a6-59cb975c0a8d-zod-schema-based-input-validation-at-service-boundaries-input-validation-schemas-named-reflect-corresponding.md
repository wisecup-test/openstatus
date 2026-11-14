---
version: 2
name: b560c3a1-215e-4353-97a6-59cb975c0a8d-zod-schema-based-input-validation-at-service-boundaries-input-validation-schemas-named-reflect-corresponding
description: "Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Named Reflect Corresponding"
metadata:
  actual:
    adr_id: b560c3a1-215e-4353-97a6-59cb975c0a8d
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Named Reflect Corresponding

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/**`

## Context

- Service functions and agent tools accept heterogeneous external input arguments that must be validated prior to database interaction.
- Relying strictly on static compilation types permits invalid, malformed, or hostile payloads to reach core business logic and persistence transactions at runtime.
- Across 131 service modules, input parameters are systematically passed through schema parsing methods before database queries or mutations occur.

## Problem Statement

External inputs supplied to service functions and agent tools present security and data integrity risks if processed without runtime verification. Static type definitions are erased at runtime, allowing malformed arguments or unintended properties to bypass compile-time checks and execute against database transactions. An architectural mechanism is required to enforce runtime schema compliance, fail fast on invalid payloads, and infer strict static types at service boundaries.

## Decision

1. SHOULD: Input validation schemas SHOULD be named to reflect the corresponding service operation and be co-located or imported from designated schema definitions.

## Policy Block

- SHOULD Input validation schemas SHOULD be named to reflect the corresponding service operation and be co-located or imported from designated schema definitions.

In scope:
- All service function entry points accepting external or caller-provided arguments
- All agent tool interfaces accepting input parameters
- Data modification and query operations triggered through service layer interfaces

Out of scope:
- Internal private helper functions where input has already been validated at the service boundary
- Static constants and hard-coded internal configuration values

Exceptions:
- EXP-42-001: A service endpoint receives an empty or parameterless input payload

## Rationale

- Static type checking alone does not protect runtime execution against untrusted, malformed, or hostile inputs supplied across service boundaries.
- Systematic parsing with Zod schemas at service entry points ensures fail-fast execution before database queries, transactions, or state mutations occur, eliminating injection and type coercion vulnerabilities across 131 service modules.
- Centralizing structural schema validation provides bidirectional type guarantees, linking runtime validation with static compiler types.

## Consequences

Positive:
- Guarantees runtime data integrity and sanitization prior to database query and transaction execution.
- Fails fast at the architectural boundary with structured validation errors, preventing corrupt database writes.
- Derives static TypeScript types directly from runtime validation schemas, eliminating redundant type guards.

Negative:
- Introduces runtime CPU overhead for parsing and validating deeply nested data structures.
- Requires creating and maintaining schema definitions alongside evolving service features.

## Alternatives

- Manual defensive parameter validation using ad-hoc assertion statements (rejected)
  Rejected because: Manual assertions are inconsistently applied, prone to human error, and fail to provide automated static type inference.
  When valid: Isolated internal helper functions with no external caller boundary.
- TypeScript compile-time static type annotations without runtime schema parsing (rejected)
  Rejected because: TypeScript types are erased at runtime and provide zero protection against malformed JSON or dynamic external payloads.
  When valid: Strictly internal functions receiving pre-validated parameters within the same trust boundary.

## Risks

- Schema divergence where service implementation requires fields not captured or incorrectly constrained in the schema.
  Mitigation: Derive static input types directly from schema inferences and validate with automated test suites.
  Owner: Engineering team
- Performance degradation on high-throughput endpoints parsing large, complex payloads.
  Mitigation: Benchmark complex validation trees and optimize schema structures to avoid redundant transformations.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct input validation schemas utilizing object schemas and type-specific primitives matching the expected domain model.
- Invoke the parse method immediately upon entering service handlers so that invalid payloads are rejected before database access or state mutations.
- Export inferred input types from schemas to ensure static type consistency across caller invocations.

## Continuation Context


Verify commands:
- Discover the repository build and type-checking scripts from the package manifest and execute them to verify that all service inputs conform to their inferred schema types.
- Discover the repository test execution script from the package manifest and run the test suite to verify that boundary validation rejects malformed payloads and accepts valid inputs.
- Discover the repository static analysis or lint script from the package manifest and run it to verify that no service handler bypasses input schema parsing.

Accept when:
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and static analysis scripts.
- Verified by: Peer code reviews verifying that all new or modified service entry points parse inputs against defined schemas.
- Violation handling: Pull requests containing unvalidated service entry points or bypassed schema validation are blocked from merging.
- Violation handling: Identified boundary validation omissions must be refactored to introduce schema parsing before deployment.
- Exception process: Exceptions must be documented with architectural justification and approved by the lead architect or security team.