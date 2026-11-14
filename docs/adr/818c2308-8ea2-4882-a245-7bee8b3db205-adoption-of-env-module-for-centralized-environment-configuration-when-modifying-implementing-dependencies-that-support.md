---
version: 2
name: 818c2308-8ea2-4882-a245-7bee8b3db205-adoption-of-env-module-for-centralized-environment-configuration-when-modifying-implementing-dependencies-that-support
description: "Adoption of @/env Module for Centralized Environment Configuration: When Modifying Implementing Dependencies That Support"
metadata:
  actual:
    adr_id: 818c2308-8ea2-4882-a245-7bee8b3db205
    status: accepted
    candidate_group_id: 769d826a-736d-4c06-974b-1987d15d8aa7
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/**
      - src/routes/**
      - src/utils/**
---

# Adoption of @/env Module for Centralized Environment Configuration: When Modifying Implementing Dependencies That Support

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/libs/**`
- `src/routes/**`
- `src/utils/**`

## Context

- Application services, routes, RPC interceptors, and client initializers require access to runtime credentials and configuration flags.
- Decentralized access to platform environment globals risks silent runtime failures and inconsistent configuration parsing across disparate modules.
- Static analysis indicates uniform importation of the internal path-aliased environment module across server routes, external service clients, RPC interceptors, and logging utilities.

## Problem Statement

Unmanaged access to platform environment variables across multiple application domains leads to untyped configuration reads, unvalidated credential parsing, and runtime failures that remain undetected until invocation.

## Decision

1. MUST: When modifying or implementing dependencies that support the environment validation schema, the consumer MUST inspect the repository lock or resolution artifact to determine the exact locked version before implementation.

## Policy Block

- MUST When modifying or implementing dependencies that support the environment validation schema, the consumer MUST inspect the repository lock or resolution artifact to determine the exact locked version before implementation.

In scope:
- All server route handlers, RPC interceptors, database clients, background workers, and utility modules requiring runtime configuration or secrets.

Out of scope:
- Standalone deployment provisioning scripts and local development container configurations managed entirely outside the application runtime.

## Rationale

- Funneling environment access through the dedicated module provides compile-time type safety and guarantees runtime validation prior to handler execution.
- Evidence across route handlers, RPC interceptors, and client initializers confirms a deliberate convention to access configuration through the shared environment abstraction.
- Centralization prevents fragmentation of configuration parsing logic across micro-boundaries and utility modules.

## Consequences

Positive:
- Ensures strong type safety and centralized validation for all runtime environment variables and secrets across server modules.
- Prevents unexpected runtime failures caused by missing or misconfigured external service credentials.
- Provides a single source of truth for environment variable definitions and configuration access.

Negative:
- Creates a centralized dependency that must be maintained whenever new environment variables are introduced across any subsystem.
- Initialization failure in the environment module halts startup across all consuming services and routes simultaneously.

## Alternatives

- Direct access to platform-level process environment globals across individual route and utility files (rejected)
  Rejected because: Bypasses centralized schema validation, produces runtime failures when variables are absent, and scatters environment key dependencies across the codebase
  When valid: Only in minimal single-file scripts that lack modular infrastructure
- Per-module configuration files that independently parse platform environment variables (rejected)
  Rejected because: Duplicates validation logic, creates inconsistent parsing across boundaries, and increases maintenance overhead
  When valid: When boundaries represent fully decoupled independent services deployed in separate repositories

## Risks

- Invalid or missing configuration values cause startup crashes across all application services.
  Mitigation: Validate all environment properties during startup and surface clear diagnostics identifying missing variables.
  Owner: engineering team
- Developers may bypass the centralized module and introduce direct references to platform environment globals in newly added files.
  Mitigation: Enforce strict linting rules and static analysis in continuous integration to catch direct global reads.
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
- Define all configuration properties with strict schema validators to reject undefined or malformed environment values at process initialization.
- Consume the validated environment object across application routes, utilities, and client factories using the established module alias.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script to confirm all environment accesses adhere to the centralized schema.
- Discover and execute the repository linting script to ensure no direct references to platform environment globals exist in application source files.
- Discover and execute the test runner to verify configuration validation logic and dependent component execution.

Accept when:
- All application modules retrieve environment configuration exclusively through the internal environment module without referencing global process environments directly.
- Static code analysis and test suites verify that every referenced environment key is declared in the central configuration schema and resolves successfully.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline flagging direct references to platform environment globals outside the designated configuration module.
- Verified by: Peer code review verifying that all new configuration values are defined within the centralized environment module schema.
- Violation handling: Pull requests containing direct reads of platform environment globals fail automated checks and are blocked from merging.
- Violation handling: Flagged code must be refactored to consume the typed configuration instance from the centralized environment module.
- Exception process: Submit an architecture review request detailing the technical constraint that prevents importing the centralized environment module.
- Exception process: Obtain explicit approval from the backend architecture lead prior to merging any code that reads unmanaged environment variables.