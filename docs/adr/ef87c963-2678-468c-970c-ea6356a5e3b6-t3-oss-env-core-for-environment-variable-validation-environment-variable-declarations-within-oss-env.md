---
version: 2
name: ef87c963-2678-468c-970c-ea6356a5e3b6-t3-oss-env-core-for-environment-variable-validation-environment-variable-declarations-within-oss-env
description: "@t3-oss/env-core for Environment Variable Validation: Environment Variable Declarations Within Oss Env"
metadata:
  actual:
    adr_id: ef87c963-2678-468c-970c-ea6356a5e3b6
    status: accepted
    candidate_group_id: 31da84ba-1169-423b-8506-9a385a0a9140
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# @t3-oss/env-core for Environment Variable Validation: Environment Variable Declarations Within Oss Env

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/pagerduty`

Applies to:
- `src/**`

## Context

- Runtime configurations require deterministic schema validation to ensure that required external service credentials and endpoints are present prior to service startup.
- Unvalidated environment variable access can lead to silent failures or unexpected runtime exceptions deep within notification processing flows.
- The codebase exhibits environment variable declaration using @t3-oss/env-core with zod validation in notification module configuration.

## Problem Statement

Without centralized, type-safe environment configuration validation, missing or malformed environment values propagate unhandled through application workflows, resulting in runtime errors that are difficult to diagnose.

## Decision

1. MUST: Environment variable declarations within @t3-oss/env-core MUST define explicit zod schemas for all expected runtime variables.

## Policy Block

- MUST Environment variable declarations within @t3-oss/env-core MUST define explicit zod schemas for all expected runtime variables.

In scope:
- Modules defining runtime configuration and external service integration credentials

Out of scope:
- Static build-time constants and client-side assets not consuming runtime environment variables

## Rationale

- Static schema definition via @t3-oss/env-core centralizes environment variable parsing and guarantees type safety across consuming code.
- Pairing @t3-oss/env-core with zod validation fails application initialization early upon missing or malformed configuration, preventing partial runtime execution.
- Evidence indicates adoption within notification integration code, demonstrating the necessity of validating external service credentials.

## Consequences

Positive:
- Guarantees that missing or invalid environment variables are trapped during initialization rather than during active message dispatch.
- Provides type-safe access to environment variables without manual string casting or runtime assertions.

Negative:
- Introduces external dependencies on @t3-oss/env-core and zod for configuration loading.
- Increases startup validation overhead and requires explicit schema updates whenever environment requirements change.

## Alternatives

- Direct access to raw process environment variables (rejected)
  Rejected because: Leaves configuration untyped and defers missing variable detection until execution time.
  When valid: Prototyping scripts or lightweight utility modules where schema dependencies are unnecessary.
- Custom hand-written validation functions without third-party schema libraries (rejected)
  Rejected because: Increases maintenance burden and duplicates parsing logic across individual packages.
  When valid: Environments with strict zero-dependency constraints.

## Risks

- Failure to validate optional environment variables properly may trigger unexpected initialization failures.
  Mitigation: Clearly distinguish between required and optional environment properties in the schema.
  Owner: engineering team
- Schema divergence across independent packages may lead to inconsistent validation rules for shared environment variables.
  Mitigation: Standardize environment schema definitions and share reusable schemas across package boundaries where applicable.
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
- Define environment schemas in dedicated configuration modules and export the validated environment object for consumption.
- Use zod string validators to enforce constraints on URLs, access tokens, and communication endpoints required by integrations.

## Continuation Context


Verify commands:
- Discover and execute the project test runner across all packages containing environment configuration modules.
- Discover and run the project type-checking and linting suites to ensure all environment variable usages reference the validated configuration object.

Accept when:
- All environment configuration test suites pass without validation or runtime errors.
- Static type checking passes with zero type errors related to environment variable references.

## Enforcement

- Verified by: Automated continuous integration checks executing test and type validation suites.
- Verified by: Peer code review verifying that direct process environment access is avoided in favor of validated configuration schemas.
- Violation handling: Pull requests containing direct unvalidated environment variable access or failing schema validation tests are blocked from merging.
- Exception process: Exceptions for isolated bootstrapping scripts must be documented in code review with sign-off from technical leads.