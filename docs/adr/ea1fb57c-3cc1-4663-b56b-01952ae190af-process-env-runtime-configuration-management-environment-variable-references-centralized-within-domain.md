---
version: 2
name: ea1fb57c-3cc1-4663-b56b-01952ae190af-process-env-runtime-configuration-management-environment-variable-references-centralized-within-domain
description: "process.env Runtime Configuration Management: Environment Variable References Centralized Within Domain"
metadata:
  actual:
    adr_id: ea1fb57c-3cc1-4663-b56b-01952ae190af
    status: accepted
    candidate_group_id: aa264d68-51a8-4cce-a40f-bb3bf9575f06
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# process.env Runtime Configuration Management: Environment Variable References Centralized Within Domain

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `instrumentation-client.ts`
- `next.config.ts`
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/**`

## Context

- The application operates across heterogeneous execution contexts including edge runtimes, server-side processing, and client-side browser interfaces.
- Operational parameters, telemetry configurations, and external integration keys must be dynamically configurable between environments without modifying source code.
- Direct access to the process environment variable interface has been established across service routing, instrumentation, and presentation layers.
- Consistent environment access conventions are necessary to prevent secret exposure while enabling deployment-specific behavior.

## Problem Statement

Heterogeneous execution contexts require a unified and reliable mechanism to configure runtime behaviors, external service integrations, and telemetry without hardcoding environment-specific values into application logic. In the absence of structured configuration constraints, direct and unvalidated access to process environment variables risks credential leakage across runtime boundaries, runtime failures due to missing required variables, and inconsistent environment resolution across server, edge, and client tiers.

## Decision

1. SHOULD: Environment variable references SHOULD be centralized within domain-specific configuration modules rather than repeated as raw string lookups across presentation components.

## Policy Block

- SHOULD Environment variable references SHOULD be centralized within domain-specific configuration modules rather than repeated as raw string lookups across presentation components.

In scope:
- Configuration of application runtime parameters across client, edge, and server execution contexts.
- Access to external service credentials, endpoints, and deployment environment indicators.

Out of scope:
- Static application data and content assets that do not vary by deployment environment.
- User-provided runtime inputs and request parameters passed during interactive sessions.

Exceptions:
- EXC-37-001: Build-time static asset compilation requires fixed configuration embedded without runtime environment access

## Rationale

- Direct process environment variable access provides standard, zero-overhead configuration sourcing supported universally across runtime execution tiers.
- Evidence across configuration scripts, middleware handlers, telemetry integrations, and component modules substantiates an established convention of inspecting process environment variables for operational flags.
- Standardizing environment configuration access ensures secret isolation between server and client bundles while supporting dynamic deployments across multiple operational environments.

## Consequences

Positive:
- Uniform configuration access pattern across edge, server, and client build pipelines.
- Elimination of hardcoded operational endpoints and credentials from source code.
- Enables dynamic configuration switching across different deployment environments without rebuilding application binaries.

Negative:
- Scattered environment variable references increase coupling to process-level configuration across component layers.
- Lack of a centralized validation schema increases the risk of runtime errors if required environment variables are undefined at launch.
- Manual prefix tracking is required to prevent accidental exposure of sensitive server keys to client compilation outputs.

## Alternatives

- Centralized strict schema validation module parsing all environment variables at application initialization (rejected)
  Rejected because: Introduces startup validation overhead and cross-context dependency complexity across disparate edge and client compilation targets
  When valid: When all execution contexts share a single unified runtime and require strict upfront schema enforcement for all declared variables
- Externalized configuration management service queried over network at runtime (rejected)
  Rejected because: Adds runtime network latency, potential point-of-failure dependency at boot, and operational infrastructure complexity
  When valid: When dynamic real-time configuration reload across large-scale distributed microservices is strictly mandated

## Risks

- Accidental exposure of sensitive credentials to client-side bundles through improperly isolated environment references
  Mitigation: Enforce strict separation between server-only and public client environment identifiers during automated inspection and review
  Owner: Security and Platform Engineering Team
- Runtime failure caused by undefined or missing required environment variables in newly deployed environments
  Mitigation: Implement defensive existence checks and fallback defaults where safe, backed by continuous deployment configuration verification
  Owner: Application Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Audit all environment variable references to ensure private credentials remain isolated to server and edge execution boundaries.
- Group related environment variable accesses into domain-focused helper utilities to prevent duplicated property lookup logic across presentation modules.

## Continuation Context


Verify commands:
- Discover the repository configuration manifest and execute the designated static analysis or linting script to identify unvalidated environment variable usages.
- Discover and execute the project build verification script to ensure client bundle compilation does not include unisolated server environment identifiers.
- Execute a search across source files for direct process environment invocations to verify compliance with scoping and security rules.

Accept when:
- All process environment variable lookups comply with boundary isolation rules between server and client contexts.
- The project static analysis and build verification scripts complete with zero errors related to environment configuration.
- All referenced environment variables have documented fallback values or are verified present in deployment environment definitions.

## Enforcement

- Verified by: Automated static analysis and bundle verification checks run during continuous integration.
- Verified by: Peer code review verifying appropriate categorization and isolation of environment variable accesses.
- Violation handling: Pull requests introducing unisolated credential lookups or undeclared environment dependencies are blocked from merging until corrected.
- Violation handling: Non-compliant direct lookups identified in code review must be refactored to comply with boundary rules.
- Exception process: Exceptions for static substitution or specialized runtime overrides require documented architectural review and approval from platform engineering.