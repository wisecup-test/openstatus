---
version: 2
name: caa36032-42f2-48f1-8e3e-7d1f4c867c08-server-side-analytics-ingestion-with-openpanel-sdk-consumers-inspect-repository-dependency-lock-artifact
description: "Server-Side Analytics Ingestion with @openpanel/sdk: Consumers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: caa36032-42f2-48f1-8e3e-7d1f4c867c08
    status: accepted
    candidate_group_id: 3b2a0c78-9a6d-46cc-9296-76a3421d249e
    evidence:
      source: deep
  scope:
    root: packages/analytics
    paths:
      - src/**
---

# Server-Side Analytics Ingestion with @openpanel/sdk: Consumers Inspect Repository Dependency Lock Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/analytics`

Applies to:
- `src/**`

## Context

- Backend services require unified event telemetry and product analytics tracking across server environments.
- The codebase establishes a centralized analytics module that integrates @openpanel/sdk with environment resolution and structured event schemas.
- Direct runtime variable access must be governed through centralized configuration to prevent configuration drift between development and production deployments.

## Problem Statement

Scattering direct telemetry calls or ad-hoc analytics client initializations across backend services leads to inconsistent event schemas, unvalidated environment configuration access, and tight coupling to external telemetry vendors. A centralized server-side telemetry integration is necessary to standardize event dispatching, encapsulate runtime configuration, and safeguard service execution from telemetry transport failures.

## Decision

1. MUST: Consumers MUST inspect the repository dependency lock artifact to determine and verify the exact resolved version of @openpanel/sdk prior to implementation.

## Policy Block

- MUST Consumers MUST inspect the repository dependency lock artifact to determine and verify the exact resolved version of @openpanel/sdk prior to implementation.

In scope:
- Server-side services and backend execution boundaries dispatching telemetry events
- Centralized telemetry instrumentation modules configuring @openpanel/sdk

Out of scope:
- Client-side user interface tracking and browser runtime analytics
- Low-level system infrastructure logging and container metrics collection

## Rationale

- Adopting @openpanel/sdk within a dedicated module isolates external vendor APIs from application business logic.
- Coupling SDK initialization with centralized environment parsing ensures runtime environment markers are validated before telemetry transmission.
- Enforcing a shared event schema module prevents telemetry schema fragmentation across different server execution pathways.

## Consequences

Positive:
- Centralizes telemetry dispatch logic within a single maintainable server module.
- Guarantees type-safe event payloads through unified event catalog definitions.
- Isolates vendor-specific SDK initialization and environment variable handling from consuming services.

Negative:
- Introduces an external runtime dependency on third-party telemetry infrastructure.
- Requires ongoing maintenance to synchronize custom event definitions with the centralized catalog.
- Potential latency overhead on server execution paths if event dispatching is not handled asynchronously.

## Alternatives

- Direct unencapsulated @openpanel/sdk usage in individual server handlers (rejected)
  Rejected because: Causes duplicate client configurations, leaking runtime environment variables and fragmenting event schemas.
  When valid: Valid only in single-file prototypes with no ongoing maintenance.
- Custom in-house telemetry ingestion pipeline (rejected)
  Rejected because: Significantly increases operational complexity and maintenance burden compared to adopting a dedicated SDK.
  When valid: Valid when data sovereignty regulations prohibit third-party analytics services.

## Risks

- Telemetry service latency or network downtime affecting backend request processing.
  Mitigation: Execute telemetry dispatch asynchronously and catch transport exceptions to avoid blocking request lifecycles.
  Owner: Backend Engineering Team
- Unsynchronized dependency versions leading to breaking API changes during runtime client execution.
  Mitigation: Enforce strict dependency lockfile verification and validate public SDK reference documentation before upgrades.
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
- Instantiate the telemetry client as a singleton or scoped export within the dedicated analytics module to share the connection pool across server invocations.
- Reference the centralized event definitions module when adding new event types to preserve payload consistency across all callers.

## Continuation Context


Verify commands:
- Discover and run the repository test runner to execute server analytics unit and integration tests.
- Discover and execute the repository type-check script to verify compliance with analytics event schemas.
- Discover and execute the repository dependency validation task to ensure lockfile synchronization.

Accept when:
- All server analytics tests pass with successful client instantiation and event dispatch simulation.
- Static type verification succeeds without errors across analytics event payloads.
- Dependency lockfile validation completes without uncommitted version discrepancies.

## Enforcement

- Verified by: Automated continuous integration checks executing test suites and static type validation.
- Verified by: Peer code review auditing server-side telemetry invocations and event definitions.
- Violation handling: Pull requests containing direct unabstracted SDK imports or unvalidated event payloads will be blocked from merging.
- Violation handling: Static analysis warnings must be resolved prior to deployment.
- Exception process: Exceptions for alternative telemetry sinks require review and documented approval by the lead backend architect.