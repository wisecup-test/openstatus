---
version: 2
name: 32b749db-ad2c-4ad4-9ae3-37783e2065a7-t3-oss-env-core-secret-and-environment-variable-validation-application-modules-not-read-runtime-environment
description: "@t3-oss/env-core Secret and Environment Variable Validation: Application Modules Not Read Runtime Environment"
metadata:
  actual:
    adr_id: 32b749db-ad2c-4ad4-9ae3-37783e2065a7
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Application Modules Not Read Runtime Environment

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- Application services require sensitive API tokens, webhook credentials, and service URLs across background checkers, billing handlers, and notification pipelines
- Unvalidated runtime access to configuration properties exposes services to silent undefined values and operational failure during external API requests
- Centralizing credential definitions through strict schema validation enforces type safety and prevents secret leakage across operational boundaries

## Problem Statement

Unstructured and ad-hoc consumption of environment secrets across distributed API handlers causes runtime regressions, obscures missing credentials until invoked, and risks leaking server secrets to unintended operational scopes.

## Decision

1. MUST_NOT: Application modules MUST NOT read runtime environment variables directly outside the centralized environment configuration module.

## Policy Block

- MUST_NOT Application modules MUST NOT read runtime environment variables directly outside the centralized environment configuration module.

In scope:
- All API routers, background workers, and service adapters accessing runtime secrets, API keys, or operational environment configuration

Out of scope:
- Static client-side assets and third-party vendor scripts executed outside the application runtime boundary

## Rationale

- Adopting @t3-oss/env-core provides deterministic schema-driven validation for all external credentials and secrets upon application startup
- Centralized schema parsing eliminates defensive null checks across route handlers and ensures strong type guarantees for downstream external service invocations

## Consequences

Positive:
- Guarantees fail-fast validation of required secrets and credential strings before request processing begins
- Provides inferred static typing and autocomplete across all service routers consuming configuration and credentials
- Prevents accidental leakage of server-only secrets to client-facing boundaries by enforcing explicit schema scoping

Negative:
- Introduces a build-time and initialization-time dependency on schema validation resolution
- Configuration errors halt application startup if required environment variables are omitted during deployment

## Alternatives

- Ad-hoc direct access to process environment variables at call sites (rejected)
  Rejected because: Bypasses runtime schema validation, leading to silent failures, missing secret errors at runtime, and lack of static type inference
  When valid: Only in standalone boot scripts executed prior to module graph initialization
- Custom manual validation assertions at service startup without schema libraries (rejected)
  Rejected because: Increases boilerplate maintenance and lacks cohesive schema composition and typed client-server boundary separation
  When valid: In zero-dependency embedded environments where schema validation libraries cannot be packaged

## Risks

- Application crashes during initialization if required secrets are missing in development or testing environments
  Mitigation: Provide comprehensive environment variable template fixtures and automated schema validation tests across continuous integration pipelines
  Owner: engineering team
- Accidental exposure of server secrets if client and server schemas are co-mingled without strict prefix separation
  Mitigation: Segregate client-facing prefixes from server credentials in the environment schema definition
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
- Define all server secrets within the server schema property of the environment definition function
- Import the validated configuration object directly in consumer modules rather than reading raw process variables

## Continuation Context


Verify commands:
- Discover the repository verification script from the project manifest and execute the test suite to validate schema parsing
- Execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module

Accept when:
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

## Enforcement

- Verified by: Static analysis and lint checks scanning for raw environment variable reads outside the dedicated environment schema module
- Verified by: Automated test suite execution validating schema initialization across test, staging, and production environments
- Violation handling: Pull requests containing unvalidated direct access to environment variables fail continuous integration checks and require remediation
- Violation handling: Deployment pipelines reject application startup when mandated secret declarations fail schema validation constraints
- Exception process: Teams requesting temporary exemptions for unvalidated environment variables must submit an architectural exception documenting risk mitigation and an expedited migration date