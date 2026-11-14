---
version: 2
name: f1a59a8f-4571-402c-b261-5e9dea608f5e-adoption-of-openstatus-api-src-router-effective-status-for-centralized-effective-status-evaluation-modules-not-independently-implement-e2515b436995ae3a
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Not Independently Implement Bespoke Effective"
metadata:
  actual:
    adr_id: f1a59a8f-4571-402c-b261-5e9dea608f5e
    status: accepted
    candidate_group_id: 8a4fe0ef-90ea-4fed-b69f-f54f0e8f7fa9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Modules Not Independently Implement Bespoke Effective

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/content/**`
- `src/lib/**`

## Context

- Service and component operational status must be consistently evaluated across escalation handlers, markdown report generators, and interactive status user interfaces.
- Multiple application modules require access to the effective health state of components without duplicating calculation logic.
- The internal router module @openstatus/api/src/router/effective-status provides centralized evaluation contracts across disparate consumer contexts.

## Problem Statement

Calculating service and component operational status independently across background reporting jobs and user-facing presentation layers introduces calculation drift and conflicting incident representations. A unified module is required to derive and expose effective status across all consumer contexts.

## Decision

1. MUST_NOT: Modules MUST NOT independently implement bespoke effective status derivation or component health aggregation heuristics that bypass the shared router contract.

## Policy Block

- MUST_NOT Modules MUST NOT independently implement bespoke effective status derivation or component health aggregation heuristics that bypass the shared router contract.

In scope:
- All application modules that calculate, display, or escalate service and component operational status.
- Server-side escalation jobs, markdown report generators, and client-rendered status route views.

Out of scope:
- Static marketing pages and documentation components that do not render real-time service health indicators.
- Third-party webhook ingestion pipelines operating outside the core status domain.

## Rationale

- Consolidating effective status calculation into a single internal module ensures uniform operational assessments across alerts, public status pages, and escalation pipelines.
- Observed evidence across multiple critical application modules demonstrates that consistent domain logic prevents divergent status representations between backend reporting and frontend rendering.
- Using a shared workspace router contract simplifies testing and maintenance of multi-component status aggregation rules.

## Consequences

Positive:
- Establishes a single source of truth for service and component health aggregation logic across the system.
- Eliminates disparate status calculation algorithms between background automation processes and interactive user interfaces.
- Shared typing and query contracts streamline cross-functional development across reporting and visualization features.

Negative:
- Tight coupling to the internal effective status router module increases the blast radius of interface changes across multiple application layers.
- Shared module contract modifications require coordinated verification across both backend escalation workflows and frontend views.

## Alternatives

- Decentralized Status Derivation per Domain Module (rejected)
  Rejected because: Re-implementing effective status logic within individual report generators and view components risks calculation drift and conflicting incident status reports.
  When valid: Valid only in decoupled standalone utilities that operate completely disconnected from the primary platform status domain.
- Database View or Stored Aggregation (rejected)
  Rejected because: Moving effective status derivation exclusively into database queries reduces flexibility for dynamic client queries and escalation rules that depend on multi-source evaluations.
  When valid: Valid when query volume demands pre-aggregated read replicas with fixed evaluation algorithms.

## Risks

- Changes to effective status calculation contracts can break multiple downstream surfaces simultaneously.
  Mitigation: Enforce automated integration test suites and strict type contracts across shared router interfaces.
  Owner: Platform Engineering
- Direct coupling across server escalation logic and client presentation layers can introduce dependency cycles if boundaries are misconfigured.
  Mitigation: Isolate router interface definitions in clean workspace package boundaries and enforce architectural import linting.
  Owner: Frontend Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Examine the workspace dependency definitions to locate the internal router package exports and type declarations for effective status.
- Verify that status escalation routines and client presentation queries share identical parameter shapes when querying effective status.

## Continuation Context


Verify commands:
- Discover and execute the project dependency resolution validation script to confirm lock-artifact consistency.
- Discover and execute the repository static analysis and module boundary linter to verify import conformance with the shared router module.
- Discover and execute the automated test suite covering effective status calculation and consumer workflows.

Accept when:
- All status calculations and display surfaces derive state exclusively through the exported interfaces of the effective status router.
- Static boundary verification confirms zero direct or duplicated status evaluation implementations across consumer modules.
- The project verification suite passes without warnings or unresolved dependencies.

## Enforcement

- Verified by: Static analysis and architectural boundary checks during continuous integration pipelines.
- Verified by: Automated unit and integration test suites validating status aggregation results.
- Verified by: Peer code review on all pull requests modifying status evaluation workflows.
- Violation handling: Pull requests introducing custom status derivation algorithms will be blocked by automated verification checks.
- Violation handling: Violations identified during code review must be refactored to consume the shared router module prior to merge.
- Exception process: Architectural review approval is required for any domain-specific status overrides or alternative computation paths.
- Exception process: Approved exceptions must be registered with explicit rationale and defined deprecation or expiration timelines.