---
version: 2
name: 70a834b0-ad2d-40fc-a5f2-12f97737ea98-t3-oss-env-core-environment-variable-schema-validation-environment-variable-schemas-declared-validated-oss
description: "@t3-oss/env-core Environment Variable Schema Validation: Environment Variable Schemas Declared Validated Oss"
metadata:
  actual:
    adr_id: 70a834b0-ad2d-40fc-a5f2-12f97737ea98
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Environment Variable Schemas Declared Validated Oss

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/analytics`

Applies to:
- `**` (whole root)

## Context

- Runtime configuration values and sensitive API credentials were raw string properties accessed directly through runtime environment sources without build-time or runtime type validation.
- Lack of structured validation risked deploying applications with missing or misconfigured credentials, causing runtime crashes during execution rather than failing fast at module evaluation.
- The codebase introduced @t3-oss/env-core paired with zod to declare strict schemas for client-accessible and server-restricted environment variables.

## Problem Statement

Unvalidated runtime environment variables allow misconfigurations, missing credentials, and unintentional exposure of sensitive secrets to client-facing contexts without failing fast at application startup.

## Decision

1. MUST: Environment variable schemas MUST be declared and validated using @t3-oss/env-core with zod validators at module boundary initialization.

## Policy Block

- MUST Environment variable schemas MUST be declared and validated using @t3-oss/env-core with zod validators at module boundary initialization.

In scope:
- Modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

Out of scope:
- Static constants and configuration values that do not depend on runtime environment variables or deployment-specific secrets.

## Rationale

- Adopting @t3-oss/env-core guarantees that configuration errors surface deterministically at startup rather than during downstream runtime requests.
- Pairing @t3-oss/env-core with zod enables type inference and runtime parsing over environment variables, preventing undefined or malformed secret values.
- Separating server secrets from client-exposed variables mitigates the risk of secret leakage into public client bundles.

## Consequences

Positive:
- Fail-fast initialization prevents runtime crashes by detecting missing or malformed environment variables at startup.
- Static type inference provides typed access to configuration keys across all importing packages.
- Clear boundary segregation prevents accidental leakage of server-only secrets into client-accessible environments.

Negative:
- Module initialization incurs execution overhead for schema parsing and validation during startup.
- Developers must maintain and synchronize explicit schema declarations alongside infrastructure configuration.

## Alternatives

- Direct unvalidated access to runtime environment properties (rejected)
  Rejected because: Lacks type safety and schema validation, failing unpredictably at point of use rather than during application initialization.
  When valid: Valid only in minimal standalone scripts where zero external dependencies is an operational constraint.
- Manual TypeScript type assertions without runtime validation schemas (rejected)
  Rejected because: TypeScript type assertions are erased at compile time and provide no runtime protection against missing or malformed environment variables.
  When valid: Valid when external orchestration systems strictly guarantee environment variables prior to process invocation.

## Risks

- Failure to define mandatory environment variables halts application boot in production deployments.
  Mitigation: Validate environment definitions against deployment configuration templates in pre-deployment integration pipelines.
  Owner: Engineering Team
- Schema drift between deployment secrets and application validation schemas.
  Mitigation: Maintain synchronization between deployment credential registries and schema definitions through automated verification.
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
- Construct environment configuration modules by importing the schema generator from @t3-oss/env-core and declaring type validators using zod for each expected variable.
- Export the validated configuration object as the single point of entry for consuming modules rather than exposing direct references to runtime environment properties.

## Continuation Context


Verify commands:
- Discover the project test script from the dependency manifest and execute the test runner to verify environment schema parsing.
- Discover and run the project type checking script to ensure exported environment configurations satisfy static type signatures.
- Discover and execute the project linting and boundary verification scripts to confirm no direct accesses to unvalidated runtime environment variables remain.

Accept when:
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

## Enforcement

- Verified by: Continuous integration type checking and test execution pipelines.
- Verified by: Static analysis and lint checks for direct references to unvalidated runtime environment sources.
- Verified by: Peer code review on pull requests touching environment configurations.
- Violation handling: Pull requests that introduce direct runtime environment property access or omit schema definitions are blocked from merging.
- Violation handling: Continuous integration pipelines fail if environment validation schemas fail parsing.
- Exception process: Submit an architectural review request with written justification explaining why environment schema validation cannot be applied to the specific module.
- Exception process: Obtain explicit approval from the technical lead before bypassing validated environment configuration modules.