---
version: 2
name: 3e6f385a-5e07-4f3a-95f7-f99ba64e653d-adoption-of-t3-oss-env-core-for-environment-variable-validation-developers-inspect-repository-lock-artifact-determine
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Developers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 3e6f385a-5e07-4f3a-95f7-f99ba64e653d
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Developers Inspect Repository Lock Artifact Determine

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/bird-whatsapp`

Applies to:
- `src/**`

## Context

- Runtime configuration requires predictable environment variable injection across workspace packages.
- Direct undeclared environment variable access creates runtime failure risks when configurations are missing or malformed.
- The codebase demonstrates adoption of @t3-oss/env-core with zod schema validation for defining and validating configuration parameters.

## Problem Statement

Unvalidated runtime environment variable access allows missing or malformed configuration to propagate silently until execution time, leading to runtime failures that are difficult to diagnose.

## Decision

1. MUST: Developers MUST inspect the repository lock artifact to determine the exact resolved version of @t3-oss/env-core before authoring or modifying environment validation definitions.

## Policy Block

- MUST Developers MUST inspect the repository lock artifact to determine the exact resolved version of @t3-oss/env-core before authoring or modifying environment validation definitions.

In scope:
- Workspace packages and modules requiring runtime environment variable configuration.

Out of scope:
- Pure utility functions and algorithmic modules that do not consume external configuration.

## Rationale

- Evidence demonstrates @t3-oss/env-core paired with zod string validators to enforce strict validation on environment variables at load time.
- Centralizing environment definitions through @t3-oss/env-core provides fail-fast guarantees at process startup rather than encountering missing values during active execution.
- Generating static types from zod schemas ensures consistent typing across consumer modules.

## Consequences

Positive:
- Configuration errors are detected immediately during process initialization before dependent services accept traffic.
- Type systems provide static compile-time checking for validated environment variables.
- Expected environment variables are documented declaratively in structured schemas.

Negative:
- Introducing schema dependencies adds runtime footprint and setup complexity to package initialization.
- Local execution requires explicit configuration matching declared schemas before process startup.

## Alternatives

- Direct unvalidated runtime environment property access (rejected)
  Rejected because: Leaves processes vulnerable to missing or misconfigured runtime values that fail unpredictably during execution.
  When valid: Isolated standalone scripts without external configuration dependencies.
- Custom manual environment validation routines (rejected)
  Rejected because: Requires bespoke boilerplate validation logic and manually maintained type interfaces that drift out of synchronization with runtime reality.
  When valid: Constrained execution contexts where external dependencies cannot be introduced.

## Risks

- Process crash during initialization if non-critical environment variables fail schema validation.
  Mitigation: Ensure optional environment variables are explicitly declared with fallback defaults or nullable zod schemas.
  Owner: Engineering Team
- Schema divergence across workspace packages if environment configurations are duplicated.
  Mitigation: Export reusable configuration schemas from shared internal libraries when configurations overlap.
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
- Define environment schemas using @t3-oss/env-core by separating server-side and client-side variables as required by execution boundaries.
- Compose schema validators using zod primitives including z.string() to enforce shape, presence, and format constraints.

## Continuation Context


Verify commands:
- Discover and run the project static type checker to verify that environment schema definitions compile without diagnostic errors.
- Discover and execute the package test suite to verify that invalid environment configurations throw validation errors at startup.

Accept when:
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites and static type verification.
- Verified by: Peer code review verifying that environment variable access conforms to schema-backed definitions.
- Violation handling: Continuous integration builds will fail if environment variable schemas do not pass static type checking or test validation.
- Violation handling: Direct access to unvalidated environment properties identified during code review will require refactoring to use the validated schema.
- Exception process: Exceptions must be documented in code review with architecture lead approval when zero-dependency constraints prevent external schema libraries.