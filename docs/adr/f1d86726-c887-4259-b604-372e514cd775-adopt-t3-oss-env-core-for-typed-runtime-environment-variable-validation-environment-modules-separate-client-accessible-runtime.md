---
version: 2
name: f1d86726-c887-4259-b604-372e514cd775-adopt-t3-oss-env-core-for-typed-runtime-environment-variable-validation-environment-modules-separate-client-accessible-runtime
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Modules Separate Client Accessible Runtime"
metadata:
  actual:
    adr_id: f1d86726-c887-4259-b604-372e514cd775
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Modules Separate Client Accessible Runtime

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/email`

Applies to:
- `**` (whole root)

## Context

- Unvalidated access to runtime environment variables across application packages introduces runtime failures when required keys or configuration values are omitted or misconfigured.
- Centralizing and strictly validating environment configuration at package boundaries ensures that missing credentials or invalid runtime modes are caught at initialization rather than during downstream execution.
- Adopting a dedicated environment schema library guarantees type safety and static typing for runtime configuration parameters across services.

## Problem Statement

Direct, unvalidated access to raw process environment variables risks silent failures, runtime crashes, and credential leaks when mandatory configuration values or secrets are absent or fail runtime type expectations. Without formal schema enforcement at module initialization, configuration errors are deferred until runtime execution, complicating debugging and compromising service reliability.

## Decision

1. SHOULD: Environment modules SHOULD separate client-accessible runtime configuration from server-only secrets within the schema declaration.

## Policy Block

- SHOULD Environment modules SHOULD separate client-accessible runtime configuration from server-only secrets within the schema declaration.

In scope:
- Packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

Out of scope:
- Pure utility libraries and static type definition modules that operate without external runtime configuration or environment dependencies.

## Rationale

- Validating environment variables at module initialization with @t3-oss/env-core surfaces configuration errors immediately during startup rather than causing intermittent runtime failures during downstream execution.
- Typed environment schemas provide compile-time guarantees and editor autocompletion for configuration values, reducing typographical errors and improper secret handling.
- Isolating raw environment access to designated configuration boundaries simplifies auditing and prevents sensitive keys from leaking across unverified application layers.

## Consequences

Positive:
- Guarantees fail-fast behavior at startup whenever required runtime environment variables or credentials are absent or invalid.
- Provides type-safe configuration objects with complete static typing derived directly from schema definitions.
- Eliminates redundant type-casting and manual parsing of raw environment variables throughout business logic.

Negative:
- Introduces startup failure overhead if non-critical optional environment variables are incorrectly marked as mandatory.
- Adds a runtime dependency on schema validation and environment management libraries to consuming packages.

## Alternatives

- Direct unvalidated access to raw process environment variables (rejected)
  Rejected because: Leaves configuration keys untyped, allows missing required variables to cause silent runtime failures, and scatters environment references across application files.
  When valid: Trivial standalone scripts with no external dependencies and minimal configuration requirements.
- Custom hand-rolled validation functions using conditional assertions (rejected)
  Rejected because: Requires boilerplate assertion code across every package, lacks standardized type inference, and increases maintenance burden compared to declarative schemas.
  When valid: Environments where external library dependencies cannot be added due to extreme bundle or execution constraints.

## Risks

- Mismatched or missing environment keys in deployment environments cause unexpected application startup termination.
  Mitigation: Document required environment variables in deployment templates and validate schemas during continuous integration testing.
  Owner: Engineering Team
- Developers bypass the validated environment module and read raw process environment variables directly.
  Mitigation: Enforce static analysis linting rules that prohibit direct process environment variable access outside configuration modules.
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
- Define all server-side secrets and client-accessible environment variables in a single dedicated configuration module per package.
- Export the validated configuration object as the sole access point for environment values across the package.

## Continuation Context


Verify commands:
- Discover and run the project typecheck script from the project manifest to verify that all environment variable accesses resolve against the validated configuration object.
- Discover and run the project test script from the project manifest with sample environment values to verify startup schema validation passes.
- Discover and run the project linter to confirm that direct access to process environment variables is restricted to configuration modules.

Accept when:
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

## Enforcement

- Verified by: Automated continuous integration checks executing static typechecking and linting suites on every pull request.
- Verified by: Peer code review verifying that new environment variables are declared in the schema module.
- Violation handling: Pull requests containing direct raw environment variable reads or undeclared configuration properties will fail continuous integration and be blocked from merging.
- Exception process: Exceptions for scripts or test harness files must be formally documented with inline linter exception directives and approved by module owners.