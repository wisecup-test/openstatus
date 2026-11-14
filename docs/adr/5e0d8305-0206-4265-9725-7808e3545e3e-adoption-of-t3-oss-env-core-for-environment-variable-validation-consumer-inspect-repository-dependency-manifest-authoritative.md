---
version: 2
name: 5e0d8305-0206-4265-9725-7808e3545e3e-adoption-of-t3-oss-env-core-for-environment-variable-validation-consumer-inspect-repository-dependency-manifest-authoritative
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Consumer Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 5e0d8305-0206-4265-9725-7808e3545e3e
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Consumer Inspect Repository Dependency Manifest Authoritative

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/screenshot-service`

Applies to:
- `src/**`

## Context

- Applications require operational configuration values supplied through runtime environment variables across execution environments.
- Direct access to process environment variables without validation introduces risks of unhandled runtime exceptions and misconfigured service deployments.
- The service adopts @t3-oss/env-core to establish schema-validated environment configuration definitions.

## Problem Statement

Accessing unvalidated process environment variables directly throughout application modules leads to hidden runtime exceptions, deferred configuration failure, and silent operational degradation when deployment parameters are misconfigured.

## Decision

1. MUST: The consumer MUST inspect the repository dependency manifest and authoritative lock artifact to resolve the exact locked version of @t3-oss/env-core and verify its API specification before implementation.

## Policy Block

- MUST The consumer MUST inspect the repository dependency manifest and authoritative lock artifact to resolve the exact locked version of @t3-oss/env-core and verify its API specification before implementation.

In scope:
- Service initialization and configuration modules consuming process environment variables
- Application components requiring typed operational parameters at startup

Out of scope:
- Build-time tooling scripts executed outside application runtime contexts
- Low-level platform bootstrap hooks invoked prior to application module evaluation

Exceptions:
- EXC-20-001: A low-level runtime diagnostic profiler requires invocation prior to module system evaluation

## Rationale

- Validating environment variables at application startup ensures immediate fail-fast behavior during deployment rather than deferred runtime crashes during request handling.
- Centralizing environment definitions through @t3-oss/env-core produces typed configuration objects, eliminating redundant parsing logic and stringly-typed configuration bugs.
- Static analysis evidence confirms active adoption of @t3-oss/env-core with schema validation primitives to secure environment input boundaries.

## Consequences

Positive:
- Guarantees fail-fast validation upon service startup when required environment variables are absent or malformed.
- Provides statically inferred configuration types across application modules consuming environment variables.
- Enforces a clean separation between server-only and client-facing runtime configuration.

Negative:
- Introduces external dependency overhead for environment variable parsing during initialization.
- Application startup halts immediately if any declared required environment variable is missing from the execution environment.

## Alternatives

- Direct unvalidated process environment variable access (rejected)
  Rejected because: Lacks type safety and defers configuration failure until missing variables are evaluated at runtime.
  When valid: Small ephemeral utility prototypes where dependency overhead must be zero.
- Handwritten manual environment validation routines (rejected)
  Rejected because: Requires redundant validation boilerplate and error formatting across services that is prone to omission.
  When valid: Environments operating under strict zero-dependency constraints.

## Risks

- Service fails to boot if infrastructure deployments drift or omit newly added required environment variables
  Mitigation: Synchronize deployment infrastructure configuration templates with the environment schema before deploying service updates
  Owner: engineering team
- Breaking API modifications across library revisions disrupting schema parsing behavior
  Mitigation: Follow the mandatory lock-version grounding process to verify schema builder APIs against the locked dependency artifact
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
- Define a single dedicated environment configuration module that exports the validated configuration object for the entire service.
- Ensure all environment validation errors generate human-readable messages detailing missing or invalid variable names during initialization failure.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute the test runner to validate environment configuration parsing.
- Discover the static analysis and type checking script to verify environment schema type inference and variable references across modules.

Accept when:
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

## Enforcement

- Verified by: Automated static type checking and linting pipelines verifying module imports and configuration property access
- Verified by: Continuous integration test suites executing initialization and configuration parsing tests
- Verified by: Peer code reviews auditing source changes to prevent direct unvalidated environment variable access
- Violation handling: Automated build pipeline failure upon detection of direct process environment variable access outside the configuration module
- Violation handling: Rejection of pull requests that introduce unvalidated environment variable consumption
- Exception process: Submit an architectural exception request detailing runtime constraints to the architecture review board for review and approval.