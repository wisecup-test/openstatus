---
version: 2
name: c725d060-c6de-4bd5-a68f-b9b86e334fad-adoption-of-openstatus-api-client-module-for-command-menu-domain-operations-command-menu-entity-resolution-coordinate-localized
description: "Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Entity Resolution Coordinate Localized"
metadata:
  actual:
    adr_id: c725d060-c6de-4bd5-a68f-b9b86e334fad
    status: accepted
    candidate_group_id: 1c3677cf-a2cc-42f7-ae4b-7c57addf2a75
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/command-menu/groups/**
---

# Adoption of @openstatus/api Client Module for Command Menu Domain Operations: Command Menu Entity Resolution Coordinate Localized

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/command-menu/groups/**`

## Context

- The dashboard application requires unified access to domain entities and status page definitions across search and command menu navigation scopes.
- Bespoke API integrations or duplicated data contracts across command menu subgroups introduce schema drift and inconsistent entity resolution.
- Static analysis of command menu scoping modules reveals shared consumption of the centralized @openstatus/api module alongside localized navigation types and constants.

## Problem Statement

Without a standardized client library for domain entities, navigation components risk duplicating entity schemas, misaligning identifier contracts, and fragmenting API communication across command menu groups.

## Decision

1. SHOULD: Command menu entity resolution SHOULD coordinate localized cache lookup structures with entity identifiers retrieved from @openstatus/api contracts.

## Policy Block

- SHOULD Command menu entity resolution SHOULD coordinate localized cache lookup structures with entity identifiers retrieved from @openstatus/api contracts.

In scope:
- Dashboard command menu components and navigation group modules requiring access to domain entity definitions and remote services.
- Frontend navigation scopes resolving resource identifiers and domain status structures.

Out of scope:
- Isolated static presentation components that require no domain service integration or entity lookups.
- External third-party integration webhooks operating outside the dashboard client runtime.

## Rationale

- Adopting @openstatus/api centralizes domain contract schemas and client interactions across navigation groups, eliminating schema drift.
- Reusing standardized client models enables consistent entity lookups and in-memory caching across command scopes.
- Centralization aligns frontend command navigation with core service boundaries defined in the shared API package.

## Consequences

Positive:
- Unified domain entity contracts across all dashboard command menu scopes.
- Reduced boilerplate and duplicate network handling in UI navigation groups.
- Coherent integration between domain models and localized navigation caching layers.

Negative:
- Tighter coupling between command menu UI modules and internal @openstatus/api package contract evolution.
- Changes to API package exports require coordinated updates across all dependent command menu groups.

## Alternatives

- Direct fetch calls and inline type definitions within each command menu scope (rejected)
  Rejected because: Duplicates domain schemas, increases maintenance overhead, and risks protocol divergence across scopes
  When valid: Prototyping isolated proof-of-concept features prior to formal domain modeling
- Adoption of a centralized state management store that encapsulates @openstatus/api calls (deferred)
  Rejected because: Adds unnecessary state abstraction layers for lightweight command menu navigation and lookup operations
  When valid: Complex multi-step dashboard workflows that require cross-component reactive mutation synchronization

## Risks

- Breaking contract changes in @openstatus/api can disrupt command menu resolution.
  Mitigation: Enforce contract verification through project type-checking and automated build validation.
  Owner: Engineering Team
- Uncached or redundant API calls during rapid command menu search inputs.
  Mitigation: Utilize localized identifier map caching layers to optimize entity title and navigation lookups.
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
- Ensure command navigation scope modules import resource types directly from @openstatus/api and co-locate scope-specific static configurations.
- Implement in-memory lookup maps to resolve entity titles and navigation paths without triggering duplicate remote queries during command filtering.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify that command menu scoping modules only import authorized domain client packages.
- Execute the repository type-checking script across dashboard workspace packages to confirm type compatibility with @openstatus/api exports.
- Run the project test suite to verify that command menu group lookups and cache resolutions operate correctly.

Accept when:
- Static analysis validates that command menu modules consume @openstatus/api without unauthorized ad-hoc network clients.
- Type validation succeeds with zero type errors across all command menu navigation groups.
- Automated tests pass, verifying correct entity navigation and identifier lookups.

## Enforcement

- Verified by: Continuous integration static analysis checks enforcing module dependency boundaries.
- Verified by: Automated workspace type-checking verifying contract adherence.
- Verified by: Pull request architectural code reviews for command menu navigation changes.
- Violation handling: Continuous integration builds fail upon detection of unauthorized direct network clients or unverified dependencies.
- Violation handling: Pull requests containing duplicated domain models or protocol divergence are blocked pending alignment with @openstatus/api.
- Exception process: Submit an architectural variance proposal detailing why domain contracts cannot be served by @openstatus/api.
- Exception process: Obtain explicit review and approval from dashboard architecture leads prior to merging exceptions.