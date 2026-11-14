---
version: 2
name: ec3ea61e-14e1-4587-9e39-d49bd5a55e3a-zod-runtime-schema-validation-for-notification-payloads-before-implementing-updating-zod-schema-definitions
description: "Zod Runtime Schema Validation for Notification Payloads: Before Implementing Updating Zod Schema Definitions"
metadata:
  actual:
    adr_id: ec3ea61e-14e1-4587-9e39-d49bd5a55e3a
    status: accepted
    candidate_group_id: 43f4a8eb-d729-4c10-9edf-7ad646f9d166
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Runtime Schema Validation for Notification Payloads: Before Implementing Updating Zod Schema Definitions

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/ntfy`

Applies to:
- `src/**`

## Context

- Notification delivery modules ingest serialized payload configurations originating from external triggers and persistence stores.
- External notification service clients require structured parameters with expected field types and default connection configurations.
- Unvalidated JSON parsing exposes downstream dispatch logic to unhandled type errors and malformed external requests.

## Problem Statement

Notification integration modules receive serialized payload data from persistent storage and external triggers. Without runtime validation, malformed or incomplete payload structures can lead to unexpected runtime crashes, silent delivery failures, or malformed requests to downstream external notification endpoints.

## Decision

1. MUST: Before implementing or updating zod schema definitions, consumers MUST inspect the repository dependency lock artifact to determine the exact resolved version of zod and verify API compatibility against version-specific documentation.

## Policy Block

- MUST Before implementing or updating zod schema definitions, consumers MUST inspect the repository dependency lock artifact to determine the exact resolved version of zod and verify API compatibility against version-specific documentation.

In scope:
- Ingress handling and processing of serialized notification payload data across notification provider integrations
- Runtime configuration validation for external notification service dispatchers

Out of scope:
- Internal static constant definitions and compile-time-only type declarations
- Low-level network transport serialization protocols

## Rationale

- Adopting zod provides strict runtime structural validation for serialized payload data received from persistent stores or external inputs, preventing malformed payloads from reaching delivery dispatch logic.
- Enforcing validation at the boundary via explicit zod schema parsing guarantees type correctness at runtime, mitigating runtime exceptions when accessing nested configuration fields.
- Centralizing payload validation in explicit schema definitions decouples data integrity verification from external client transport execution.

## Consequences

Positive:
- Guarantees runtime schema adherence and prevents malformed notification data from triggering unexpected runtime errors or downstream service failures.
- Provides deterministic error handling when notification data structures fail validation before network requests are initiated.
- Centralizes validation logic and default configuration values within declarative schema definitions.

Negative:
- Introduces minor runtime processing overhead during JSON parsing and schema validation.
- Requires ongoing schema synchronization whenever external notification provider payloads or configurations change.

## Alternatives

- Manual runtime type checks and ad-hoc property assertions (rejected)
  Rejected because: Manual type assertions are error-prone, verbose, difficult to maintain, and fail to provide declarative structural parsing with automated defaults.
  When valid: Small utility scripts with single-field validations where third-party dependencies are strictly disallowed.
- Compile-time type assertions without runtime validation (rejected)
  Rejected because: Static compile-time types are erased during compilation and cannot prevent runtime failures when parsing deserialized dynamic payloads.
  When valid: Internal statically typed modules where data never crosses serialization or process boundaries.

## Risks

- Schema drift between external provider notification payloads and internal zod definitions causing valid notifications to be rejected.
  Mitigation: Maintain comprehensive integration tests against mock provider payloads and document schema changes during provider updates.
  Owner: engineering team
- Unhandled schema parsing exceptions causing notification worker termination on malformed input.
  Mitigation: Enclose boundary parse calls in dedicated error handling logic to capture and log validation errors gracefully without crashing workers.
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
- Construct zod schemas in dedicated schema modules to allow reuse across delivery workers and test fixtures.
- Ensure payload parsing occurs immediately after JSON deserialization and before invoking external client request methods.

## Continuation Context


Verify commands:
- Discover the project test runner script from the repository manifest and execute the test suite validating payload schema enforcement.
- Discover the repository linter script from the root configuration and execute static analysis across notification integration modules.

Accept when:
- All notification payload validation tests pass successfully with valid and invalid input fixtures.
- Static analysis and type checking pass without violations across notification modules.

## Enforcement

- Verified by: Automated continuous integration test suite validating schema rejection of invalid payloads
- Verified by: Peer code review verifying boundary parsing prior to external client calls
- Violation handling: Pull requests bypassing zod schema parsing on external notification payloads are blocked from merge until compliant validation is implemented.
- Exception process: Formal architectural review and approval documented in an exception ticket detailing why runtime schema validation cannot be applied.