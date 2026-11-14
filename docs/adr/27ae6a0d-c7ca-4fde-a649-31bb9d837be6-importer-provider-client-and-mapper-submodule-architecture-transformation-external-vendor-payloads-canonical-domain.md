---
version: 2
name: 27ae6a0d-c7ca-4fde-a649-31bb9d837be6-importer-provider-client-and-mapper-submodule-architecture-transformation-external-vendor-payloads-canonical-domain
description: "Importer Provider Client and Mapper Submodule Architecture: Transformation External Vendor Payloads Canonical Domain"
metadata:
  actual:
    adr_id: 27ae6a0d-c7ca-4fde-a649-31bb9d837be6
    status: accepted
    candidate_group_id: 1c1bfc9d-9f02-4f19-ba60-a13c7cefdafb
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# Importer Provider Client and Mapper Submodule Architecture: Transformation External Vendor Payloads Canonical Domain

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/importers`

Applies to:
- `src/providers/**`

## Context

- Data ingestion integrations require connecting to external service endpoints with disparate authentication, network protocols, and payload schemas.
- Unstructured integration code risks blending HTTP transport, response parsing, data normalization, and state management into monolithic files.
- Observed importer providers consistently structure their internal responsibilities into dedicated client, mapper, external type, and shared core type modules.

## Problem Statement

External API integrations frequently suffer from tight coupling when network communication, vendor payload formats, domain normalization, and ingestion workflows are mixed together, leading to brittle integrations and fragile test suites when third-party schemas evolve.

## Decision

1. MUST: Transformation from external vendor payloads to canonical domain formats MUST be performed exclusively within the dedicated mapper module.

## Policy Block

- MUST Transformation from external vendor payloads to canonical domain formats MUST be performed exclusively within the dedicated mapper module.

In scope:
- All external service integration providers within the data importer subsystem.
- Module implementations handling external API ingestion, normalization, and state indexing.

Out of scope:
- Internal utility libraries that do not connect to external third-party service endpoints.
- Shared domain type definitions that do not perform protocol transport or payload mapping.

## Rationale

- Static analysis of provider implementations across multiple integration targets reveals consistent modularization separating client access from data normalization.
- Isolating vendor API structures into dedicated modules ensures that upstream breaking changes are contained within the vendor-specific boundary.
- Decoupling transformation logic into dedicated mapping modules allows deterministic testing without network mocking overhead.

## Consequences

Positive:
- Enforces clean separation between external transport logic, remote schema parsing, and canonical domain mapping.
- Enables testing of mapping logic with static payloads completely decoupled from transport and networking.
- Isolates upstream API contract modifications strictly to client and mapper modules without disturbing core importer interfaces.

Negative:
- Increases initial boilerplate and module count for every newly integrated provider.
- Requires navigation across separate internal submodules during debugging of integration flows.

## Alternatives

- Monolithic Single-Module Provider Architecture (rejected)
  Rejected because: Coupling HTTP requests, external response types, transformation logic, and provider orchestration within a single module degrades maintainability and complicates isolated unit testing.
  When valid: Trivial providers with one unauthenticated endpoint and no data transformation requirements.
- Shared Global Client and Mapping Subsystem (rejected)
  Rejected because: Centralizing all external provider clients and mappers creates cross-provider coupling and causes provider-specific schema changes to ripple across unrelated integrations.
  When valid: Integrations that share an identical upstream protocol and unified schema specification.

## Risks

- Drift between external API type definitions and actual runtime payloads.
  Mitigation: Maintain strict unit tests for mapping modules using recorded vendor fixtures.
  Owner: engineering team
- Inconsistent boundary enforcement leading to leaky abstractions across client and mapper layers.
  Mitigation: Define standard provider module templates and enforce import linting boundaries in continuous integration.
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
- Construct provider submodules with strict single-direction dependencies: provider orchestrator coordinates client and mapper, while client and mapper remain decoupled from each other.
- Colocate external endpoint payload type definitions within provider-specific API type modules rather than polluting shared domain type namespaces.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and dependency linting script to validate module import boundaries.
- Discover and execute the repository type-checking script across all provider submodules to confirm strict adherence to domain and API type contracts.
- Discover and execute the repository unit test suite covering client and mapper modules to verify payload normalization.

Accept when:
- All provider implementations encapsulate external network requests exclusively within dedicated client modules.
- Data transformation between provider schemas and canonical domain representations occurs solely within dedicated mapper modules.
- Automated architectural and type-checking verification scripts complete without boundary or dependency violations.

## Enforcement

- Verified by: Continuous integration dependency boundary analysis and static type checking.
- Verified by: Peer code review validating provider module separation.
- Violation handling: Continuous integration builds fail upon detection of undecomposed or circular provider imports.
- Violation handling: Code reviews block merges that bypass mapper or client module boundaries.
- Exception process: Submit an architectural exception request detailing why the integration cannot be decomposed into standard submodules.
- Exception process: Obtain written approval from the platform engineering team and record the deviation in the module documentation.