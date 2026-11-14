---
version: 2
name: 31f68206-31be-4470-a0b0-2c656a768d88-zod-schema-validation-for-external-provider-payloads-provider-schemas-apply-transformation-functions-handle
description: "Zod Schema Validation for External Provider Payloads: Provider Schemas Apply Transformation Functions Handle"
metadata:
  actual:
    adr_id: 31f68206-31be-4470-a0b0-2c656a768d88
    status: accepted
    candidate_group_id: 47a971ad-1e20-4235-b2c9-cbdf09abe622
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External Provider Payloads: Provider Schemas Apply Transformation Functions Handle

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/importers`

Applies to:
- `src/providers/**`

## Context

- External third-party API payloads ingested by provider integration clients are untrusted and inherently dynamic.
- Static type declarations alone cannot guarantee that runtime data received over HTTP matches expected compile-time interfaces.
- Integration clients must ingest, sanitize, and normalize external provider data before passing structured representations to downstream internal systems.

## Problem Statement

Integrating with external third-party services exposes the application to unexpected payload mutations, missing fields, and type mismatches that bypass static type checking. Without runtime boundary validation and data normalization, schema drift in external services leads to unpredictable failures within internal core domains.

## Decision

1. SHOULD: Provider schemas SHOULD apply transformation functions to handle missing or optional values at the ingestion boundary rather than propagating nullable states downstream.

## Policy Block

- SHOULD Provider schemas SHOULD apply transformation functions to handle missing or optional values at the ingestion boundary rather than propagating nullable states downstream.

In scope:
- External provider integration clients making network requests to third-party endpoints
- API response payload parsing and contract definition modules at the ingestion boundary

Out of scope:
- Internal intra-service RPC calls where contracts are strictly managed by internal shared schemas
- Internal purely algorithmic functions operating exclusively on internal domain models

Exceptions:
- EXC-42-001: External response is raw binary streaming data where JSON schema validation is not applicable

## Rationale

- The IR evidence demonstrates consistent usage of Zod schemas and runtime parsing across all provider client implementations.
- Validating and transforming external data at the network ingestion boundary isolates downstream modules from third-party schema drift and unexpected payload structures.
- Runtime schema validation provides immediate and deterministic error identification when an external service updates its API contracts unexpectedly.

## Consequences

Positive:
- Guarantees type safety and structural integrity of incoming external payloads before they reach core application logic.
- Encapsulates contract transformations and field normalizations at the boundary layer.
- Provides deterministic debugging information and structured validation errors when external upstream APIs break contracts.

Negative:
- Introduces computational overhead for parsing and validating JSON response payloads at runtime.
- Requires maintaining duplicate contract definitions if external providers already publish machine-readable specifications.
- Causes runtime request failures if an external provider introduces unannounced breaking schema changes.

## Alternatives

- Unchecked type assertion without runtime schema validation (rejected)
  Rejected because: Compile-time type assertions fail to protect against runtime schema drift and unexpected payload mutations from external providers.
  When valid: Only acceptable in rapid internal prototyping when interacting exclusively with mock datasets.
- Manual imperative validation functions per boundary payload (rejected)
  Rejected because: Manual guard checks are error-prone, verbose, difficult to audit, and lack composable transformation capabilities.
  When valid: When external runtime constraints prohibit adding external third-party dependencies.

## Risks

- Provider API breaking changes cause runtime schema parse exceptions and interrupt ingestion pipelines.
  Mitigation: Configure resilient error handling, boundary alerts, and non-blocking failure captures at the provider client boundary.
  Owner: engineering team
- Performance degradation during high-throughput ingestion due to intensive runtime schema parsing.
  Mitigation: Optimize schema structures by avoiding unnecessary regex validations and bench-testing high-volume payloads.
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
- Separate boundary type schemas into dedicated modules distinct from HTTP client transport logic to maximize reusability across client methods.
- Use schema transformation methods to sanitize nullish values and coerce string or numeric identifiers into internal canonical structures directly during parse time.

## Continuation Context


Verify commands:
- Discover and run the project test suite to ensure all provider schema parsing tests pass against simulated external payloads.
- Discover and run the project type checker and static analysis suite to verify strict type inference from parsed schemas.

Accept when:
- All integration test suites pass with validated external payload assertions.
- Static type checking confirms all downstream consumers receive validated and inferred data types without manual type casting.

## Enforcement

- Verified by: Continuous integration test pipelines executing unit and contract validation tests
- Verified by: Peer code reviews checking that all external client endpoints invoke schema parse functions
- Violation handling: Automated CI build failure if external API responses are cast directly without schema validation
- Violation handling: Rejection of pull requests that bypass boundary parsing or fail schema conformance tests
- Exception process: Submit an architectural waiver detailing why boundary schema parsing cannot be performed
- Exception process: Obtain written sign-off from the security and architecture teams prior to merging