---
version: 2
name: 594de9a2-0669-4050-80d8-6a1e68a7e3d9-adoption-of-openstatus-assertions-module-for-synthetic-check-definitions-and-validation-routing-procedures-not-define-hoc-local
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Routing Procedures Not Define Hoc Local"
metadata:
  actual:
    adr_id: 594de9a2-0669-4050-80d8-6a1e68a7e3d9
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Routing Procedures Not Define Hoc Local

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`

## Context

- The synthetic monitoring platform requires consistent definitions for response assertions across configuration routers and real-time execution routers.
- Disparate assertion definitions between persistence endpoints and live verification endpoints risk schema drift and incompatible validation rules.
- The internal workspace module @openstatus/assertions provides domain-level assertion schemas including discriminated unions for status, header, text body, JSON body, and record assertions.
- API routers coordinate monitor creation and diagnostic check execution by importing standardized assertion schemas from @openstatus/assertions.

## Problem Statement

Without a unified assertion domain module, API routers that configure synthetic monitors and execute live diagnostic checks would duplicate assertion schemas or define divergent validation structures. Such duplication creates operational risk where a monitor definition accepted by persistence routers fails execution in checker routers, or where evaluation results cannot be uniformly interpreted across the system.

## Decision

1. SHOULD_NOT: API routing procedures SHOULD_NOT define ad-hoc or local schemas for synthetic check assertions outside of @openstatus/assertions.

## Policy Block

- SHOULD_NOT API routing procedures SHOULD_NOT define ad-hoc or local schemas for synthetic check assertions outside of @openstatus/assertions.

In scope:
- All API router procedures that accept, validate, or mutate synthetic monitor assertion configurations.
- All execution handlers and checker dispatch endpoints that evaluate synthetic response assertions.

Out of scope:
- Generic HTTP routing procedures and database access layers that do not manipulate synthetic assertion schemas.
- Client-side rendering components that consume already-serialized assertion result payloads without direct validation needs.

## Rationale

- Centralizing synthetic assertion schemas in @openstatus/assertions eliminates schema drift between monitor persistence schemas and checker execution schemas.
- Discriminated union schemas on the type field enforce exhaustive compile-time and runtime validation across distinct check categories.
- Standardizing assertion evaluation around a shared success indicator simplifies failure detection routines across all checker execution paths.

## Consequences

Positive:
- Guarantees schema consistency between synthetic monitor configuration endpoints and on-demand checker execution endpoints.
- Enables reusable discriminated union validation across distinct monitoring protocols without duplicated schema declarations.
- Standardizes inspection of assertion evaluation outcomes through a shared success property contract.

Negative:
- Couples API router modules to internal workspace package dependency updates and structural schema modifications.
- Coordinated workspace updates are required whenever assertion schema structures or type unions expand.

## Alternatives

- Inline Assertion Schemas in Individual API Routers (rejected)
  Rejected because: Inlining assertion schemas causes duplicated validation logic, increases the likelihood of schema drift between persistence and execution, and hampers maintenance.
  When valid: When assertion types are strictly unique to an isolated single-endpoint script without cross-system implications.
- Generic Untyped Key-Value Assertion Dictionaries (rejected)
  Rejected because: Untyped key-value dictionaries prevent discriminated union validation, forgo compile-time verification, and obscure assertion semantics.
  When valid: When assertion criteria are opaque payloads passed directly to an uninspected external subsystem.

## Risks

- Breaking schema changes in @openstatus/assertions could fail input validation across multiple dependent router procedures simultaneously.
  Mitigation: Enforce strict workspace type-checking and automated test execution across all dependent router test suites before deploying module updates.
  Owner: API engineering team
- New assertion types added to the assertions library without updating discriminated union handlers in all routers could cause unhandled validation branches.
  Mitigation: Utilize exhaustive type checks on discriminated union type fields to trigger compile-time errors when unhandled types exist.
  Owner: API engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When introducing new assertion types, register the schema within @openstatus/assertions and update the discriminated union export before referencing it in router input schemas.
- Ensure diagnostic checker endpoints and monitor mutation procedures share identical assertion schema imports to preserve runtime payload parity.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and type verification script from the repository manifest and execute it to verify schema compatibility across all router packages.
- Discover and run the project test suite covering API router assertion validation and checker execution workflows.

Accept when:
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

## Enforcement

- Verified by: Continuous integration type checks verifying workspace dependency imports and schema usage.
- Verified by: Code reviews ensuring no ad-hoc assertion schemas are defined in router layers.
- Violation handling: Build rejection on contributions that introduce local assertion schema definitions instead of importing @openstatus/assertions.
- Violation handling: Mandatory remediation of type discrepancies between router inputs and shared assertion schemas.
- Exception process: Formal architecture review and team lead approval with documented justification for temporary standalone assertion definitions.