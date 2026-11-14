---
version: 2
name: 1ef58eb7-004d-400b-addd-9ab5f4d144c6-zod-runtime-schema-validation-for-api-procedure-boundaries-service-adapters-domain-functions-not-implement
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Service Adapters Domain Functions Not Implement"
metadata:
  actual:
    adr_id: 1ef58eb7-004d-400b-addd-9ab5f4d144c6
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Service Adapters Domain Functions Not Implement

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- External API clients transmit untrusted network payloads that must be sanitized, constrained, and typed prior to executing business workflows.
- Static type assertions provide no runtime security guarantees once code compiles to runtime environments.
- Multiple procedure endpoints across public and authenticated routers require consistent input boundary validation, type coercion, and error reporting.

## Problem Statement

External requests to API procedure endpoints carry untrusted and unvalidated parameters. Without rigorous, runtime-enforced schema validation at the network perimeter, malformed, unexpected, or malicious data can penetrate service layers, causing runtime exceptions, data corruption, or injection vulnerabilities.

## Decision

1. MUST_NOT: Service adapters and domain functions MUST NOT re-implement redundant perimeter input validation when parameters have already been validated by boundary procedure schemas.

## Policy Block

- MUST_NOT Service adapters and domain functions MUST NOT re-implement redundant perimeter input validation when parameters have already been validated by boundary procedure schemas.

In scope:
- All public and protected procedure definitions accepting parameters from external callers.
- Runtime parsing of dynamic configuration objects and deserialized entity attributes.

Out of scope:
- Internal, trusted in-process function invocations that operate exclusively on previously validated domain types.

## Rationale

- Adopting schema-based validation at procedure boundaries guarantees that only verified and sanitized payloads reach internal service adapters.
- Deriving TypeScript types directly from runtime schemas avoids divergence between compile-time contracts and runtime validation logic.
- Centralizing input parsing via structured schemas standardizes client error responses and simplifies input sanitization logic.

## Consequences

Positive:
- Guarantees that all ingress data reaching business logic conforms to strict domain constraints and data types.
- Eliminates untrusted parameter injection, prototype pollution, and malformed state transitions at the perimeter.
- Provides automatic static type inference for handler functions directly from boundary schema declarations.

Negative:
- Runtime serialization and validation processing incurs compute overhead on ingress network requests.
- Schema structures must be maintained and synchronized alongside internal service and persistence types.

## Alternatives

- Manual runtime boundary checks using conditional guards and native assertion statements (rejected)
  Rejected because: Inconsistent across procedure handlers, prone to human error, and lacks integrated static type inference.
  When valid: Only viable in resource-constrained environments where external validation libraries cannot be bundled.
- Compile-time type assertions without runtime boundary parsing (rejected)
  Rejected because: TypeScript types are completely erased at runtime and provide zero protection against malformed external network payloads.
  When valid: Acceptable only for internal, trusted in-process function calls where data has already been validated at the system boundary.

## Risks

- CPU overhead incurred during runtime schema parsing on latency-sensitive or high-throughput queries.
  Mitigation: Benchmark critical read paths and avoid deep, overly complex schema nesting where minimal primitive validation suffices.
  Owner: engineering team
- Drift between database schema definitions and API input or output validation schemas.
  Mitigation: Utilize shared schema packages or automated schema generation from persistence definitions to maintain consistency.
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
- Define reusable schema fragments for shared domain identifiers, pagination cursors, and enumeration values to prevent validation drift across routers.
- Apply boundary normalizations such as lowercase casing transforms and number coercions directly within schema declarations before passing parsed inputs to downstream services.

## Continuation Context


Verify commands:
- Discover the repository build and type verification script from the project manifest and execute it to confirm schema compilation across all procedure definitions.
- Discover and execute the repository automated test runner to validate that malformed input payloads are rejected and valid payloads are accepted.
- Discover and run the project static analysis linter to ensure all procedure definitions declare input validation contracts without bypasses.

Accept when:
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

## Enforcement

- Verified by: Automated static analysis and type checking integrated into continuous integration pipelines.
- Verified by: Peer review protocols validating that all new or updated procedures declare corresponding schema definitions.
- Violation handling: Continuous integration build failure upon unvalidated input parameters or missing procedure schemas.
- Violation handling: Rejection of pull requests introducing untyped or raw input bindings.
- Exception process: Exceptions require architectural review and explicit sign-off documented within pull request tracking.
- Exception process: Exempted endpoints must document compensating boundary controls ensuring payload sanitization.