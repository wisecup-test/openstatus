---
version: 2
name: b4b06d4b-b49c-4b35-9064-eff7215f8648-adoption-of-zod-for-runtime-schema-validation-and-ingestion-boundary-modeling-typescript-boundary-types-domain-contracts-derived
description: "Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Typescript Boundary Types Domain Contracts Derived"
metadata:
  actual:
    adr_id: b4b06d4b-b49c-4b35-9064-eff7215f8648
    status: accepted
    candidate_group_id: e31ac072-3db1-4961-a5ee-464d55ca9a14
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation and Ingestion Boundary Modeling: Typescript Boundary Types Domain Contracts Derived

Status: proposed
Date: 2025-02-25
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `src/**`

## Context

- External telemetry, audit logging events, and analytics ingestion pipelines receive untrusted payloads from disparate application runtimes and client integrations.
- Compile-time type definitions alone cannot defend against schema divergence or malformed JSON inputs delivered at runtime.
- Analytics client configuration parameters and query parameters require strict parsing, type coercion, and defensive defaulting to guarantee predictable datastore queries.

## Problem Statement

Ingesting unvalidated analytics telemetry, client configuration options, and audit log events directly into analytical storage pipelines introduces risks of silent schema corruption, unhandled runtime deserialization exceptions, and structural drift between static type definitions and real-world payload formats.

## Decision

1. SHOULD: TypeScript boundary types and domain contracts SHOULD be derived directly from validation schemas through type inference rather than maintained as separate manual type declarations.

## Policy Block

- SHOULD TypeScript boundary types and domain contracts SHOULD be derived directly from validation schemas through type inference rather than maintained as separate manual type declarations.

In scope:
- All external data ingestion interfaces, telemetry schemas, audit log event contracts, and analytics client configuration parsing.

Out of scope:
- Internal computational utilities operating strictly on previously validated data structures within closed execution contexts.

Exceptions:
- EXC-20-001: High-throughput inner loops where data has already been validated at the system boundary prior to invocation.

## Rationale

- Direct evidence across multiple modules reveals consistent adoption of Zod for defining structured audit log actions, client configuration schemas, and data ingestion models.
- Zod enables runtime validation alongside static type inference, ensuring that runtime contract guarantees mirror compile-time interfaces without dual-maintenance overhead.
- Composable validation transforms allow defensive handling of input anomalies, such as dynamic timestamp fallback generation and safe parsing of serialized string payloads.

## Consequences

Positive:
- Ensures complete type safety and structural integrity across external data ingestion, analytics logging, and client configuration boundaries.
- Single source of truth for runtime validation contracts and inferred compile-time TypeScript type definitions, eliminating schema drift.
- Standardized safe-parsing mechanisms allow graceful error handling and deterministic payload rejection without unhandled runtime crashes.

Negative:
- Validation parsing introduces a small runtime computational cost on high-throughput data ingestion pathways.
- Developers must learn specific schema combination and transformation idioms, such as parse-time dynamic defaults instead of static default evaluation.

## Alternatives

- TypeScript compile-time type assertions and manual interface definitions without runtime validation (rejected)
  Rejected because: Static interfaces are erased at compilation and provide no protection against malformed or unexpected runtime JSON payloads from external boundaries.
  When valid: Valid only for purely internal data structures that never traverse process boundaries, network interfaces, or external configurations.
- Manual imperative conditional validation functions (rejected)
  Rejected because: Handwritten guard statements duplicate type definitions, create high maintenance overhead, and lack composable transformation and safe-parsing capabilities.
  When valid: Valid in resource-constrained environments where schema parsing bundle size must be strictly minimized.

## Risks

- Static evaluation of dynamic defaults causing shared state or stale timestamps across validation runs.
  Mitigation: Enforce code review checks and automated tests verifying dynamic default values utilize parse-time transform methods rather than static default initializers.
  Owner: Engineering Team
- Latency overhead on high-frequency telemetry ingestion paths caused by deep schema validation.
  Mitigation: Profile serialization and validation throughput on heavy ingestion pipelines and isolate validation to entry boundaries.
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
- Compose granular reusable base schemas into higher-level action schemas using object extension and literal discriminators.
- Utilize safe parsing wrappers when handling external serialized strings to inspect validation results without throwing uncaught exceptions.

## Continuation Context


Verify commands:
- Discover the project repository test runner and execute the automated test suites governing boundary schema validation and parsing contracts.
- Discover the project repository static analysis verification tools and execute type checking to ensure boundary interfaces infer types directly from validation schemas.

Accept when:
- All external ingestion payloads and client configuration objects pass validation against their respective Zod schemas with zero unhandled schema parse exceptions.
- Audit log event schemas enforce discriminated string literal action types with matching structured metadata properties across test suites.
- Dynamic timestamp attributes evaluate at invocation time rather than schema module initialization time.

## Enforcement

- Verified by: Automated test suites validating payload acceptance and rejection against schema contracts.
- Verified by: Static analysis and pull request code reviews ensuring external boundary interfaces export and apply Zod validation schemas.
- Violation handling: Continuous integration build failures when boundary endpoints ingest unvalidated external payloads or lack matching schema declarations.
- Violation handling: Mandatory code revision requests during review for any boundary data access bypassing schema parsing.
- Exception process: Submit an architectural exception request documenting the throughput constraints that prohibit runtime parsing.
- Exception process: Provide benchmark evidence showing schema validation constitutes a proven performance bottleneck along with alternative input sanitization verification.