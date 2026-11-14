---
version: 2
name: 07b38a44-24bc-4577-bc11-6a7788c4c682-openstatus-api-src-trpc-errors-internal-module-adoption-for-centralized-procedure-call-error-handling-monitoring-telemetry-29cf5b8c9dcf90ce
description: "@openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Monitoring Telemetry Configurations Across Server Edge"
metadata:
  actual:
    adr_id: 07b38a44-24bc-4577-bc11-6a7788c4c682
    status: accepted
    candidate_group_id: 0c61fac3-cc9d-4ec8-a1e7-b15a3bb62c76
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# @openstatus/api/src/trpc-errors Internal Module Adoption for Centralized Procedure Call Error Handling: Monitoring Telemetry Configurations Across Server Edge

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/app/api/trpc/lambda/**`

## Context

- Application services handle remote procedure call endpoints across heterogeneous environments including server and edge runtimes.
- Observability configurations and telemetry hooks require deterministic error classification to filter or capture operational exceptions without code duplication.
- The internal workspace module @openstatus/api/src/trpc-errors exports centralized error definitions shared between request handling logic and runtime telemetry filters.

## Problem Statement

Dispersed error construction across server routes and observability configuration modules leads to inconsistent error classification, uncoordinated telemetry filtering, and divergence in error semantics across execution runtimes.

## Decision

1. SHOULD: Monitoring and telemetry configurations across server and edge runtimes SHOULD use the exported error types from @openstatus/api/src/trpc-errors to filter and categorize operational exceptions.

## Policy Block

- SHOULD Monitoring and telemetry configurations across server and edge runtimes SHOULD use the exported error types from @openstatus/api/src/trpc-errors to filter and categorize operational exceptions.

In scope:
- All API routes and endpoint adapters handling remote procedure call executions.
- All runtime observability and monitoring configuration modules across server and edge environments.

Out of scope:
- Client-side presentation components that consume formatted error messages without directly classifying protocol errors.
- External third-party integration webhooks operating outside the internal procedure call protocol.

## Rationale

- Centralizing error definitions in @openstatus/api/src/trpc-errors ensures that both request execution adapters and runtime monitoring configurations share an identical error taxonomy.
- Evidence demonstrates concurrent imports of @openstatus/api/src/trpc-errors across route handlers and multi-runtime monitoring setups, proving active architectural reliance on unified error handling exports.

## Consequences

Positive:
- Guarantees consistent error taxonomy between API transport layers and observability instrumentation.
- Enables uniform error classification and filtering across diverse runtime environments including server and edge configurations.
- Prevents divergence of error codes and payloads across consumer applications in the workspace.

Negative:
- Changes to shared error definitions in @openstatus/api/src/trpc-errors require synchronized updates and verification across server and edge build targets.
- Creates an internal coupling between monitoring configurations and internal API library export paths.

## Alternatives

- Decentralized error creation within individual API route handlers and monitoring hooks (rejected)
  Rejected because: Causes error code drift between endpoint response payloads and telemetry monitoring configurations, preventing unified error filtering across runtimes.
  When valid: Only valid in isolated prototypes with no shared telemetry or multi-runtime monitoring requirements.
- Direct dependency on generic protocol error classes without workspace domain abstraction (rejected)
  Rejected because: Couples observability filters tightly to external protocol defaults rather than domain-specific workspace error taxonomy.
  When valid: Valid when an application has no internal shared libraries or workspace package architecture.

## Risks

- Breaking changes to shared error definitions could disrupt both API responses and runtime error monitoring simultaneously.
  Mitigation: Establish automated interface contract tests for @openstatus/api/src/trpc-errors that run against all runtime targets prior to release.
  Owner: engineering team
- Inadvertent introduction of server-only primitives into shared error definitions could cause compilation failures in edge environments.
  Mitigation: Ensure @openstatus/api/src/trpc-errors avoids environment-specific dependencies to maintain runtime portability.
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
- Verify that @openstatus/api/src/trpc-errors exports remain compatible across all target execution environments including edge and standard server runtimes.
- Import error definitions directly from the designated module path without creating local aliases or wrapper structures that obscure origin.

## Continuation Context


Verify commands:
- discover_and_execute_typecheck_validation
- discover_and_execute_module_dependency_lint
- discover_and_execute_runtime_test_suite

Accept when:
- All modules importing remote procedure call error classifications resolve definitions exclusively from @openstatus/api/src/trpc-errors.
- Static analysis and type validation confirm zero direct instantiations of untyped protocol errors in application handlers and runtime monitoring configurations.
- Build and runtime initialization across server and edge environments pass without module resolution errors.

## Enforcement

- Verified by: Automated dependency analysis during continuous integration verifying import specifiers for error definitions.
- Verified by: Static type checking verifying error type conformity in route handlers and observability setup modules.
- Verified by: Peer code review on all pull requests modifying API endpoints or monitoring configurations.
- Violation handling: Pull requests with unapproved or decentralized error definitions fail continuous integration checks.
- Violation handling: Engineers must refactor custom error instances to consume @openstatus/api/src/trpc-errors exports.
- Exception process: Submit an architectural exception request detailing why the centralized error module cannot accommodate the required classification.
- Exception process: Obtain approval from architecture and observability leads prior to introducing standalone error handling structures.