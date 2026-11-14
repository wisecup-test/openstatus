---
version: 2
name: 86a0d4ea-ae85-42a6-b36a-ae118de287f2-zod-schema-validation-for-external-api-client-boundaries-external-client-requests-pass-explicit-authentication
description: "Zod Schema Validation for External API Client Boundaries: External Client Requests Pass Explicit Authentication"
metadata:
  actual:
    adr_id: 86a0d4ea-ae85-42a6-b36a-ae118de287f2
    status: accepted
    candidate_group_id: 2a48e15c-71e7-4ab2-94a0-84daf33ebd71
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Zod Schema Validation for External API Client Boundaries: External Client Requests Pass Explicit Authentication

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/importers`

Applies to:
- `src/providers/**`

## Context

- External provider integrations ingest data across remote network boundaries with varying payload stability.
- Provider client implementations fetch data over HTTP using authentication headers and structured JSON formats.
- Unvalidated external payloads expose internal domain services to runtime crashes and silent data corruption when remote contracts change.

## Problem Statement

External provider APIs are untrusted external boundaries that can change payload structures without notice, causing unhandled runtime exceptions or subtle data corruption inside core business logic when response payloads are consumed without strict structural verification.

## Decision

1. SHOULD: External API client requests SHOULD pass explicit authentication and content negotiation headers alongside schema parsing boundaries.

## Policy Block

- SHOULD External API client requests SHOULD pass explicit authentication and content negotiation headers alongside schema parsing boundaries.

In scope:
- External API client integrations that fetch data from third-party or remote HTTP endpoints.
- Boundary modules responsible for ingesting, adapting, or importing remote provider data.

Out of scope:
- Internal service-to-service calls with compile-time generated RPC contracts.
- Static configuration loaders that do not cross remote network boundaries.

Exceptions:
- EXC-26-001: High-throughput binary or raw streaming endpoints where schema parsing introduces prohibitive latency or memory overhead.

## Rationale

- Direct schema parsing via Zod guarantees runtime type safety at the network boundary where external contracts are prone to third-party drift.
- Failing fast via schema validation prevents malformed remote data from propagating into internal domain workflows.
- Standardizing on schema parsing across provider clients provides uniform error signaling and contract visibility across integration boundaries.

## Consequences

Positive:
- Guarantees that external payload structures conform strictly to expected contracts before processing.
- Detects third-party API contract drift immediately at the boundary interface.
- Enables static type inference directly aligned with validated runtime data structures.

Negative:
- Introduces runtime CPU parsing latency for every incoming external network response payload.
- Requires ongoing maintenance of schema definitions when third-party provider APIs introduce non-breaking schema additions.

## Alternatives

- Compile-time type assertions without runtime validation (rejected)
  Rejected because: Type assertions are erased during compilation and provide no protection against malformed or drifting third-party API payloads.
  When valid: Internal module boundaries where types are statically guaranteed and do not cross remote network boundaries.
- Manual imperative field-by-field boundary verification (rejected)
  Rejected because: Manual property checks are error-prone, verbose to maintain, and fail to provide unified schema-driven type inference.
  When valid: Endpoints with a single scalar return value where an additional schema definition introduces disproportionate overhead.

## Risks

- Unannounced third-party API response schema drift causing unexpected parse rejections in production.
  Mitigation: Design schemas to strip unexpected non-critical properties and maintain regression test fixtures validating provider payloads.
  Owner: Integration Engineering Team
- Performance degradation during bulk ingestion due to deep recursive schema parsing.
  Mitigation: Benchmark high-volume payloads and scope schema validation to strictly required structural fields.
  Owner: Integration Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define provider response schemas in dedicated contract modules co-located with client implementations, isolating schema declarations from transport logic.
- Invoke schema.parse immediately upon receiving decoded JSON responses from external network calls prior to domain transformation.

## Continuation Context


Verify commands:
- Discover and run the repository test runner targeting external integration client suites to verify payload parsing and error handling.
- Discover and execute the repository static analysis linter to confirm that external client fetch responses pass through schema validation.

Accept when:
- All external client integration tests pass, confirming schema validation executes on network response payloads.
- No unvalidated external response payloads are exposed directly through provider client interfaces.
- Schema parsing rejections yield structured validation errors rather than unhandled raw network exceptions.

## Enforcement

- Verified by: Automated test suites covering provider client network response parsing.
- Verified by: Peer code review for all new and modified external API client integration pull requests.
- Verified by: Static analysis rules verifying boundary response data handling.
- Violation handling: Pull requests omitting runtime schema validation on external HTTP client responses are blocked from merging until schemas are added.
- Violation handling: Discovered unvalidated external client boundaries are logged as technical debt defects for remediation.
- Exception process: Submit an architectural exception request detailing throughput constraints or alternative boundary protections to the integration architecture lead for review and approval.