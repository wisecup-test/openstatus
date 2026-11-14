---
version: 2
name: 09c07e18-d608-4bb6-8106-e210cb94785a-adopt-t3-oss-env-core-for-schema-validated-runtime-environment-configuration-validate-required-credential-strings-strict-validation
description: "Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Validate Required Credential Strings Strict Validation"
metadata:
  actual:
    adr_id: 09c07e18-d608-4bb6-8106-e210cb94785a
    status: accepted
    candidate_group_id: b3d96d76-7bcc-4aec-b99d-91547519e512
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Adopt @t3-oss/env-core for Schema-Validated Runtime Environment Configuration: Validate Required Credential Strings Strict Validation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/emails`

Applies to:
- `src/**`

## Context

- Runtime dispatch of external service requests requires access to sensitive credentials and runtime execution flags.
- Direct, unvalidated access to runtime environment properties risks undetected configuration errors and runtime dispatch failures.
- Centralizing configuration parsing through schema validation provides type safety and immediate initialization-time validation for external integration parameters.

## Problem Statement

Unvalidated access to runtime environment variables allows missing or malformed credentials to propagate into external service dispatch logic, causing delayed runtime failures and inconsistent configuration handling across package modules.

## Decision

1. MUST: Validate all required credential strings with strict validation schemas before dispatching external service requests.

## Policy Block

- MUST Validate all required credential strings with strict validation schemas before dispatching external service requests.

In scope:
- Runtime modules requiring external service credentials or environment-specific configurations.
- Outbound client dispatchers that communicate with external service APIs.

Out of scope:
- Static assets and styling definitions that do not interact with runtime environment properties.
- Self-contained algorithmic utility functions without external service dependencies.

Exceptions:
- EX-37-001: A bootstrap harness must establish fundamental execution mode flags prior to module loader initialization.

## Rationale

- Schema validation with @t3-oss/env-core enforces early validation during service initialization, preventing unhandled runtime exceptions during external client operations.
- Centralizing environment parsing provides explicit type guarantees for downstream consumers, eliminating repetitive manual verification across client modules.
- Restricting raw runtime environment lookups to a single schema module ensures a clear boundary for credential handling and auditability.

## Consequences

Positive:
- Provides guaranteed type safety and fail-fast validation for all declared environment variables at service initialization.
- Eliminates duplicate parsing and manual type casting of runtime environment properties across client modules.
- Establishes a single auditable boundary for credential extraction and secret handling.

Negative:
- Introduces startup validation overhead and halts execution if non-critical configuration fields are misconfigured.
- Requires defining and maintaining explicit schema definitions for every new environment variable introduced.

## Alternatives

- Direct unvalidated runtime environment property access (rejected)
  Rejected because: Lacks type guarantees and causes runtime failures only when missing keys are accessed during execution.
  When valid: Valid in minimal standalone scripts without external dependencies or security requirements.
- Custom hand-written environment parsing functions (rejected)
  Rejected because: Increases maintenance burden and creates inconsistent validation rules compared to standardized schema libraries.
  When valid: Valid in zero-dependency environments where third-party libraries cannot be added.

## Risks

- Missing optional environment variables causing validation failures during testing or bootstrapping.
  Mitigation: Define appropriate defaults or optional markers within the schema definitions for non-essential configuration keys.
  Owner: Engineering Team
- Divergence between schema definitions and the active deployment environment configured variables.
  Mitigation: Validate deployment environment variable matrices against schema specifications during continuous integration pipelines.
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
- Declare all required runtime secrets and configuration values in the dedicated schema definition module, exporting a validated configuration object for package consumption.
- Reference the validated configuration object in all client wrappers and dispatch logic rather than querying the host environment directly.

## Continuation Context


Verify commands:
- Discover the workspace dependency manifest and execute the designated type-checking script to confirm schema definitions compile without errors.
- Discover the project test runner configuration and execute test suites validating runtime environment variable parsing and failure behavior.
- Discover and run the project linting and static analysis scripts to verify that raw runtime environment reads are restricted to designated configuration schemas.

Accept when:
- The schema validation module parses declared environment variables successfully in valid execution environments.
- Process execution halts with informative validation errors when required runtime configuration variables are missing or malformed.
- Static analysis confirms modules consume typed configuration outputs rather than unvalidated runtime environment sources directly.

## Enforcement

- Verified by: Continuous integration test suites and static analysis checks.
- Verified by: Peer review of pull requests touching environment variable declarations or client configurations.
- Violation handling: Build rejection on continuous integration when unvalidated environment reads are introduced in application logic.
- Violation handling: Review rejection requiring encapsulation of raw environment variable reads within the designated schema module.
- Exception process: Formal review and approval by the architecture group detailing technical justification for raw environment access.
- Exception process: Documented exception recorded in the central architecture registry before deployment.