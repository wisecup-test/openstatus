---
version: 2
name: 075e6226-98d3-4431-900e-54d23a9e4269-adoption-of-openstatus-regions-for-unified-geographic-region-modeling-applications-not-define-local-geographic-region
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Applications Not Define Local Geographic Region"
metadata:
  actual:
    adr_id: 075e6226-98d3-4431-900e-54d23a9e4269
    status: accepted
    candidate_group_id: 903339f8-5d24-42ec-828d-2745bb17f92b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/data/**
      - src/lib/checker/**
---

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Applications Not Define Local Geographic Region

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/content/**`
- `src/data/**`
- `src/lib/checker/**`

## Context

- Network diagnostic capabilities, latency checks, and CDN probe inspection features require consistent geographic location identifiers across client interfaces and server route handlers.
- Multiple subsystems validate regional parameters, parse probe payloads, and render region-indexed latency metrics.
- A unified workspace package provides shared geographic region constants, validation schemas, and metadata across application boundaries.

## Problem Statement

Defining geographic region identifiers, display labels, and validation schemas independently across server route handlers, diagnostic clients, and metric visualization tables introduces naming divergence, payload deserialization failures, and contract drift across the application.

## Decision

1. MUST_NOT: Applications MUST_NOT define local geographic region enums, custom region identifier string unions, or duplicate region metadata outside of @openstatus/regions.

## Policy Block

- MUST_NOT Applications MUST_NOT define local geographic region enums, custom region identifier string unions, or duplicate region metadata outside of @openstatus/regions.

In scope:
- All application modules, API route handlers, background tasks, and UI components handling geographic region identifiers, edge locations, or regional latency metrics.

Out of scope:
- Generic utilities and visual primitives with no awareness of or dependency on network locations, probe endpoints, or geographic data.

## Rationale

- Evidence across nine distinct files confirms that network diagnostic and latency visualization features rely on a shared geographic location abstraction.
- Centralizing region schemas ensures consistent request validation between public diagnostic routes and backend probe dispatchers.
- Using a single workspace module for region metadata prevents fragmentation of display titles, flag assets, and probe identifiers across presentation layers.

## Consequences

Positive:
- Establishes a single canonical source of truth for edge locations, probe endpoints, and geographic display metadata.
- Eliminates data serialization mismatches between frontend input forms, server route validation schemas, and external probe runners.
- Simplifies regional metrics aggregation and table rendering by standardizing region keys across client and server tiers.

Negative:
- Creates an architectural coupling across server route handlers, client views, and diagnostic utilities to the shared package release cycle.
- Modifications or additions to regional definitions require coordinating dependency version updates across consuming workspaces.

## Alternatives

- Decentralized local region enums and type definitions per feature module (rejected)
  Rejected because: Introduces contract drift and naming discrepancies between client UI tables, API route handlers, and backend probe runners.
  When valid: Isolated single-file experiments or prototypes that do not integrate with regional network probes or shared metrics.
- Dynamic runtime region discovery via external API endpoints on every request (rejected)
  Rejected because: Introduces network latency, runtime failure modes during probe initialization, and overhead for client-side rendering.
  When valid: Environments where edge locations change dynamically at runtime without scheduled deployment cycles.

## Risks

- Breaking changes to region keys or schema contracts in the shared module propagate breaking failures across dependent client and server routes.
  Mitigation: Establish comprehensive schema regression test suites within the shared package to guarantee backward compatibility for region keys.
  Owner: engineering team
- Desynchronization between consuming packages leading to runtime mismatches when parsing edge probe payloads.
  Mitigation: Automate lock artifact synchronization across the workspace to avoid version skew between dependent packages.
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
- Inspect the workspace structure to identify exports provided by the shared region module before introducing new probe target schemas.
- Ensure all regional metric lookups and filtering operations consume the canonical region identifiers exported by the shared module.

## Continuation Context


Verify commands:
- Discover the test script in the project dependency manifest and execute the test suite covering regional schema validation and probe routing.
- Discover the type inspection and static analysis script in the workspace configuration and execute it to verify all region imports resolve without error.

Accept when:
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline that flag custom region enumerations.
- Verified by: Schema validation tests verifying that all region inputs adhere to the shared module validation contracts.
- Verified by: Code review by architectural peers before merging regional routing or latency visualization changes.
- Violation handling: Pull requests defining local region string unions or ad-hoc region schemas are blocked by automated review gates.
- Violation handling: Static analysis errors must be resolved by replacing local definitions with imports from the shared module before merging.
- Exception process: Submit an architectural review request outlining why a localized or external region source is strictly necessary.
- Exception process: Document temporary exemptions in the module review ticket with an assigned sunset date for reconciliation.