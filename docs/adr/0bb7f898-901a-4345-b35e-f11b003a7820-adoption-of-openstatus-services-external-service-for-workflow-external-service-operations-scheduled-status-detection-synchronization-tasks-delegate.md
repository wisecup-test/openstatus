---
version: 2
name: 0bb7f898-901a-4345-b35e-f11b003a7820-adoption-of-openstatus-services-external-service-for-workflow-external-service-operations-scheduled-status-detection-synchronization-tasks-delegate
description: "Adoption of @openstatus/services/external-service for Workflow External Service Operations: Scheduled Status Detection Synchronization Tasks Delegate"
metadata:
  actual:
    adr_id: 0bb7f898-901a-4345-b35e-f11b003a7820
    status: accepted
    candidate_group_id: 0255ec26-c1c0-4acf-84cf-6c5611895c6c
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# Adoption of @openstatus/services/external-service for Workflow External Service Operations: Scheduled Status Detection Synchronization Tasks Delegate

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/cron/**`

## Context

- Background workflow tasks execute periodic routines to detect external service configurations and synchronize provider status.
- Direct persistence queries from distributed cron jobs risk scattering domain validation rules and persistence invariants across disparate task definitions.
- The codebase provides @openstatus/services/external-service as an internal domain module to govern external service lifecycle operations and data access.

## Problem Statement

Scheduled background workflows require a consistent mechanism to retrieve, update, and detect external service statuses without duplicating database persistence logic, violating domain boundaries, or causing synchronization inconsistencies across asynchronous executions.

## Decision

1. SHOULD: Scheduled status detection and synchronization tasks SHOULD delegate upstream provider responses to service boundary methods rather than embedding custom persistence transformation logic within workflow definitions.

## Policy Block

- SHOULD Scheduled status detection and synchronization tasks SHOULD delegate upstream provider responses to service boundary methods rather than embedding custom persistence transformation logic within workflow definitions.

In scope:
- Scheduled background workflows and asynchronous jobs managing external service state, status detection, or entity synchronization.

Out of scope:
- Client-facing presentation components and stateless utility routines operating independently of external service domain entities.

Exceptions:
- EXC-20-001: A low-level administrative data repair requires direct transactional execution that cannot be expressed via the service API.

## Rationale

- Centralizing external service queries and mutations within @openstatus/services/external-service prevents divergent database operations across distinct workflow scripts.
- Reusing the internal domain module preserves architectural boundaries by decoupling task scheduling orchestration from underlying storage primitives.
- Providing a uniform service abstraction simplifies mocking and validation within workflow integration test suites.

## Consequences

Positive:
- Standardizes external service domain interactions across asynchronous background routines.
- Encapsulates data persistence rules and schema changes behind a unified module boundary.
- Simplifies testing and verification by allowing workflow routines to interact with a cohesive service contract.

Negative:
- Couples background workflow jobs to the interface release cycle of the internal service package.
- Requires workflow developers to accommodate service abstraction layers rather than issuing arbitrary bespoke queries.

## Alternatives

- Direct persistence layer queries from scheduled workflow routines (rejected)
  Rejected because: Bypassing the service layer scatters persistence schemas and synchronization invariants across cron routines.
  When valid: Valid only during standalone one-off schema migrations or administrative recovery scripts.
- Workflow-local repository implementations for external service entities (rejected)
  Rejected because: Creating duplicate data access abstractions inside the workflow package fragments domain logic already maintained in the shared services package.
  When valid: Valid when workflow data requirements completely diverge from shared service representations.

## Risks

- Breaking interface adjustments in the domain service module will cause concurrent failures across multiple workflow jobs.
  Mitigation: Maintain strict semantic interface contracts, execute automated workspace regression tests, and apply gradual deprecation cycles for modified service methods.
  Owner: Backend Engineering Team
- Service boundary overhead and redundant entity queries may introduce execution latency during intensive cron runs.
  Mitigation: Profile domain service latency within workflow executions and employ localized in-memory caching for repeated lookups within individual task runs.
  Owner: Workflow Platform Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Verify that module imports in workflow packages resolve exclusively to the official public boundary of the adopted service package.
- Ensure error handling strategies in asynchronous tasks explicitly capture and log exceptions propagated across the service boundary.

## Continuation Context


Verify commands:
- Discover and run the workspace static analysis script to verify module import constraints across workflow packages.
- Discover and run the workspace test runner to execute workflow integration tests against the service boundary.

Accept when:
- Static analysis validates that workflow routines import external service operations exclusively from the approved service module without unauthorized persistence access.
- Workspace test suites execute and pass with all external service interactions conforming to the service contract.

## Enforcement

- Verified by: Static analysis and import boundary rules evaluated in automated integration pipelines.
- Verified by: Mandatory peer code review focusing on module dependency compliance for workflow implementations.
- Violation handling: Pull requests introducing direct database operations or bypassing the service boundary are blocked from merging until refactored.
- Exception process: Engineers must submit an architecture exception request detailing why the service interface is insufficient.
- Exception process: Exceptions require formal sign-off from the architecture lead before code integration.