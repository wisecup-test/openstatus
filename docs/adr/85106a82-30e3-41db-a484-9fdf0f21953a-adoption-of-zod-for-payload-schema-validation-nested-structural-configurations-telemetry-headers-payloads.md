---
version: 2
name: 85106a82-30e3-41db-a484-9fdf0f21953a-adoption-of-zod-for-payload-schema-validation-nested-structural-configurations-telemetry-headers-payloads
description: "Adoption of zod for Payload Schema Validation: Nested Structural Configurations Telemetry Headers Payloads"
metadata:
  actual:
    adr_id: 85106a82-30e3-41db-a484-9fdf0f21953a
    status: accepted
    candidate_group_id: 4e2a1a98-2292-4ad7-96f3-b44c0c9979ca
    evidence:
      source: deep
  scope:
    root: packages/utils
    paths:
      - src/**
---

# Adoption of zod for Payload Schema Validation: Nested Structural Configurations Telemetry Headers Payloads

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/utils`

Applies to:
- `src/**`

## Context

- Shared data payloads passed between execution pipelines require runtime validation and structural integrity guarantees beyond compile-time type checking.
- Inconsistent payload structures and unvalidated input boundaries can introduce runtime exceptions during monitoring execution and telemetry dispatch.
- Defining centralized schema contracts allows services to parse incoming payload structures, enforce strict enumeration values, and apply consistent fallback default values.
- Adoption of a schema validation library provides both runtime parsing and static type inference from a single source of truth.

## Problem Statement

Distributed services coordinating monitoring execution lack a unified runtime mechanism to validate payload shapes, resulting in potential schema drift, missing required attributes, and undefined defaults across subsystem boundaries.

## Decision

1. SHOULD: Nested structural configurations and telemetry headers in payloads SHOULD be modeled using z.object and z.record to maintain explicit key-value constraints.

## Policy Block

- SHOULD Nested structural configurations and telemetry headers in payloads SHOULD be modeled using z.object and z.record to maintain explicit key-value constraints.

In scope:
- Shared utility modules defining payload contracts and input schemas.
- Payload validation logic at service ingress and egress boundaries.

Out of scope:
- Internal component local state management where data does not cross subsystem boundaries.
- Low-level transport streaming buffers where raw binary serialization is utilized.

## Rationale

- Observed schema definitions in shared payload modules demonstrate centralized usage of zod object schemas with explicit enums and default assignments.
- Utilizing zod enables declarative validation with built-in support for default fallbacks and strict structural typing across boundaries.
- Centralizing schema contracts minimizes payload parsing discrepancies and prevents runtime malformed payload processing.

## Consequences

Positive:
- Runtime payload validation guarantees structural correctness before processing monitoring events.
- Default assignments through schema modifiers ensure consistent fallback parameters across environments.
- Static types can be directly derived from schemas, preventing drift between validation rules and type annotations.

Negative:
- Parsing complex nested payload schemas incurs a minor runtime validation overhead at execution boundaries.
- Upgrading or modifying schema definitions requires synchronization across all dependent consumer services.

## Alternatives

- Manual validation functions with conditional assertions (rejected)
  Rejected because: Handcrafted validation is error-prone, verbose, and difficult to keep synchronized with static type definitions.
  When valid: Trivial single-field validation where third-party runtime overhead cannot be tolerated.
- Compile-time-only interface declarations without runtime schema validation (rejected)
  Rejected because: Compile-time interfaces are erased at build time and cannot protect against malformed payloads received across process boundaries.
  When valid: Fully homogeneous monolithic codebases where all data flows are guaranteed internally by the compiler.

## Risks

- Performance overhead when parsing high-frequency payload streams through nested schemas
  Mitigation: Design schema structures to avoid redundant parsing iterations and validate primarily at ingress boundaries
  Owner: Engineering Team
- Schema version mismatch when producers and consumers run on desynchronized deployments
  Mitigation: Design payload schemas with backward-compatible optional fields and explicit default modifiers
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
- Compose complex payload schemas from shared atomic building blocks and common enumeration constants.
- Ensure that optional configuration records utilize explicit key and value schema definitions to prevent untyped mapping.

## Continuation Context


Verify commands:
- Discover the project test runner from repository configuration and execute the test suite covering payload validation schemas.
- Discover the repository type checking script and execute static analysis across all shared contract packages to ensure schema-derived types match expectations.

Accept when:
- All schema validation test cases execute and pass successfully without runtime parsing errors on valid payloads.
- Type checking verifies without errors across all modules importing the payload schemas.

## Enforcement

- Verified by: Automated continuous integration pipeline executing schema test suites and static type verification.
- Verified by: Peer code review verifying that all new cross-boundary payloads are governed by declarative validation schemas.
- Violation handling: Pull requests introducing unvalidated payload contracts or bypassing schema validation must be rejected.
- Violation handling: Runtime validation failures must be logged with structured validation issues and rejected at boundary entry.
- Exception process: Exceptions require architectural review and documented justification for why runtime schema validation cannot be applied.