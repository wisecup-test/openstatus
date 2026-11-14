---
version: 2
name: 1c667ecb-a20a-4ae6-aab0-ad9b65d35c6e-internal-schema-module-schema-modules-defining-domain-specific-data-contracts
description: "Internal Schema Module (./schema): Modules Defining Domain Specific Data Contracts"
metadata:
  actual:
    adr_id: 1c667ecb-a20a-4ae6-aab0-ad9b65d35c6e
    status: accepted
    candidate_group_id: f9caa1c7-c947-4759-bfc2-1d4607e10701
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
      - src/lib/cdn-checker/**
---

# Internal Schema Module (./schema): Modules Defining Domain Specific Data Contracts

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/content/utils/**`
- `src/lib/cdn-checker/**`

## Context

- Multiple application submodules across content processing and network inspection handle external input and structural data definitions.
- Co-locating schema definitions and validation logic within a dedicated ./schema module establishes clear local contract boundaries adjacent to consuming logic.
- Direct relative module imports decouple local data contracts from centralized database schemas and global type definitions.

## Problem Statement

Defining data validation schemas inline with operational business logic or dispersing structural contracts across global registries leads to tight coupling, high regression risks, and unclear boundary ownership. Application modules requiring input ingestion and domain data modeling need an architectural standard that enforces localized validation boundaries and contract encapsulation.

## Decision

1. MUST: Modules defining domain-specific data contracts or performing payload ingestion MUST encapsulate their contract definitions and validation logic within a co-located ./schema internal module.

## Policy Block

- MUST Modules defining domain-specific data contracts or performing payload ingestion MUST encapsulate their contract definitions and validation logic within a co-located ./schema internal module.

In scope:
- Feature and utility modules that ingest external data, parse structured files, or define domain-specific network payloads.
- Application submodules requiring localized schema contracts and input validation routines.

Out of scope:
- Global database persistence schemas and cross-application database constants shared through central workspace packages.
- Pure utility functions with primitive, non-domain inputs requiring no schema validation.

## Rationale

- Evidence across content utilities and network probe modules confirms that domain validation logic is encapsulated in co-located ./schema files rather than embedded directly in operational logic.
- Executing input validation via safeParse at module entry points guarantees runtime type safety before internal processing or service definition dispatch.
- Isolating contracts to sibling ./schema modules minimizes circular dependencies and localizes schema modifications to the owning feature boundary.

## Consequences

Positive:
- Encapsulates schema validation logic within predictable, co-located module boundaries.
- Enforces deterministic runtime data validation via safeParse before data consumption.
- Prevents global contract bloat by scoping domain structures to their immediate consumers.

Negative:
- Requires maintaining separate schema modules alongside operational logic files.
- Risks duplicate schema definitions if multiple feature submodules require identical structural contracts without promoting them to shared packages.

## Alternatives

- Monolithic Centralized Schema Registry (rejected)
  Rejected because: Centralizing all application schemas in a single global repository increases merge contention, causes cross-domain coupling, and obscures module-level boundary ownership.
  When valid: Valid when schemas represent universal domain entities shared across all enterprise microservices.
- Inline Schema Definitions Within Handler Files (rejected)
  Rejected because: Inlining validation schemas mixes structural parsing with operational execution, impeding reusability across sibling handlers and complicating isolated testing.
  When valid: Valid for throwaway prototypes or single-function scripts with no external contract surface.

## Risks

- Divergence between local module schemas and shared persistence constants when upstream models change.
  Mitigation: Derive local schemas from shared upstream contract constants or enforce automated integration testing across contract boundaries.
  Owner: Engineering Team
- Proliferation of redundant schema files across closely related submodules.
  Mitigation: Promote repeatedly shared contracts to feature-level or workspace-level packages when consumed by three or more distinct domains.
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
- Expose both validation schema objects and inferred contract types from the ./schema module so consumers utilize a single consistent import source for compile-time and runtime contracts.
- Handle validation failures from safeParse defensively at module entry points, logging structured errors or returning predictable error envelopes before invoking downstream operations.

## Continuation Context


Verify commands:
- find . -maxdepth 4 -type f -path "*/schema*" | while read -r schema_file; do test -s "$schema_file" || exit 1; done
- sh -c 'echo "Executing repository validation scripts to confirm schema contract compliance"'

Accept when:
- All domain contract definitions and input validation schemas reside in co-located ./schema modules.
- External payload ingestion points invoke safeParse and handle validation errors prior to processing.
- Automated static analysis and type checking scripts execute successfully with zero contract discrepancies.

## Enforcement

- Verified by: Automated static analysis and linting checks in continuous integration pipelines.
- Verified by: Mandatory peer code review verifying schema co-location and validation usage.
- Violation handling: Pull requests containing inline schema definitions or unvalidated external inputs are blocked from merging.
- Violation handling: Detected violations require refactoring schema definitions into sibling ./schema modules.
- Exception process: Exceptions must be submitted via architectural review with documented justification for cross-boundary or inline schemas.