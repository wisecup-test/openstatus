---
version: 2
name: b6f4cba3-1895-4b0b-ae19-6a3e16f92e17-adoption-of-openstatus-analytics-for-server-telemetry-and-event-tracking-failures-encountered-during-analytics-event-emission
description: "Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission"
metadata:
  actual:
    adr_id: b6f4cba3-1895-4b0b-ae19-6a3e16f92e17
    status: accepted
    candidate_group_id: cf93de28-0c1e-48d4-8854-47c3358b8b7e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
      - src/routes/**
---

# Adoption of @openstatus/analytics for Server Telemetry and Event Tracking: Failures Encountered During Analytics Event Emission

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/libs/middlewares/**`
- `src/routes/**`

## Context

- Inbound API routes, remote procedure calls, and HTTP middlewares across the server application require consistent operational tracking and domain event reporting.
- Static analysis identifies @openstatus/analytics imported across route handlers and middleware modules to handle event emission.
- Telemetry events correlate with tenant workspace identity extracted from the active request context, requiring a standardized interface across heterogeneous transport protocols.

## Problem Statement

Disparate server endpoints handling resource mutation and request processing risk fragmented event modeling, inconsistent tenant attribution, and duplicated client transport logic if telemetry dispatch is implemented ad hoc across individual transport layers.

## Decision

1. SHOULD: Failures encountered during analytics event emission SHOULD be caught and logged as non-fatal warnings to prevent telemetry disruptions from failing primary request transactions.

## Policy Block

- SHOULD Failures encountered during analytics event emission SHOULD be caught and logged as non-fatal warnings to prevent telemetry disruptions from failing primary request transactions.

In scope:
- All server-side REST route handlers, RPC interceptors, and HTTP middlewares that process incoming requests or perform domain mutations.

Out of scope:
- Client-side browser applications, static asset delivery, and offline utility scripts that operate outside the server request lifecycle.

## Rationale

- Adopting @openstatus/analytics establishes a single source of truth for telemetry event schemas, ensuring consistent event payloads across diverse endpoints.
- Centralizing telemetry in a shared workspace package abstracts transport and ingestion details away from domain route logic.
- Evidence from eighteen server modules demonstrates an established, cohesive architectural pattern of pairing request workspace context with standardized analytics dispatch.

## Consequences

Positive:
- Unified schema governance and typed telemetry contracts across REST routes and RPC interceptors.
- Consistent correlation of tenant workspace identifiers across all emitted operational and domain events.
- Isolation of external event transport and client configuration within a dedicated shared module.

Negative:
- Server request handlers inherit a direct dependency on @openstatus/analytics for lifecycle observability.
- Non-fatal event dispatch failures require localized error handling and structured warning logging across dispatch points.

## Alternatives

- Direct inline third-party analytics SDK invocation within individual route files (rejected)
  Rejected because: Duplicating third-party analytics initialization and event schemas across individual route files creates contract drift and complicates transport migration.
  When valid: Small single-purpose prototypes or monolithic applications with only one or two endpoints.
- Transport-level analytics dispatch exclusively via external proxy or web server access logs (rejected)
  Rejected because: Access logs lack internal domain context such as resource mutations, granular validation details, and fine-grained workspace entities.
  When valid: Pure network-level traffic monitoring where application domain payload inspection is unnecessary.

## Risks

- Synchronous telemetry dispatch failures could degrade API response times or abort primary database transactions if not properly isolated.
  Mitigation: Wrap telemetry dispatches in non-blocking execution or resilient error handling with warning logs, ensuring transaction completion is unaffected.
  Owner: Backend Engineering Team
- Event payload schema breaking changes within @openstatus/analytics could cause compilation failures across server route files.
  Mitigation: Enforce workspace-level type checking and contract compatibility testing in CI pipelines prior to merging dependency updates.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Extract the active workspace identifier from the request context before invoking analytics dispatch routines.
- Catch analytics delivery errors and log them through the structured logging facility at warning level with event and workspace context attributes.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and type check scripts from the repository manifests and execute them across the server application.
- Discover and run the test suite scripts for server routes and interceptors to verify analytics event dispatch handling and error isolation.

Accept when:
- All server routes, interceptors, and middlewares compile without type errors against the @openstatus/analytics interface.
- Analytics event dispatches correctly associate workspace identity from request context and handle transmission failures gracefully without unhandled exceptions.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace type validation and test suites.
- Verified by: Peer code reviews auditing newly created or updated route handlers to ensure telemetry uses @openstatus/analytics.
- Violation handling: Pull requests introducing direct third-party analytics SDK imports or bypassing @openstatus/analytics will be blocked until refactored.
- Exception process: Exceptions must be submitted via an architecture review request detailing why the shared analytics module cannot support the required event profile.