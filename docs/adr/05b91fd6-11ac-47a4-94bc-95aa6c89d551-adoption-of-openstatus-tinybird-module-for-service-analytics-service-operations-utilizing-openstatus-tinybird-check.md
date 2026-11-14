---
version: 2
name: 05b91fd6-11ac-47a4-94bc-95aa6c89d551-adoption-of-openstatus-tinybird-module-for-service-analytics-service-operations-utilizing-openstatus-tinybird-check
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Service Operations Utilizing Openstatus Tinybird Check"
metadata:
  actual:
    adr_id: 05b91fd6-11ac-47a4-94bc-95aa6c89d551
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Service Operations Utilizing Openstatus Tinybird Check

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/**`

## Context

- The service layer requires high-performance analytical query capabilities alongside transactional relational persistence for monitor telemetry aggregations.
- Direct and decentralized analytics calls across service handlers risk inconsistent secret resolution and fragile connection management.
- Service execution environments require controlled toggles to support degraded or mock operations without modifying business logic.

## Problem Statement

Service components requiring analytical aggregations and telemetry queries must access specialized analytics backends without scattering connection configurations, credential resolution, or operational toggles throughout individual business logic modules.

## Decision

1. SHOULD: Service operations utilizing @openstatus/tinybird SHOULD check the operational no-op state configured via TINYBIRD_NOOP to support degraded or local execution modes.

## Policy Block

- SHOULD Service operations utilizing @openstatus/tinybird SHOULD check the operational no-op state configured via TINYBIRD_NOOP to support degraded or local execution modes.

In scope:
- Service modules executing analytical telemetry queries and aggregations.
- Context modules managing analytics client initialization and credentials.

Out of scope:
- Relational transactional persistence managed exclusively through database schema modules.
- Frontend rendering components that do not execute service-level data operations.

## Rationale

- Adopting @openstatus/tinybird establishes a standardized boundary for analytical aggregations across service routines.
- Centralizing configuration through service context with TINY_BIRD_API_KEY, TINYBIRD_URL, and TINYBIRD_NOOP guarantees unified secret handling and safe execution gating.
- Separating telemetry aggregation from transactional relational operations enables independent scaling and schema evolution.

## Consequences

Positive:
- Standardizes analytical telemetry queries behind a dedicated, typed module interface across service handlers.
- Centralizes credential resolution and operational toggles through service context, eliminating redundant environment variable reads.
- Enables seamless execution bypass during development and testing through the no-op operational flag.

Negative:
- Introduces an external workspace module dependency that requires version maintenance and synchronization across packages.
- Requires coordination between analytics schema contracts and relational models when assembling composite reporting entities.

## Alternatives

- Direct relational queries against primary database schemas for all aggregation metrics (rejected)
  Rejected because: Relational transactional schemas are unsuited for high-volume telemetry aggregations, risking query degradation on transactional workloads.
  When valid: Valid when metrics volume is negligible and transactional tables can compute summaries with minimal latency.
- Inline HTTP requests to external analytics endpoints within individual service handlers (rejected)
  Rejected because: Scatters endpoint targets, authentication credentials, and request payloads across business logic without centralized error handling or no-op control.
  When valid: Valid in standalone one-off scripts where importing a shared module is disproportionate overhead.

## Risks

- Missing or invalid runtime environment configuration causing runtime connection failure.
  Mitigation: Validate required environment variables during context initialization and provide clear diagnostic error reporting.
  Owner: Service Engineering Team
- Schema drift between external analytics endpoints and internal service consumption models.
  Mitigation: Encapsulate query contracts within dedicated service helper functions and enforce type validation on returned payloads.
  Owner: Service Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct analytical client instances exclusively within shared context factories to encapsulate secret ingestion and operational toggles.
- Ensure query handlers validate input criteria prior to invoking analytics retrieval methods.

## Continuation Context


Verify commands:
- Discover the project test runner and execute the automated test suite for service context and monitor modules.
- Discover the workspace static analysis and type-checking scripts to verify correct usage of @openstatus/tinybird imports.

Accept when:
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

## Enforcement

- Verified by: Continuous integration automated test suites and static analysis checks.
- Verified by: Peer code review on pull requests touching service queries or client initialization.
- Violation handling: Automated rejection of pull requests that instantiate unmanaged analytics connections or bypass centralized context configuration.
- Exception process: Submit an architectural review request documenting why centralized context or the @openstatus/tinybird client cannot satisfy service analytical requirements.