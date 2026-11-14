---
version: 2
name: e92a76bc-f4c2-4f57-9e1e-a1f0007531b9-zod-schema-definition-for-notification-payloads-optional-nested-payload-metadata-declared-via
description: "Zod Schema Definition for Notification Payloads: Optional Nested Payload Metadata Declared Via"
metadata:
  actual:
    adr_id: e92a76bc-f4c2-4f57-9e1e-a1f0007531b9
    status: accepted
    candidate_group_id: 34b9637f-6219-4ef8-9a08-9b44f389c821
    evidence:
      source: deep
  scope:
    root: packages/notifications/opsgenie
    paths:
      - src/**
---

# Zod Schema Definition for Notification Payloads: Optional Nested Payload Metadata Declared Via

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/opsgenie`

Applies to:
- `src/**`

## Context

- External alert notification dispatch requires structured payload serialization that satisfies target provider API requirements.
- Notification integration modules interface with core database schemas while constructing provider-specific incident alert structures.
- Absence of runtime validation on outgoing notification payloads risks silent transmission failures, unhandled provider rejections, or malformed alert dispatches.
- The codebase adopts Zod to establish explicit runtime schemas with type inference and fallback defaults for notification payloads.

## Problem Statement

External notification services require strictly shaped payload data containing incident statuses, service descriptions, and severity levels. Without runtime boundary validation and type-safe schema composition, malformed payload dispatch can cause unhandled external API rejections, runtime exceptions during alert serialization, and inconsistent default attributes across notification integration adapters.

## Decision

1. SHOULD: Optional nested payload metadata MUST be declared via z.object and chained with optional modifiers rather than accepting unconstrained object values.

## Policy Block

- SHOULD Optional nested payload metadata MUST be declared via z.object and chained with optional modifiers rather than accepting unconstrained object values.

In scope:
- Notification integration adapters constructing alert payloads for external dispatch.
- Schema definitions validating event severity, message details, and incident metadata.

Out of scope:
- Internal persistence models governed directly by database schema definitions without external provider integration.
- Non-structured notification logging or internal tracing.

Exceptions:
- EXC-20-001: A third-party alert provider accepts arbitrary raw text payloads without structured schema requirements.

## Rationale

- Zod delivers runtime validation coupled with TypeScript static type inference, ensuring that runtime data matches compile-time expectations without duplicate interface definitions.
- Declarative schema composition with z.object and z.enum prevents invalid severity strings or missing message fields from reaching external notification endpoints.
- Incorporating schema-level defaults ensures required provider properties are consistently populated across all alert dispatch calls.

## Consequences

Positive:
- Guarantees deterministic payload structure and prevents malformed alert dispatch to external notification providers.
- Eliminates redundancy by deriving static types directly from runtime validation schemas.
- Fails fast at module boundaries when external alert payloads miss required fields or contain unsupported severity values.

Negative:
- Introduces minor execution overhead during schema parsing and validation before payload transmission.
- Requires ongoing schema maintenance whenever external notification provider APIs update their required payload structures.

## Alternatives

- Relying strictly on TypeScript interface declarations without runtime validation (rejected)
  Rejected because: Static interfaces are erased at compile time and cannot prevent invalid data or missing fields at runtime boundaries.
  When valid: When internal data flow is completely isolated within trusted compile-time boundaries with zero external network dispatch.
- Manual procedural validation using custom guard functions and condition checks (rejected)
  Rejected because: Custom procedural guards introduce repetitive boilerplate, inconsistent error handling, and high maintenance overhead compared to declarative schemas.
  When valid: When third-party library dependencies cannot be added to the execution environment.

## Risks

- External notification provider API changes cause schema validation failures during alert dispatch.
  Mitigation: Implement integration contract tests that validate schemas against provider documentation and monitor validation rejection rates.
  Owner: Notification engineering team
- Mismatched library major versions across workspace packages introduce unexpected schema parsing behavior.
  Mitigation: Follow the Discovery Policy lock-version grounding to verify resolved dependency versions across all workspace packages.
  Owner: Platform engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define payload schemas in dedicated adapter schema modules and export inferred TypeScript types alongside the runtime validators.
- Handle schema parsing errors at the dispatch boundary with descriptive contextual logging to capture malformed payload attempts.

## Continuation Context


Verify commands:
- Discover the repository test runner from project configuration and execute notification payload schema unit tests.
- Discover the project static type analysis runner and execute type validation across notification integration packages.

Accept when:
- All notification payload validation tests pass with zero schema parsing failures.
- Type checking completes with zero diagnostic errors across all schema definitions and inferred types.

## Enforcement

- Verified by: Continuous integration automated test suite execution validating payload schemas.
- Verified by: Static type analysis checks verifying schema and inferred type alignment.
- Verified by: Peer code review for notification integration pull requests.
- Violation handling: Pull requests lacking runtime schema validation for new notification provider payloads will be rejected.
- Violation handling: Test failures resulting from schema parsing errors block merge pipeline.
- Exception process: Submit an architectural review request documenting why runtime schema validation cannot be applied to the specific notification provider.
- Exception process: Obtain explicit sign-off from the notification domain lead before merging.