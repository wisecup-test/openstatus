---
version: 2
name: d0d59f6a-eb46-4347-9c66-0c2e0a55bee0-adoption-of-openstatus-api-src-trpc-errors-for-centralized-error-handling-procedure-route-handlers-runtime-monitoring-configurations
description: "Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Procedure Route Handlers Runtime Monitoring Configurations"
metadata:
  actual:
    adr_id: d0d59f6a-eb46-4347-9c66-0c2e0a55bee0
    status: accepted
    candidate_group_id: 20a80ba6-7010-4233-887b-c7e52e94f2c0
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/app/api/trpc/lambda/**
---

# Adoption of @openstatus/api/src/trpc-errors for Centralized Error Handling: Procedure Route Handlers Runtime Monitoring Configurations

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `sentry.edge.config.ts`
- `sentry.server.config.ts`
- `src/app/api/trpc/lambda/**`

## Context

- Applications serving API requests across heterogeneous execution environments require consistent exception classification and status reporting.
- Telemetry instrumentation and route adapters independently need access to structured error codes and categorization without duplicating parsing logic.
- Centralizing error schemas within a dedicated internal package subpath establishes a single source of truth for runtime exception inspection.

## Problem Statement

When API route handlers and monitoring instrumentation across server and edge runtimes independently classify exceptions, error codes and telemetry tags diverge, causing inconsistent client responses and fragmented monitoring visibility.

## Decision

1. MUST: All procedure route handlers and runtime monitoring configurations MUST import protocol error definitions and classification utilities directly from @openstatus/api/src/trpc-errors.

## Policy Block

- MUST All procedure route handlers and runtime monitoring configurations MUST import protocol error definitions and classification utilities directly from @openstatus/api/src/trpc-errors.

In scope:
- All API route adapters handling procedure requests and protocol responses.
- All application monitoring and exception-reporting configuration layers across server and edge runtimes.

Out of scope:
- Client-side browser presentation components consuming transport responses.
- Standalone background worker routines that do not process protocol procedure invocations.

## Rationale

- Centralized error definitions in @openstatus/api/src/trpc-errors guarantee consistent classification across API handlers and runtime telemetry agents.
- Sharing error parsing logic between server and edge runtimes prevents divergence in status code resolution and exception monitoring.
- Standardizing on an internal module contract isolates protocol error definition changes to a single maintained boundary.

## Consequences

Positive:
- Guarantees uniform error classification, status resolution, and telemetry logging across server and edge execution runtimes.
- Eliminates duplicated error definitions and divergent exception handling between API route handlers and monitoring instrumentation.
- Simplifies error instrumentation maintenance by isolating protocol error structures in a single shared module.

Negative:
- Creates an internal module coupling where updates to error classifications require synchronized releases across consuming applications.
- Risk of runtime portability friction if internal subpath exports inadvertently pull in runtime-incompatible dependencies across edge and server environments.

## Alternatives

- Decentralized error parsing and local status mapping per runtime handler (rejected)
  Rejected because: Duplicates error filtering logic and creates diverging error signatures across runtime boundaries.
  When valid: Isolated microservices with entirely disjoint protocol schemas and no shared dependency tree.
- Generic runtime error interception without protocol-specific classification (rejected)
  Rejected because: Obscures specific error codes and metadata needed for actionable telemetry and client response mapping.
  When valid: Minimal proxy layers that forward raw transport payloads without semantic inspection.

## Risks

- Introducing runtime-specific dependencies inside the shared error module could break edge execution environments.
  Mitigation: Maintain strict runtime independence within the error module to avoid introducing node-specific or edge-incompatible APIs.
  Owner: Engineering Team
- Modifications to shared error schemas may unintentionally alter error codes expected by API clients and monitoring alerts.
  Mitigation: Enforce semantic versioning and regression testing across consuming route handlers whenever error schemas change.
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
- Ensure the shared error module export remains runtime-agnostic so it evaluates cleanly in both server and edge execution contexts.
- Map all protocol error structures to telemetry tags using the shared classification functions provided by the module.

## Continuation Context


Verify commands:
- Discover the project verification scripts from the project manifest and execute the static analysis suite to verify error module import compliance.
- Execute the project test suite using the project build tool to verify error classification behavior across all runtime targets.

Accept when:
- All application endpoints and runtime monitoring configurations import error classification structures exclusively from the shared error module.
- Static analysis and repository verification checks pass with zero unresolved error type references across server and edge environments.

## Enforcement

- Verified by: Automated static analysis rules verifying import paths for error classification utilities.
- Verified by: Peer code reviews during integration of new route handlers or runtime instrumentation.
- Violation handling: Pull requests with decentralized or duplicated error classification definitions are blocked during continuous integration.
- Violation handling: Violations must be refactored to consume the centralized error handling module prior to merge.
- Exception process: Submit an architectural RFC outlining runtime constraints that prevent importing the shared error module.
- Exception process: Obtain documented approval from module maintainers before implementing localized error handlers.