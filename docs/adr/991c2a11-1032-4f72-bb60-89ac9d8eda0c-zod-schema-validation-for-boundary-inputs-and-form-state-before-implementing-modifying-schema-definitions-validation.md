---
version: 2
name: 991c2a11-1032-4f72-bb60-89ac9d8eda0c-zod-schema-validation-for-boundary-inputs-and-form-state-before-implementing-modifying-schema-definitions-validation
description: "Zod Schema Validation for Boundary Inputs and Form State: Before Implementing Modifying Schema Definitions Validation"
metadata:
  actual:
    adr_id: 991c2a11-1032-4f72-bb60-89ac9d8eda0c
    status: accepted
    candidate_group_id: f6e99943-2e18-4231-91cb-1260300e20ee
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Zod Schema Validation for Boundary Inputs and Form State: Before Implementing Modifying Schema Definitions Validation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/**`

## Context

- External inputs enter the system across client-side form interactions, edge proxy query parameters, and persistent configuration structures.
- Unvalidated inputs introduce risks of runtime exceptions, data corruption, and security bypasses across application layers.
- The application adopts schema-driven validation to enforce structural constraints and runtime typing at untrusted boundaries.

## Problem Statement

External boundaries across client forms, network routing proxies, and configuration loaders receive untrusted and dynamically shaped data. Without systematic runtime schema validation, malformed or malicious inputs can propagate into downstream business logic and data persistence layers, resulting in unpredictable runtime exceptions, state corruption, or security vulnerabilities.

## Decision

1. MUST: Before implementing or modifying schema definitions using the validation library, developers MUST discover the repository lock artifact and verify the exact resolved version of the validation library and associated resolver packages.

## Policy Block

- MUST Before implementing or modifying schema definitions using the validation library, developers MUST discover the repository lock artifact and verify the exact resolved version of the validation library and associated resolver packages.

In scope:
- All data ingestion boundaries receiving untrusted external inputs, including request query parameters and route parameters.
- Interactive user input collection handled via client-side form components.
- Deserialization and consumption of dynamic page configuration objects.

Out of scope:
- Internal function calls operating on validated domain models within trusted module boundaries.
- Static constant definitions and compile-time configuration values.

Exceptions:
- EXC-42-001: High-throughput internal processing where upstream boundaries have established payload validity and schema re-parsing creates measurable latency bottlenecks.

## Rationale

- Centralizing input validation through explicit Zod schemas establishes runtime type safety that matches compile-time static types.
- Utilizing safeParse for network and configuration boundaries ensures defensive parsing without uncaught runtime exception crashes.
- Integrating schema resolvers into form handling maintains consistency between frontend user feedback and backend boundary guarantees.

## Consequences

Positive:
- Ensures runtime data integrity at system entry points, mitigating malformed and unexpected input risks.
- Aligns runtime validation directly with static type definitions, eliminating type drift at boundaries.
- Standardizes validation error reporting across both user interface forms and network endpoints.

Negative:
- Adds minor runtime processing overhead during schema parsing of incoming payloads.
- Requires synchronized maintenance of schema contracts alongside domain model updates.

## Alternatives

- Manual imperative validation functions checking individual fields (rejected)
  Rejected because: Lacks unified static type inference, increases boilerplate, and introduces maintenance overhead as domain models evolve.
  When valid: Minimal standalone utility scripts where schema dependencies cannot be justified.
- Compile-time only type assertions without runtime validation checks (rejected)
  Rejected because: Leaves the application vulnerable to runtime exceptions and malformed external payloads at system boundaries.
  When valid: Internal inter-module calls strictly bounded within the same compiled process boundary.

## Risks

- Performance latency introduced by repetitive parsing of deeply nested payloads.
  Mitigation: Construct targeted, minimal schemas for individual boundary contexts rather than parsing expansive global objects.
  Owner: engineering team
- Schema drift occurring when underlying data structures evolve without corresponding schema updates.
  Mitigation: Co-locate schemas with data models and run automated typecheck verifications to detect contract mismatches.
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
- Define modular schema definitions isolating structural requirements for primitives, nested objects, and collections.
- Favor non-throwing parsing methods at request and proxy boundaries to enable structured error responses and fallback states.

## Continuation Context


Verify commands:
- Discover and execute the repository automated test suite to confirm schema validation rules and error branches behave as expected.
- Execute the project typechecking command to verify type alignment between declared schemas and downstream consumer contracts.
- Run the repository linting and static analysis tool to ensure compliance with schema parsing standards across boundary handlers.

Accept when:
- All schema validation test suites complete with zero failures across boundary handlers and form components.
- The repository typechecking command succeeds without type mismatches between validated schemas and downstream consumer contracts.
- Untrusted inputs arriving at boundary endpoints and user interface forms are verified to pass through schema parsing before processing.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository typechecks and validation test suites on every pull request.
- Verified by: Peer code review verifying that all boundary entry points and interactive forms enforce schema validation.
- Violation handling: Pull requests introducing unvalidated external inputs or untyped assertions at boundaries are blocked from merging until compliant schemas and tests are implemented.
- Exception process: Submit an architectural exception request detailing the performance or structural rationale for bypassing schema validation.
- Exception process: Obtain documented approval from the security and architecture review authorities prior to merging.