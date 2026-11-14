---
version: 2
name: d414cf28-4c81-482c-b88d-39321e76dd89-adoption-of-openstatus-importers-for-external-provider-data-ingestion-service-orchestration-workflows-validate-provider-configuration
description: "Adoption of @openstatus/importers for External Provider Data Ingestion: Service Orchestration Workflows Validate Provider Configuration"
metadata:
  actual:
    adr_id: d414cf28-4c81-482c-b88d-39321e76dd89
    status: accepted
    candidate_group_id: 09c9bc24-7a08-4bec-ac7b-e52f23f88a2c
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/import/**
---

# Adoption of @openstatus/importers for External Provider Data Ingestion: Service Orchestration Workflows Validate Provider Configuration

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/import/**`

## Context

- Service workflows must ingest monitors, components, and status pages from multiple external provider platforms.
- External provider schemas differ significantly in structure, naming conventions, and relationship hierarchies.
- Data ingestion requires preview dry runs, plan limit validation, and multi-phase transactional database persistence within strict workspace boundaries.
- Centralizing provider-specific translation into a dedicated workspace module prevents leakage of external data structures into service orchestration logic.

## Problem Statement

Integrating multiple third-party monitoring services directly within application service logic causes schema coupling, complicates tenancy validation, and increases the risk of corrupting database relationships during multi-phase imports. An architectural boundary is required to isolate external provider ingestion formats, validate import payloads against plan limits, and enforce consistent workspace tenancy during transactional database writes.

## Decision

1. MUST: Service orchestration workflows MUST validate provider configuration and execute dry-run previews using the standardized provider validation interface prior to triggering persistent import jobs.

## Policy Block

- MUST Service orchestration workflows MUST validate provider configuration and execute dry-run previews using the standardized provider validation interface prior to triggering persistent import jobs.

In scope:
- All service workflows responsible for importing external monitoring, component, or status page resources.
- Preview validation, dry-run evaluations, and quota limit checks for inbound platform data migrations.
- Multi-phase database persistence routines mapping external provider structures to internal schemas.

Out of scope:
- Internal platform data replication, export pipelines, and backup synchronization workflows.
- Direct internal API endpoints creating single resources within standard user application sessions.

## Rationale

- Evidence confirms that import preview, execution, and persistence modules uniformly rely on @openstatus/importers for provider validation and payload transformation.
- Decoupling third-party provider structures into a specialized module enables adding new provider adapters without modifying core service persistence mechanisms.
- Deriving tenancy strictly from service context prevents provider payloads from compromising multi-tenant isolation.

## Consequences

Positive:
- Isolates third-party provider schema changes from internal database schemas and service domain boundaries.
- Enforces tenant boundary isolation by deriving authorization and workspace attributes from execution context rather than external payloads.
- Provides reusable validation and dry-run preview capabilities prior to executing database transactions.

Negative:
- Adds an extra modular abstraction layer requiring synchronization between provider adapters and service persistence schemas.
- Requires intermediate in-memory identifier mapping during multi-phase imports to maintain entity relationships across transactional stages.
- Increases package surface area across the workspace, requiring lock resolution consistency across internal module dependencies.

## Alternatives

- Direct in-service third-party API integration and transformation without an intermediate module (rejected)
  Rejected because: Tightly couples service orchestration to volatile third-party schemas and complicates dry-run previews and multi-phase transactional imports.
  When valid: Admissible only in throwaway single-provider scripts with no multi-phase persistence or workspace boundary requirements.
- Dynamic runtime loading of unverified third-party provider schemas inside the execution worker (rejected)
  Rejected because: Lacks compile-time schema validation and risks introducing unvalidated provider payloads into database transactions.
  When valid: Applicable when provider schemas are defined dynamically by end users via runtime domain-specific configurations.

## Risks

- Upstream third-party API schema alterations may break provider parsing unexpectedly.
  Mitigation: Maintain automated regression test fixtures against mock provider payloads for each supported provider submodule.
  Owner: engineering team
- Partial failures during multi-phase imports may leave orphaned database records.
  Mitigation: Implement phase-based transactional rollbacks and evaluate resource count limits before write execution.
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
- Structure each external provider as a distinct submodule export under the primary importers library boundary.
- Ensure phase writers retain an in-memory source-to-internal identifier mapping structure to resolve relational dependencies across stages.
- Encapsulate dry-run execution logic within provider validation interfaces to support idempotent preview workflows.

## Continuation Context


Verify commands:
- Discover and run the project's static analysis and dependency linting suite to ensure all external provider parsing is confined to the importers module.
- Discover and execute the automated test suites covering import preview validation, limit enforcement, and multi-phase persistence workflows.

Accept when:
- All external provider integration workflows route schema mapping and validation through the dedicated importers module.
- Transactional persistence workflows derive tenancy identifiers exclusively from authenticated context rather than provider payloads.
- Automated verification test suites pass for import preview validation, limit enforcement, and multi-phase persistence.

## Enforcement

- Verified by: Automated dependency boundary checks and static import analysis during continuous integration.
- Verified by: Pull request peer reviews ensuring new external integrations reside within the importers module.
- Verified by: Integration test suites verifying preview dry runs, limit evaluations, and transactional phase executions.
- Violation handling: Continuous integration builds fail upon detecting direct third-party provider parsing outside the designated module.
- Violation handling: Code review blocks PR merges that accept unvalidated external tenancy identifiers into database write transactions.
- Violation handling: Flagged violations must be refactored into compliant module adapters before promotion to production.
- Exception process: Submit an architectural variance request detailing why an external provider requires non-standard ingestion patterns.
- Exception process: Obtain explicit review and approval from platform service maintainers before bypassing module boundaries.
- Exception process: Document the deviation in the affected service documentation alongside an active tracking issue.