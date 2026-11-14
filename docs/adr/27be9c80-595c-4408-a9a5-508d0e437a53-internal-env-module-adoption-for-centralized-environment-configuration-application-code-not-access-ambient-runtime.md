---
version: 2
name: 27be9c80-595c-4408-a9a5-508d0e437a53-internal-env-module-adoption-for-centralized-environment-configuration-application-code-not-access-ambient-runtime
description: "Internal env Module Adoption for Centralized Environment Configuration: Application Code Not Access Ambient Runtime"
metadata:
  actual:
    adr_id: 27be9c80-595c-4408-a9a5-508d0e437a53
    status: accepted
    candidate_group_id: 7c1d83aa-a233-4b90-af47-a3f448078481
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
      - src/utils/**
---

# Internal env Module Adoption for Centralized Environment Configuration: Application Code Not Access Ambient Runtime

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/cron/**`
- `src/incident/**`
- `src/lib/**`
- `src/scripts/**`
- `src/utils/**`

## Context

- Workflow services, background cron jobs, and database adapters require access to runtime credentials, database endpoints, authorization tokens, and external service keys.
- Decentralized or direct reads of ambient environment globals across disparate service files risk configuration drift, missing runtime variables, and duplicated fallback logic.
- The application establishes a centralized internal env module accessed via relative module import paths to provide structured access through the env() function call.
- Static analysis of intermediate representations demonstrates that all eight observed workflow and background task modules uniformly import this internal module for configuration retrieval.

## Problem Statement

Accessing environment variables directly from global runtime objects across disparate background jobs, route handlers, and infrastructure clients leads to unvalidated configurations, runtime initialization failures, and maintenance overhead. The codebase requires an authoritative, centralized mechanism for accessing runtime configuration settings and credentials consistently across all workflow components.

## Decision

1. MUST_NOT: Application code MUST NOT access ambient runtime environment variables or process globals directly outside the centralized env module.

## Policy Block

- MUST_NOT Application code MUST NOT access ambient runtime environment variables or process globals directly outside the centralized env module.

In scope:
- All background workflow handlers, cron routines, HTTP route handlers, and infrastructure client adapters requiring runtime configuration.
- Internal service scripts and database synchronization utilities that require access to environment variables.

Out of scope:
- Static utility functions and pure algorithmic modules that operate without runtime environment configuration.
- Isolated domain data schemas and constants that do not depend on external secrets or connection strings.

Exceptions:
- EX-20-001: The internal environment module itself initializes and validates runtime configurations from platform host environments.

## Rationale

- Observed evidence shows consistent usage of the internal env module and the env() accessor across all eight workflow service files, proving deliberate consolidation of configuration access.
- Centralizing environment reads behind a single accessor prevents scattered ambient variable reads, enabling unified validation and predictable access patterns across workflow handlers.
- Routing configuration lookups through env() ensures consistent secret retrieval for authentication middleware, database connection setup, and external client requests.

## Consequences

Positive:
- Uniform access pattern for all runtime configuration values across background jobs and route handlers.
- Single locus of control for configuration discovery, schema validation, and runtime credential management.
- Prevention of scattered direct reads from runtime-specific global environment structures.

Negative:
- Creates an internal dependency on the configuration module across all service and worker components.
- Runtime errors during environment initialization can prevent dependent service handlers from starting.

## Alternatives

- Direct access to platform-specific global environment objects within each service file (rejected)
  Rejected because: Scattershot reads across service files lead to duplicated fallback values, missing validation, and tight coupling to specific host runtime globals.
  When valid: Acceptable only within standalone single-file prototype scripts that do not run within the application service architecture.
- Passing configuration objects explicitly through dependency injection down every call hierarchy (rejected)
  Rejected because: Significantly increases boilerplate across lightweight background cron tasks and route middleware where a direct typed accessor function provides cleaner ergonomics.
  When valid: Valid in highly modular multi-tenant libraries requiring runtime tenant isolation or multi-environment testing containers.

## Risks

- Missing or improperly formatted environment variables could trigger runtime initialization crashes across all dependent workflows.
  Mitigation: Ensure the internal configuration module implements robust startup validation and descriptive error reporting for required variables.
  Owner: Engineering team
- Relative import path drift across deep directory hierarchies may lead to inconsistent import patterns.
  Mitigation: Enforce consistent import path conventions and static module resolution checks during automated verification.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Import the internal configuration module using consistent module specifiers and invoke the env() accessor function at point of configuration consumption.
- Ensure newly introduced configuration keys are added to the internal configuration definition before referencing them in workflow handlers or middleware.

## Continuation Context


Verify commands:
- Discover the project task runner or verification scripts from the project configuration and execute static analysis checks against workflow modules.
- Discover the project test runner and execute the test suite to verify configuration retrieval and service initialization behavior.

Accept when:
- Static analysis confirms all environment access within workflow and background modules routes exclusively through the internal env module.
- Automated test executions pass with validated environment configurations across all service routes and cron tasks.

## Enforcement

- Verified by: Automated static analysis checks and lint rules during continuous integration to detect direct access to global environment objects.
- Verified by: Peer code review for all new workflow endpoints, cron jobs, and infrastructure adapters.
- Violation handling: Pull requests containing direct reads of runtime environment globals will be blocked until refactored to use the internal env module.
- Violation handling: Static analysis failures in automated pipelines will prevent merging non-compliant code.
- Exception process: Exceptions must be requested via an architectural review issue outlining why the internal configuration module cannot satisfy the specific runtime requirement.
- Exception process: Approved exceptions must be documented in the exception register with an explicit decommission or migration timeline.