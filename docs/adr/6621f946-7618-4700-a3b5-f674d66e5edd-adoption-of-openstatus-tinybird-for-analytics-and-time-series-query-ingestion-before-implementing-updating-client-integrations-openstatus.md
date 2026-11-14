---
version: 2
name: 6621f946-7618-4700-a3b5-f674d66e5edd-adoption-of-openstatus-tinybird-for-analytics-and-time-series-query-ingestion-before-implementing-updating-client-integrations-openstatus
description: "Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Before Implementing Updating Client Integrations Openstatus"
metadata:
  actual:
    adr_id: 6621f946-7618-4700-a3b5-f674d66e5edd
    status: accepted
    candidate_group_id: a1c4968b-fba3-4688-8ea7-a915cddc7814
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/monitor/**
      - src/content/**
      - src/lib/**
---

# Adoption of @openstatus/tinybird for Analytics and Time-Series Query Ingestion: Before Implementing Updating Client Integrations Openstatus

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/og/monitor/**`
- `src/content/**`
- `src/lib/**`

## Context

- Telemetry, monitor history, and service escalation workflows require performant access to time-series analytics data without burdening core transactional databases.
- The application layer coordinates reporting, markdown generation, and dynamic image generation using dedicated time-series query interfaces.
- Centralizing analytical query capabilities through a unified library standardizes query construction and error management across services.

## Problem Statement

Executing high-frequency time-series aggregations directly against primary transactional data stores introduces query contention and performance degradation, while fragmented analytical fetching leads to inconsistent query logic and error-handling across endpoints.

## Decision

1. MUST: Before implementing or updating client integrations with @openstatus/tinybird, the consumer MUST discover the repository dependency manifest and resolution lock artifact to verify the exact resolved version.

## Policy Block

- MUST Before implementing or updating client integrations with @openstatus/tinybird, the consumer MUST discover the repository dependency manifest and resolution lock artifact to verify the exact resolved version.

In scope:
- Application service modules, escalation reporting utilities, and route handlers requiring analytical or time-series event data.

Out of scope:
- Standard transactional entities, user authentication records, and relational persistence operations.

## Rationale

- Adopting @openstatus/tinybird offloads analytical aggregations to an optimized time-series engine, preserving primary database capacity.
- Consolidating analytical query execution behind a structured client interface ensures consistent credential management and query modeling across disparate application features.

## Consequences

Positive:
- Separates analytical workloads from transactional data stores, preventing query contention during metric aggregations.
- Provides consistent, typed access to telemetry and monitor status data across service and route handlers.
- Facilitates structured caching and resilient error handling across markdown and media generation paths.

Negative:
- Introduces an external dependency on analytical infrastructure availability for real-time status reporting.
- Requires ongoing synchronization of analytical query schemas with application-level data contracts.

## Alternatives

- Direct transactional relational database queries for time-series metrics and monitor reports (rejected)
  Rejected because: High-volume time-series metric aggregations degrade relational database performance and fail to scale for telemetry queries.
  When valid: Transactional environments where data cardinality is minimal and dedicated analytical storage introduces excessive operational overhead.
- Ad hoc HTTP fetching against remote analytical API endpoints without a dedicated client library (rejected)
  Rejected because: Raw HTTP calls lead to duplicate request configuration, lack type safety across query parameters, and scatter error-handling logic.
  When valid: Lightweight isolated scripts where introducing dependency modules is strictly restricted.

## Risks

- Unavailability or network partitioning affecting external analytics endpoints could stall reporting and generation workflows.
  Mitigation: Wrap analytical queries in defensive error handling and employ presentation cache layers to serve stale content during transient failures.
  Owner: engineering team
- Schema divergence between published telemetry pipes and application query contracts could cause runtime parsing errors.
  Mitigation: Maintain centralized client interfaces and enforce static type validation across query parameter and response models.
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
- Centralize client initialization in a shared internal module that extracts credentials and endpoint configuration from discovered environment variables.
- Implement defensive error handling around analytical calls to ensure monitoring and notification workflows degrade gracefully if remote analytics endpoints become unreachable.

## Continuation Context


Verify commands:
- Discover the repository dependency verification command from the project configuration and execute it to ensure package resolution is valid.
- Discover the project static type check script and execute it to verify that all invocations of @openstatus/tinybird satisfy exported type contracts.
- Discover and run the project test suite to verify that escalation reporting and status query workflows handle both successful results and error conditions.

Accept when:
- All analytical and time-series reporting queries compile against the verified @openstatus/tinybird interface without type or contract mismatches.
- The project test suite passes with zero regressions in escalation reporting and status markdown generation.
- Direct transactional database queries for time-series event metrics remain absent across application service layers.

## Enforcement

- Verified by: Automated static analysis and module import boundaries verified during continuous integration.
- Verified by: Peer code reviews verifying that time-series analytical operations route through the centralized library module.
- Violation handling: Pull requests introducing unapproved ad hoc query mechanisms or uncentralized analytical clients are blocked until aligned with architectural rules.
- Exception process: Submit an architectural change request detailing why analytical telemetry must be queried outside the adopted client interface, including performance impact assessments on transactional stores.