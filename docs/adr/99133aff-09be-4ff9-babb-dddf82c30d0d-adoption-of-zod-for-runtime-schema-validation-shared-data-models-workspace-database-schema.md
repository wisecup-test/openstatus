---
version: 2
name: 99133aff-09be-4ff9-babb-dddf82c30d0d-adoption-of-zod-for-runtime-schema-validation-shared-data-models-workspace-database-schema
description: "Adoption of Zod for Runtime Schema Validation: Shared Data Models Workspace Database Schema"
metadata:
  actual:
    adr_id: 99133aff-09be-4ff9-babb-dddf82c30d0d
    status: accepted
    candidate_group_id: fbf2fe87-05df-42ab-85d5-be2130d0b8c8
    evidence:
      source: deep
  scope:
    root: packages/notifications/grafana-oncall
    paths:
      - src/**
---

# Adoption of Zod for Runtime Schema Validation: Shared Data Models Workspace Database Schema

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/grafana-oncall`

Applies to:
- `src/**`

## Context

- External notification integration payloads arrive over network boundaries and require validation prior to processing.
- Static type systems verify types only at compile time and do not enforce type safety on dynamic external input at runtime.
- Notification modules define schemas utilizing Zod object definitions alongside shared workspace database schema entities.

## Problem Statement

External notification webhooks and integration payloads enter the system as untrusted runtime data. Relying exclusively on compile-time static type definitions leaves applications vulnerable to runtime failures and unpredictable state when external systems transmit malformed, missing, or altered fields. A declarative, composable runtime validation mechanism is required to guarantee structural compliance at integration boundaries while keeping static types synchronized.

## Decision

1. SHOULD: Shared data models from workspace database schema packages SHOULD be composed with notification integration schemas to ensure consistency across data persistence boundaries.

## Policy Block

- SHOULD Shared data models from workspace database schema packages SHOULD be composed with notification integration schemas to ensure consistency across data persistence boundaries.

In scope:
- Boundary handlers and notification integration modules receiving untrusted external data payloads
- Schema definition modules responsible for parsing external alert and notification structures

Out of scope:
- Internal trusted communications between isolated memory spaces within the same execution context
- Static configuration constants that do not ingest external runtime data

## Rationale

- External integration payloads arrive as untrusted data over network boundaries, requiring deterministic runtime validation before reaching core notification logic.
- Zod provides composable, declarative object schemas that eliminate runtime type confusion by validating and parsing input data structures simultaneously.
- Inferring static types directly from runtime schemas prevents drift between compile-time type definitions and actual payload shapes.

## Consequences

Positive:
- Guarantees runtime type safety and fail-fast rejection of malformed or unexpected external payloads.
- Establishes a single source of truth for runtime validation logic and static type definitions, reducing synchronization overhead.
- Defines clear declarative contracts for incoming integration payloads including alerting states and identifiers.

Negative:
- Introduces runtime validation performance overhead during payload parsing.
- Requires defining and maintaining explicit schemas for every external payload variation.

## Alternatives

- Manual conditional type checking and assertion functions (rejected)
  Rejected because: Manual type guard functions are verbose, error-prone, difficult to maintain, and fail to provide automated static type inference.
  When valid: When payload structures are minimal single-scalar values with zero nesting.
- Compile-time static type casting without runtime validation (rejected)
  Rejected because: Type assertions erase at runtime, allowing malformed or unexpected external payloads to cause downstream unhandled exceptions.
  When valid: When data originates from fully trusted internal memory boundaries.

## Risks

- Performance degradation if extremely large or deeply nested payloads undergo redundant validation passes.
  Mitigation: Validate untrusted payloads once at the integration boundary before passing validated data to downstream handlers.
  Owner: engineering team
- Upstream third-party integration changes could send unexpected schema variants, causing schema parse rejections.
  Mitigation: Design schemas with appropriate optionality for non-critical attributes and capture schema parse errors with descriptive logging.
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
- Construct schema definitions using z.object composed with specific field validators and enum constraints to reflect required payload structures.
- Export inferred static types alongside schema definitions to ensure downstream consumers use strongly typed representations of parsed payloads.
- Handle parse errors gracefully at the ingress boundary to return structured error responses without leaking internal stack traces.

## Continuation Context


Verify commands:
- Discover the project test runner configuration from the repository and execute test suites targeting schema validation modules.
- Discover the project type-checking configuration and execute the static type verification script across all integration packages.
- Discover the repository linting and schema validation check tasks to verify compliance with architectural constraints.

Accept when:
- All unit tests exercising schema parsing for valid and invalid payloads pass without failures.
- Static type checking completes with zero diagnostic errors across schema modules.
- All external payload ingestion points execute runtime schema validation prior to processing.

## Enforcement

- Verified by: Automated continuous integration pipelines executing type checking and schema unit test suites
- Verified by: Peer code reviews verifying that incoming integration payloads are validated through Zod schemas
- Violation handling: Pull requests bypassing runtime validation or using unvalidated type assertions must be blocked until compliant schemas are introduced.
- Violation handling: Static analysis or build failures resulting from schema type mismatches will prevent deployment.
- Exception process: Exceptions for performance-critical high-throughput internal boundaries require review and documented approval by the architecture team.