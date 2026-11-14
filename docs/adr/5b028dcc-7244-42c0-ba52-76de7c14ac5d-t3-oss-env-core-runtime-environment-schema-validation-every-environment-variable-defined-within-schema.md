---
version: 2
name: 5b028dcc-7244-42c0-ba52-76de7c14ac5d-t3-oss-env-core-runtime-environment-schema-validation-every-environment-variable-defined-within-schema
description: "@t3-oss/env-core Runtime Environment Schema Validation: Every Environment Variable Defined Within Schema"
metadata:
  actual:
    adr_id: 5b028dcc-7244-42c0-ba52-76de7c14ac5d
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Every Environment Variable Defined Within Schema

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- The application API layer requires diverse external configuration inputs including database URLs, authentication credentials, third-party provider tokens, and runtime telemetry flags.
- Direct ambient access to process environment values was detected across multiple router endpoints and utility modules alongside centralized validation schemas.
- Unchecked ambient configuration reads increase the risk of runtime exceptions when required variables are missing or malformed in specific deployment targets.

## Problem Statement

Relying on decentralized, unvalidated reads from ambient process environment variables leads to silent configuration drift, type coercion bugs, and delayed runtime failures in production services when required configuration keys are absent.

## Decision

1. MUST: Every environment variable defined within the schema MUST declare an explicit validation parser and fail fast during initialization if required configuration values are missing or malformed.

## Policy Block

- MUST Every environment variable defined within the schema MUST declare an explicit validation parser and fail fast during initialization if required configuration values are missing or malformed.

In scope:
- All server-side API routers, service adapters, background procedures, and integration clients requiring runtime configuration parameters.
- All environment variables defining credentials, webhooks, region indicators, and third-party API keys.

Out of scope:
- Static assets and pure computational helper utilities that do not consume runtime environment parameters.
- Pre-runtime infrastructure provisioning scripts and container definition manifests.

Exceptions:
- EXC-37-001: Low-level bootstrap scripts executing prior to module system initialization where external dependencies cannot be loaded.

## Rationale

- Validating configuration properties at module initialization ensures that missing or invalid environment variables cause early failure rather than intermittent errors during request processing.
- Using `@t3-oss/env-core` provides a standardized interface for separating server-only secrets from client-safe variables while enforcing strict parsing rules.
- Eliminating direct ambient lookups ensures complete visibility of all infrastructure dependencies through a single source of truth.

## Consequences

Positive:
- Guarantees compile-time type safety and runtime boundary validation for all external configuration inputs.
- Eliminates latent runtime crashes caused by misconfigured or absent environment secrets in downstream handlers.
- Centralizes secret governance and boundary classification between server-only and client-exposed values.

Negative:
- Application initialization will halt immediately if any required environment variable is missing or fails schema parsing.
- Adding or modifying runtime configuration properties requires updating the central schema definition alongside deployment environments.

## Alternatives

- Direct ambient process environment access across all consuming service files (rejected)
  Rejected because: Ambient lookups bypass type validation, allow missing variables to cause late runtime failures, and scatter configuration dependencies across disparate routes.
  When valid: Minimal standalone scripts that execute without a build pipeline or dependency resolver.
- Handcrafted initialization scripts with manual property typecasting (rejected)
  Rejected because: Custom parsing logic requires repetitive validation boilerplate and lacks standardized runtime schema enforcement.
  When valid: Zero-dependency runtime environments where external validation packages cannot be installed.

## Risks

- Strict boot-time validation can prevent application startup in development or staging environments if newly added variables lack default values.
  Mitigation: Define sensible schema defaults or nullable types for non-critical configuration values across test and local environments.
  Owner: engineering team
- Accidental inclusion of sensitive server credentials in client-exposed schema segments.
  Mitigation: Structure the schema into segregated server and client objects enforced by automated lint rules.
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
- Consolidate scattered ambient configuration accesses across service handlers into the primary schema module.
- Ensure all secret tokens, region indicators, and optional operational flags define precise validation schemas and defaults.

## Continuation Context


Verify commands:
- Discover and execute the project configuration validation test suite to confirm schema compliance across all declared environment variables.
- Discover and run the repository static analysis linter to ensure zero ambient process environment queries exist in service router files.
- Execute the project test harness with missing optional and required configurations to verify anticipated failure boundaries and fallback behaviors.

Accept when:
- All environment variable reads in service routers and runtime handlers reference the centralized schema export rather than global process lookups.
- Static configuration analysis and repository validation scripts pass without detecting unvalidated ambient environment queries.
- Missing required environment variables trigger immediate startup validation errors with descriptive diagnostic messages.

## Enforcement

- Verified by: Automated static analysis checks that scan repository service files for unapproved ambient process environment property reads.
- Verified by: Peer code review on all pull requests modifying API router handlers or configuration boundaries.
- Verified by: Runtime initialization test suites verifying schema failure modes under invalid or missing input configurations.
- Violation handling: Build and static analysis pipelines fail immediately when unvalidated ambient environment lookups are introduced.
- Violation handling: Pull requests introducing raw environment variable access without schema mapping are blocked from merging.
- Violation handling: Violations identified during code review must be refactored to consume the typed schema object.
- Exception process: Submit an architecture variance request detailing why an external execution context cannot initialize through the centralized schema module.
- Exception process: Obtain approval from the lead architecture reviewer before deploying any unmanaged ambient environment access.
- Exception process: Track approved variances with a sunset milestone to migrate definitions into the schema.