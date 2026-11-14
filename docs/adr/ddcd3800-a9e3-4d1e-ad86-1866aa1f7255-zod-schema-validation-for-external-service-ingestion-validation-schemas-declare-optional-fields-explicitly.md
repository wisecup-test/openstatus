---
version: 2
name: ddcd3800-a9e3-4d1e-ad86-1866aa1f7255-zod-schema-validation-for-external-service-ingestion-validation-schemas-declare-optional-fields-explicitly
description: "Zod Schema Validation for External Service Ingestion: Validation Schemas Declare Optional Fields Explicitly"
metadata:
  actual:
    adr_id: ddcd3800-a9e3-4d1e-ad86-1866aa1f7255
    status: accepted
    candidate_group_id: 69aed0fa-e1bb-4853-925d-e29860e1a826
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - src/fetchers/**
---

# Zod Schema Validation for External Service Ingestion: Validation Schemas Declare Optional Fields Explicitly

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/status-fetcher`

Applies to:
- `src/fetchers/**`

## Context

- External status providers expose heterogeneous HTTP response payloads that require structured normalization before consumption by the platform.
- Compile-time type definitions alone do not safeguard application workflows against breaking changes or malformed JSON responses returned by third-party status endpoints at runtime.
- Multiple fetcher modules implement schema validation at network boundaries to establish perimeter defensive typing.

## Problem Statement

External third-party status APIs can change payload contracts without notice, return malformed structures, or emit unexpected null fields. Relying solely on compile-time types leaves downstream components vulnerable to uncaught runtime exceptions and silent data corruption when external APIs drift.

## Decision

1. SHOULD: Validation schemas SHOULD declare optional fields explicitly using schema modifier methods to prevent missing property errors while maintaining strict validation for defined fields.

## Policy Block

- SHOULD Validation schemas SHOULD declare optional fields explicitly using schema modifier methods to prevent missing property errors while maintaining strict validation for defined fields.

In scope:
- All external status fetcher modules and HTTP client adapters ingesting data from third-party providers.

Out of scope:
- Internal in-memory service interactions and module boundaries already guaranteed by static type checking.

## Rationale

- Observed evidence confirms adoption of Zod schemas directly at network ingestion points across status fetcher modules.
- Validating untrusted external inputs through declarative object and string refinements isolates failures to the boundary and enables structured error handling.
- Deriving static types from runtime schemas ensures consistent contract enforcement without duplicating type and validation logic.

## Consequences

Positive:
- Guarantees runtime type safety and strict schema conformance for all external status data entering the platform.
- Prevents unexpected null values or structural drift in third-party payloads from propagating downstream.
- Provides declarative schema definitions that serve as living contract specifications for status provider integrations.

Negative:
- Introduces runtime CPU parsing overhead during response deserialization across all external status requests.
- Requires schema updates and integration test maintenance whenever external provider APIs modify their payload structures.

## Alternatives

- Manual Type Guards and Custom Validation Functions (rejected)
  Rejected because: Manual validation logic is verbose, error-prone, lacks declarative constraint refinements, and duplicates schema checking code across modules.
  When valid: Valid in minimal environments where external runtime library dependencies cannot be added.
- Compile-Time Type Assertions Without Runtime Validation (rejected)
  Rejected because: TypeScript type assertions are erased at compilation and provide zero protection against runtime schema drift or malformed JSON payloads from third-party APIs.
  When valid: Valid exclusively for trusted internal module boundaries where contracts are guaranteed by the compiler.

## Risks

- Provider API schema changes could cause unexpected runtime validation rejections, halting status ingestion for that provider.
  Mitigation: Implement resilient error handling and boundary telemetry to capture validation failures without crashing background ingestion routines.
  Owner: Platform Engineering
- Deeply nested schema validation could introduce latency during bulk payload processing.
  Mitigation: Constrain validation schemas to strictly required fields and profile parsing overhead under expected ingestion volumes.
  Owner: Platform Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Infer static TypeScript types directly from validation schemas to maintain strict parity between runtime validation and static typing.
- Keep external provider schemas scoped to their respective fetcher modules to avoid cross-provider contract coupling.

## Continuation Context


Verify commands:
- Discover and run the project type checker script defined in the dependency manifest to verify schema type compatibility.
- Discover and run the test suite scripts defined in the repository to validate that fetcher schema parsing passes against mock status payloads.

Accept when:
- The project type checker passes with zero type errors across all external fetcher modules.
- The project test suite passes, verifying that valid provider payloads parse successfully and malformed payloads trigger structured rejection errors.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository validation and test scripts on all pull requests.
- Verified by: Peer code review verifying that all incoming external status payloads define runtime validation schemas.
- Violation handling: Pull requests introducing unvalidated external API response handling are blocked from merging.
- Exception process: Exceptions require documented architectural justification and formal approval from the platform engineering lead.