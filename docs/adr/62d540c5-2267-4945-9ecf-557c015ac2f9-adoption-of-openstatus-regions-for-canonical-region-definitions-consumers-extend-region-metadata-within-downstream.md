---
version: 2
name: 62d540c5-2267-4945-9ecf-557c015ac2f9-adoption-of-openstatus-regions-for-canonical-region-definitions-consumers-extend-region-metadata-within-downstream
description: "Adoption of @openstatus/regions for Canonical Region Definitions: Consumers Extend Region Metadata Within Downstream"
metadata:
  actual:
    adr_id: 62d540c5-2267-4945-9ecf-557c015ac2f9
    status: accepted
    candidate_group_id: e93ba485-cd34-462f-82e0-d234a52187de
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of @openstatus/regions for Canonical Region Definitions: Consumers Extend Region Metadata Within Downstream

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Multiple protocol surfaces, including HTTP API schemas and RPC handler converters, require consistent handling of geographic and infrastructure region identifiers.
- Duplicating region enumerations across divergent protocol layers introduces risk of schema drift and mapping discrepancies.
- The @openstatus/regions internal package provides a centralized source of truth for region identifiers and definitions across service boundaries.

## Problem Statement

Maintaining disparate, independently defined region schemas across HTTP validation layers and RPC protocol handlers leads to synchronization errors and inconsistent contract enforcement across system boundaries.

## Decision

1. MAY: Consumers MAY extend region metadata within downstream route schemas provided the underlying identity derives directly from @openstatus/regions definitions.

## Policy Block

- MAY Consumers MAY extend region metadata within downstream route schemas provided the underlying identity derives directly from @openstatus/regions definitions.

In scope:
- Route schemas, validation layers, RPC handlers, and protocol converters dealing with region identifiers and metadata.

Out of scope:
- Isolated client-side presentation components that consume pre-transformed display strings and do not handle region identity contracts.

## Rationale

- The codebase establishes cross-boundary consistency by importing @openstatus/regions across both HTTP route schema definitions and RPC conversion handlers.
- Centralizing region definitions in a dedicated module eliminates redundant domain modeling and prevents contract divergence between transport protocols.
- Standardizing on a shared module allows upstream changes to region availability or metadata to propagate consistently across all communication boundaries.

## Consequences

Positive:
- Guarantees contract uniformity between external REST endpoints and internal RPC interfaces for region definitions.
- Eliminates duplication and drift in region enumeration maintenance across distinct service routes.
- Provides a single authoritative module for region validation and metadata augmentation.

Negative:
- Introduces an architectural dependency on a shared internal package across distinct routing and conversion modules.
- Requires consumers to update and verify module version resolutions when region capabilities or definitions evolve.

## Alternatives

- Ad hoc inline region string literals and localized enums within each route and handler (rejected)
  Rejected because: Causes schema drift and breaks type alignment between REST API interfaces and RPC transport contracts
  When valid: In small, single-file prototypes where cross-protocol interoperability is unnecessary
- Generating region types exclusively from RPC protocol definitions into route schemas (rejected)
  Rejected because: Couples HTTP route validation directly to RPC compilation artifacts rather than a dedicated domain module
  When valid: When an architecture operates exclusively via RPC with no independent REST or OpenAPI specification requirements

## Risks

- Breaking changes in the shared region definitions could cascade across REST routes and RPC handlers simultaneously.
  Mitigation: Enforce strict semantic versioning and verify compatibility against the project lock resolution artifact prior to updates.
  Owner: engineering team
- Circular dependency or package resolution conflicts when importing the shared module into leaf services.
  Mitigation: Keep the region library isolated from higher-level service dependencies and verify module boundaries via dependency graph checks.
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
- Locate the shared region module through repository workspace manifests and import canonical region types directly into route schemas and handler converters.
- When defining input validation schemas or mapping RPC structures, consume the exported region structures directly rather than re-declaring literal unions.

## Continuation Context


Verify commands:
- Discover the workspace manifest and execute the designated static type-checking and analysis tasks to verify region import conformance.
- Discover and run the test suite to ensure schema validation and RPC converter mappings pass all contract tests without type mismatch.

Accept when:
- All route schema definitions and RPC converters import region types from the canonical module without localized duplication.
- The discovered static analysis and build tasks pass with zero type errors related to region mapping.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type verification and static analysis.
- Verified by: Peer code review auditing imports in new and modified route schemas and RPC converters.
- Violation handling: Pull requests containing locally declared region enums or duplicate string literals will fail review and must be refactored to use the canonical module.
- Exception process: Proposals to introduce alternative region representations must submit an architectural exception request reviewed and approved by the engineering leads.