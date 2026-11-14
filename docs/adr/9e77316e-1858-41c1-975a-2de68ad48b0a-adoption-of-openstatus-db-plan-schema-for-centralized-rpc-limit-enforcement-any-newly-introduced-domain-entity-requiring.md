---
version: 2
name: 9e77316e-1858-41c1-975a-2de68ad48b0a-adoption-of-openstatus-db-plan-schema-for-centralized-rpc-limit-enforcement-any-newly-introduced-domain-entity-requiring
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Any Newly Introduced Domain Entity Requiring"
metadata:
  actual:
    adr_id: 9e77316e-1858-41c1-975a-2de68ad48b0a
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Any Newly Introduced Domain Entity Requiring

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/rpc/handlers/**`

## Context

- Multiple RPC service endpoints evaluate resource quotas and tier limits across distinct domain areas.
- Subscription plans and resource boundaries require consistent validation across all server handlers to prevent unauthorized resource allocation.
- Centralizing schema definitions within a shared database workspace module ensures uniform entitlement rules across the service layer.

## Problem Statement

Decentralized or duplicated resource limit definitions across independent RPC domain handlers lead to configuration drift, inconsistent quota validation, and maintenance overhead when subscription plan tiers change.

## Decision

1. MUST: Any newly introduced domain entity requiring quota constraints MUST define its entitlement boundaries within the shared @openstatus/db plan schema module before exposing creation endpoints.

## Policy Block

- MUST Any newly introduced domain entity requiring quota constraints MUST define its entitlement boundaries within the shared @openstatus/db plan schema module before exposing creation endpoints.

In scope:
- Server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

Out of scope:
- Client-side UI components displaying informational pricing badges without backend authority.
- Public or unauthenticated endpoints that do not operate on plan-governed account resources.

## Rationale

- Centralizing plan limits within @openstatus/db/src/schema/plan/schema guarantees that all RPC domain routes evaluate account limits against identical schema constraints.
- Reusing database schema definitions for runtime limit validation prevents synchronization errors and configuration drift between database migrations and API handler checks.
- Decoupling limit evaluation into dedicated route handler modules keeps domain RPC implementations focused while ensuring consistent entitlement enforcement.

## Consequences

Positive:
- Guarantees consistent enforcement of plan tiers and resource quotas across monitor, notification, status page, and private location domains.
- Establishes a single source of truth for plan entitlements, reducing administrative overhead and preventing billing and quota discrepancies.
- Facilitates centralized updates to pricing tier structures without modifying individual route business logic.

Negative:
- Creates direct coupling between RPC handler modules and the internal database schema package structure.
- Requires updates to the shared database package whenever a new domain adds resource limits, expanding cross-package coordination surface area.

## Alternatives

- Decentralized domain-level quota constants defined within individual handler modules (rejected)
  Rejected because: Leads to configuration drift, duplicate plan specifications, and inconsistent quota enforcement across feature boundaries.
  When valid: Valid only in throwaway prototypes or isolated standalone services with no shared billing or plan model.
- Dynamic remote querying of billing and plan service APIs at each RPC request (rejected)
  Rejected because: Introduces unnecessary network latency and operational failure modes for static plan entitlement boundaries.
  When valid: Valid when entitlements fluctuate dynamically based on external metering providers in real time.

## Risks

- Breaking changes to the shared plan schema module could impact all consuming RPC limit handlers simultaneously.
  Mitigation: Strict semantic type definitions and automated workspace-wide static analysis and test suites run in continuous integration.
  Owner: backend engineering team
- Deep path imports into internal schema subpaths could break if package internal file layouts are refactored.
  Mitigation: Expose stable package entry points or barrel exports from the database package for plan schemas.
  Owner: backend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Structure quota evaluation logic into dedicated limit handler modules within each RPC feature domain to maintain modularity.
- Ensure all plan limit calculations and entitlement thresholds are consumed directly from the plan schema exports rather than derived through bespoke database queries.

## Continuation Context


Verify commands:
- Discover and run the workspace type checker and static analysis script to verify that RPC limit handlers import quota definitions solely from the canonical plan schema module.
- Discover and run the project test runner across all limit handler test suites to verify quota enforcement logic against defined plan schemas.

Accept when:
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace static analysis and test validation.
- Verified by: Architectural and peer code reviews verifying adherence to centralized schema imports for entitlement checks.
- Violation handling: Pull requests containing decentralized quota constants or hardcoded limit definitions must be rejected.
- Violation handling: Code violating boundary rules must be refactored to import canonical plan schema structures before merging.
- Exception process: Exceptions for temporary or experimental entitlement overrides require approval from the backend architectural lead and must be tracked with a deprecation issue.