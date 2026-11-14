---
version: 2
name: 1da6ce56-6462-415c-9909-2deab263bb5e-adoption-of-openstatus-api-src-trpc-errors-for-centralized-error-management-application-modules-not-declare-local-error
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Application Modules Not Declare Local Error"
metadata:
  actual:
    adr_id: 1da6ce56-6462-415c-9909-2deab263bb5e
    status: accepted
    candidate_group_id: 78a52675-2a86-4000-bdbd-889f7e086ee9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Management: Application Modules Not Declare Local Error

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/app/api/trpc/lambda/**`

## Context

- API route handling and observability instrumentation across server and edge runtimes require consistent error identification and filtering.
- Multiple execution contexts require shared error categorization without duplicating mapping logic across operational entry points.
- Centralizing error definitions within an internal package decouples runtime configurations from ad-hoc error handling implementations.

## Problem Statement

Inconsistent error classification across API request adapters and disparate monitoring runtimes causes divergent telemetry and fragmented failure handling.

## Decision

1. MUST_NOT: Application modules MUST NOT declare local error classifications or duplicate error categorization predicates defined in @openstatus/api/src/trpc-errors.

## Policy Block

- MUST_NOT Application modules MUST NOT declare local error classifications or duplicate error categorization predicates defined in @openstatus/api/src/trpc-errors.

In scope:
- API route handlers and endpoint transport adapters.
- Server and edge runtime observability and monitoring configuration modules.

Out of scope:
- Client-side presentation components without API error classification responsibilities.
- Standalone utility packages that do not participate in API request processing.

## Rationale

- Direct evidence demonstrates that API route handlers and monitoring configuration files consistently import @openstatus/api/src/trpc-errors.
- Consolidating error structures into a shared module prevents error mapping divergence across distinct runtime environments.
- Single-source error definitions ensure that observability filtering rules accurately reflect API failure states.

## Consequences

Positive:
- Guarantees uniform error classification across API routing and multi-runtime observability layers.
- Eliminates divergent error filtering and reporting logic across server and edge configurations.
- Centralizes maintenance of domain and protocol error mappings within a single internal package.

Negative:
- Creates a direct dependency coupling between application runtime entry points and the shared error module.
- Breaking changes to shared error definitions require synchronized validation across server and edge execution environments.

## Alternatives

- Local error definitions within each runtime configuration and route handler (rejected)
  Rejected because: Duplicating error parsing logic across server and edge environments leads to inconsistent error filtering and divergent telemetry.
  When valid: Valid only in isolated standalone modules that do not consume or expose the shared API schema.
- Generic protocol status mapping without shared error definitions (rejected)
  Rejected because: Discards procedure-level diagnostic details required for observability platforms and structured client error handling.
  When valid: Valid for unauthenticated static assets or simple health check endpoints.

## Risks

- Modifications to exported error signatures could alter error filtering logic across observability runtimes.
  Mitigation: Enforce strict contract verification and automated static analysis checks during integration pipelines.
  Owner: engineering team
- Introduction of server-only primitives into the shared error module could trigger failures in edge execution environments.
  Mitigation: Maintain strict separation of isomorphic error logic from platform-specific APIs within the shared package.
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
- Ensure that exported error structures remain isomorphic and avoid importing runtime-specific dependencies incompatible with edge environments.
- Coordinate error filtering configurations in monitoring layers with the status classifications exported by the shared module.

## Continuation Context


Verify commands:
- Discover the workspace dependency validation script and execute it to confirm all error module imports resolve to the shared package.
- Discover and execute the project static analysis and type verification directives to validate error handling conformity.

Accept when:
- Static analysis confirms all error references in route handlers and observability configurations resolve to @openstatus/api/src/trpc-errors.
- No local error classification classes or duplicate error mapping functions exist within runtime configuration boundaries.

## Enforcement

- Verified by: Continuous integration type checking and static analysis suites.
- Verified by: Peer code review of modifications to API endpoints and observability configurations.
- Violation handling: Pull requests introducing redundant error classifications or bypassing the shared module will fail automated checks and be blocked from merging.
- Exception process: Exceptions require architectural review and written documentation detailing why the shared module cannot support the specific runtime constraint.