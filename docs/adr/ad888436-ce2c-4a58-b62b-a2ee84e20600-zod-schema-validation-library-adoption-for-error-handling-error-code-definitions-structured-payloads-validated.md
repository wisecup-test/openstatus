---
version: 2
name: ad888436-ce2c-4a58-b62b-a2ee84e20600-zod-schema-validation-library-adoption-for-error-handling-error-code-definitions-structured-payloads-validated
description: "Zod Schema Validation Library Adoption for Error Handling: Error Code Definitions Structured Payloads Validated"
metadata:
  actual:
    adr_id: ad888436-ce2c-4a58-b62b-a2ee84e20600
    status: accepted
    candidate_group_id: 43b62c44-b9f5-416e-a824-27861fa6976d
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Zod Schema Validation Library Adoption for Error Handling: Error Code Definitions Structured Payloads Validated

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/error`

Applies to:
- `src/**`

## Context

- The system requires strongly typed and validated error payloads to ensure safe transmission across internal modules and external boundaries.
- Static code analysis reveals consistent reliance on Zod across error codes, schema error constructs, and error diagnostic utilities.
- Adopting a unified schema validation library eliminates discrepancies between static type declarations and actual runtime error objects.

## Problem Statement

Handling structured error payloads and domain error codes across distributed services requires uniform runtime verification and static typing. Without a standardized schema validation library, error structures drift into untyped dictionaries or inconsistent ad-hoc validations, resulting in unverified failure payloads and runtime serialization bugs.

## Decision

1. MUST: Error code definitions and structured error payloads MUST be validated through Zod schema declarations rather than unstructured primitive types.

## Policy Block

- MUST Error code definitions and structured error payloads MUST be validated through Zod schema declarations rather than unstructured primitive types.

In scope:
- Error code registries, schema-validated error classes, and error parsing utilities across error handling packages.
- Runtime validation of structured error payloads and diagnostic context objects.

Out of scope:
- Low-level runtime kernel routines where external schema dependencies introduce cyclic references.
- Unstructured primitive assertions that do not produce domain error constructs.

Exceptions:
- EXC-20-001: Low-level bootstrap environments where external libraries cannot be loaded prior to runtime engine initialization

## Rationale

- The codebase exhibits uniform usage of Zod across error code declarations, schema error definitions, and error utilities, indicating deliberate adoption for structured error governance.
- Deriving static TypeScript types directly from Zod schemas eliminates drift between compile-time contracts and runtime error payloads.
- Centralizing error validation with an established schema library prevents malformed error objects from propagating across service boundaries.

## Consequences

Positive:
- Guarantees structural validity and runtime payload integrity for all custom error classes and error code definitions.
- Maintains synchronization between compile-time static types and runtime schemas through direct type inference.
- Establishes a uniform schema modeling convention across error utilities and reporting pipelines.

Negative:
- Introduces runtime evaluation latency on high-throughput code paths when validating complex error payloads.
- Couples the error handling subsystem directly to the public API and schema mechanics of Zod.

## Alternatives

- Native language type-guard functions and unstructured primitive types (rejected)
  Rejected because: Manual type-guard functions duplicate maintenance efforts between type declarations and validation logic while introducing human error into payload validation.
  When valid: Isolated low-level runtime environments where third-party library dependencies cannot be bundled or resolved.
- Adoption of multiple competing validation libraries across domain packages (rejected)
  Rejected because: Fragmenting schema definitions across multiple validation libraries inflates package footprints and prevents unified schema composition.
  When valid: When distinct sub-ecosystems require framework-mandated schema parsers that cannot interoperate with common libraries.

## Risks

- Excessive validation overhead during high-frequency exception throwing scenarios.
  Mitigation: Design concise schemas limited to necessary diagnostic data and benchmark validation speed along performance-sensitive paths.
  Owner: Engineering Team
- Breaking API alterations across major schema library upgrades.
  Mitigation: Adhere to the lock-version grounding process and encapsulate schema usage behind stable error module interfaces.
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
- Co-locate schema declarations directly with their associated error class definitions to maintain high domain cohesion.
- Export inferred static types directly from the schema definitions so consumers avoid writing redundant interface contracts.

## Continuation Context


Verify commands:
- Discover the project dependency manifest and verify that Zod is declared as a direct dependency for error management modules.
- Discover and execute the project test runner to ensure all error code validation and schema error tests pass.
- Discover and execute the workspace static type checker to confirm zero type discrepancies between Zod schemas and inferred error types.

Accept when:
- All verification test suites covering schema errors, error codes, and error utilities pass without failures.
- Static type verification passes with zero diagnostic errors across Zod schema definitions and their inferred TypeScript types.
- No extraneous or conflicting third-party schema validation libraries are introduced into the error handling module dependency tree.

## Enforcement

- Verified by: Automated continuous integration pipelines executing workspace unit tests and static type checking.
- Verified by: Dependency governance scans auditing workspace manifests to detect unauthorized validation libraries.
- Verified by: Peer code reviews verifying that new error definitions declare and validate payloads using Zod schemas.
- Violation handling: Automated pull request checks will fail if error definitions bypass schema validation or introduce unapproved validation libraries.
- Violation handling: Non-compliant error implementations must be refactored to comply with schema validation constraints before review approval.
- Exception process: Submit an architecture decision variance request explaining the technical constraints preventing schema validation.
- Exception process: Exceptions require written authorization from the architecture review board prior to merging code.
- Exception process: Approved exceptions must be tracked with inline documentation explaining the justification and lifecycle timeline.