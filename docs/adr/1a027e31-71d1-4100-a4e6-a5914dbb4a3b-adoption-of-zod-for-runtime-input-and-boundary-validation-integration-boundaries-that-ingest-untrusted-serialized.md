---
version: 2
name: 1a027e31-71d1-4100-a4e6-a5914dbb4a3b-adoption-of-zod-for-runtime-input-and-boundary-validation-integration-boundaries-that-ingest-untrusted-serialized
description: "Adoption of Zod for Runtime Input and Boundary Validation: Integration Boundaries That Ingest Untrusted Serialized"
metadata:
  actual:
    adr_id: 1a027e31-71d1-4100-a4e6-a5914dbb4a3b
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Integration Boundaries That Ingest Untrusted Serialized

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/pagerduty`

Applies to:
- `src/**`

## Context

- External integration modules receive serialized configuration and event payloads from datastores and external webhooks that may drift or contain malformed structures.
- Relying on implicit type assumptions for deserialized boundary data leads to runtime type errors, unhandled promise rejections, and invalid outbound network requests.
- The codebase establishes runtime input validation using Zod to enforce schema contracts on configuration objects, environment variables, and outbound client dispatch payloads.

## Problem Statement

External integration boundaries frequently ingest untrusted, dynamically serialized data and dispatch payloads to third-party endpoints. Without runtime validation at ingestion and dispatch points, structural drift, missing required properties, or invalid types in serialized data cause runtime crashes, corrupt downstream states, and generate malformed external API requests.

## Decision

1. MUST: All integration boundaries that ingest untrusted serialized data, read environment configuration, or construct external network payloads MUST validate data structures using Zod schemas prior to consumption or dispatch.

## Policy Block

- MUST All integration boundaries that ingest untrusted serialized data, read environment configuration, or construct external network payloads MUST validate data structures using Zod schemas prior to consumption or dispatch.

In scope:
- Deserialization and ingestion of untrusted input data from databases or external sources.
- Parsing and enforcement of environment configuration at module initialization.
- Preparation and validation of outbound event payloads prior to external HTTP client dispatch.

Out of scope:
- Pure internal functions operating on already-validated domain entities within secure memory boundaries.
- Performance-critical low-level primitives where data structure integrity is guaranteed by prior pipeline stages.

## Rationale

- Zod provides composable, declarative schema definitions that validate runtime data while inferring static types, eliminating discrepancy between runtime validation and compile-time contracts.
- Executing schema parsing immediately following deserialization isolates data validation errors at the architectural boundary before invalid data enters internal domain workflows.
- Validating outbound event structures guarantees that payloads match the external service contract before initiating network requests, reducing failed API transactions.

## Consequences

Positive:
- Eliminates runtime exceptions stemming from malformed or unexpected serialized data structures.
- Ensures consistency between runtime validated structures and static type definitions throughout boundary modules.
- Prevents malformed outbound requests to third-party service endpoints by asserting payload compliance before network transmission.

Negative:
- Introduces runtime computational overhead for parsing and validating deeply nested data structures.
- Requires ongoing maintenance of schema definitions alongside external third-party API contract updates.

## Alternatives

- Manual conditional type checks and ad-hoc boundary assertion guards (rejected)
  Rejected because: Manual type guards are error-prone, verbose, and drift easily from evolving integration contracts without providing inferred static typing.
  When valid: When operating in micro-constrained environments where external dependencies cannot be bundled.
- Static compile-time type assertions without runtime validation (rejected)
  Rejected because: Compile-time types are erased at runtime and cannot prevent runtime crashes when external services or datastores provide malformed serialized payloads.
  When valid: When data structures originate entirely from trusted internal memory boundaries that have already been validated.

## Risks

- Synchronous parsing of large or deeply nested payloads can increase CPU utilization and latency during high-throughput event processing.
  Mitigation: Design schemas to validate only necessary boundary fields and optimize schema composition.
  Owner: Engineering Team
- Strict schema validation may cause unhandled exceptions and drop valid notifications if an upstream external provider introduces additive non-breaking schema changes.
  Mitigation: Employ permissive schema strategies for uninspected auxiliary fields and wrap parse calls in structured error-handling blocks.
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
- Define domain boundary schemas in dedicated schema modules to facilitate reuse across ingestion handlers, environment loaders, and external client dispatchers.
- Couple schema parsing directly with deserialization logic so that raw unstructured objects are converted into validated domain structures at the earliest ingress point.

## Continuation Context


Verify commands:
- Discover the project dependency manifest and lock artifact to identify the registered test and typecheck scripts, then execute the suite to verify that all boundary schemas compile without errors.
- Discover and run the project static analysis and linting verification suite to ensure all input ingestion boundaries invoke Zod schema parse methods on serialized payloads.
- Execute the project automated test suite to validate that malformed input payloads trigger expected validation failures and prevent execution of external client dispatches.

Accept when:
- All boundary handlers parsing serialized incoming data or configuring external client dispatches pass automated test suites and reject malformed schemas with validation errors.
- Static analysis and typecheck suites complete with zero errors across all modules declaring and parsing Zod boundary schemas.
- The resolved library version matches the documented API usage confirmed from the authoritative dependency lock artifact.

## Enforcement

- Verified by: Automated test suites executing schema validation against valid and invalid boundary payloads.
- Verified by: Peer code reviews verifying that all deserialization points pass through schema parse methods.
- Verified by: Automated static analysis checks enforcing absence of unvalidated type assertions on external input.
- Violation handling: Pull requests omitting Zod validation at untrusted input boundaries will be blocked during code review.
- Violation handling: Runtime schema validation failures must log structured error details and halt further processing of invalid payloads.
- Exception process: Any bypass of runtime schema validation requires written architectural review detailing performance constraints and proving upstream verification.