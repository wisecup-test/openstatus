---
version: 2
name: 53905af0-21ba-40ab-a48a-1b1f144fd323-environment-variable-schema-validation-with-t3-oss-env-core-environment-configuration-schemas-separate-server-only
description: "Environment Variable Schema Validation with @t3-oss/env-core: Environment Configuration Schemas Separate Server Only"
metadata:
  actual:
    adr_id: 53905af0-21ba-40ab-a48a-1b1f144fd323
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Environment Configuration Schemas Separate Server Only

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/db`

Applies to:
- `**` (whole root)

## Context

- Runtime configuration sources traditionally rely on unvalidated process environment variables, introducing risks of missing secrets and runtime initialization failures.
- Database connectivity and execution environments require strict validation of connection strings, authentication credentials, and deployment flags.
- Adopting declarative schema validation prevents silent failures and propagates strongly typed configuration throughout the package boundary.

## Problem Statement

Direct, unvalidated access to process environment variables leads to silent configuration drift, delayed runtime failures during database connection attempts, and lack of type certainty for critical infrastructure credentials.

## Decision

1. SHOULD: Environment configuration schemas SHOULD separate server-only secrets from client-accessible configuration variables within the schema definition.

## Policy Block

- SHOULD Environment configuration schemas SHOULD separate server-only secrets from client-accessible configuration variables within the schema definition.

In scope:
- Configuration management and environment variable access within backend and database workspace packages.
- Initialization of runtime credentials, database connection parameters, and operational environment flags.

Out of scope:
- Static build-time constants that do not vary across runtime environments.
- Third-party external configurations managed entirely outside the application runtime boundary.

## Rationale

- @t3-oss/env-core combined with zod provides compile-time and module-initialization validation, ensuring that missing or malformed environment variables fail fast before operational workloads execute.
- Centralizing environment variables into a validated configuration module eliminates scattered direct references to process environment properties, reducing security risks associated with unvalidated secrets handling.
- Strongly typed configuration exports guarantee that consumers interact with verified values without requiring repeated runtime defensive checks.

## Consequences

Positive:
- Immediate fail-fast error reporting when required environment variables or secrets are omitted or malformed at module initialization.
- Guaranteed type safety and autocomplete for all configured environment variables across consuming modules.
- Centralized auditability for all external configuration sources and secrets required by the package.

Negative:
- Requires explicit schema updates whenever new environment variables are introduced.
- Prevents application startup in testing or development environments unless valid configuration values or mock overrides are supplied.

## Alternatives

- Direct unvalidated access to process environment variables (rejected)
  Rejected because: Lacks type safety, allows silent runtime errors when required credentials are missing, and scatters configuration references throughout the codebase.
  When valid: Small isolated scripts where external dependency overhead is strictly prohibitive.
- Ad-hoc manual validation functions per variable (rejected)
  Rejected because: Creates inconsistent error messaging, duplicates parsing logic, and fails to provide cohesive schema typing across packages.
  When valid: Environments where external validation libraries cannot be installed or bundled.

## Risks

- Module initialization failures in automated test or build environments due to missing mock configuration values.
  Mitigation: Define clear schema defaults or supply test environment fixtures in test execution lifecycles.
  Owner: engineering team
- Schema drift between local development configurations and production deployment manifests.
  Mitigation: Enforce continuous verification against documented deployment configuration specifications during review pipelines.
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
- Define all package-level environment variables within a dedicated configuration definition utilizing @t3-oss/env-core and export the parsed object as the sole access point.
- Apply specific zod schemas for each variable to enforce format correctness, string non-emptiness, and domain-specific configuration constraints.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and type verification suite to confirm that all references to environment variables utilize the validated configuration object.
- Discover and run the repository test suite to verify that invalid or missing environment configurations trigger module-load validation errors as expected.

Accept when:
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

## Enforcement

- Verified by: Static analysis and linting checks prohibiting direct process environment access outside the designated configuration module.
- Verified by: Automated continuous integration build checks validating module initialization and schema compliance.
- Verified by: Peer code review for any new configuration parameter introductions.
- Violation handling: Pull requests introducing direct process environment access or unvalidated configuration reads are blocked until reconciled with the environment schema.
- Exception process: Exceptions require documented architectural approval demonstrating why runtime validation cannot be performed at module initialization.