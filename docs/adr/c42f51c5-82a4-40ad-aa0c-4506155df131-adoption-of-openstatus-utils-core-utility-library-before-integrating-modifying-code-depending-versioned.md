---
version: 2
name: c42f51c5-82a4-40ad-aa0c-4506155df131-adoption-of-openstatus-utils-core-utility-library-before-integrating-modifying-code-depending-versioned
description: "Adoption of @openstatus/utils Core Utility Library: Before Integrating Modifying Code Depending Versioned"
metadata:
  actual:
    adr_id: c42f51c5-82a4-40ad-aa0c-4506155df131
    status: accepted
    candidate_group_id: 24f82739-f335-4572-9c13-bfa2a59d6c93
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
      - src/page-subscriber/**
---

# Adoption of @openstatus/utils Core Utility Library: Before Integrating Modifying Code Depending Versioned

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/services`

Applies to:
- `src/monitor/**`
- `src/page-subscriber/**`

## Context

- Service domain implementations across the services layer require common utility functions and shared helper routines during request and event processing.
- Code duplication across individual service handlers increases maintenance overhead and risks divergent implementations of cross-cutting logic.
- Static analysis demonstrates repeated imports of the internal `@openstatus/utils` package across distinct service modules including subscriber management and monitor streaming.

## Problem Statement

Multiple backend service domains require access to common, cross-cutting helper routines and data utilities. Without a standardized internal library, individual service handlers risk reimplementing identical utility logic, introducing behavioral divergence and inflating maintenance burden.

## Decision

1. MUST: Before integrating or modifying code depending on versioned modules, developers MUST inspect the repository dependency manifest and authoritative resolution artifact to verify exact dependency alignment.

## Policy Block

- MUST Before integrating or modifying code depending on versioned modules, developers MUST inspect the repository dependency manifest and authoritative resolution artifact to verify exact dependency alignment.

In scope:
- All backend service domain handlers and modules residing within the services layer.
- Cross-cutting helper functions and generic computational algorithms intended for reuse across multiple service boundaries.

Out of scope:
- Domain-specific persistence schemas, data access queries, and transaction management routines.
- External network client definitions and third-party vendor SDK wrappers.

## Rationale

- Empirical evidence confirms that `@openstatus/utils` is consistently utilized across independent service domain modules.
- Centralizing general-purpose utilities in an internal workspace package establishes clear ownership and facilitates uniform testing of common routines.
- Promoting shared utilities over inline implementations preserves focus within service handlers on domain orchestration and persistence workflows.

## Consequences

Positive:
- Eliminates redundant utility code across distinct service domains by establishing a centralized, reusable foundation.
- Guarantees consistent execution semantics for cross-cutting helper algorithms across all service handlers.
- Maintains clean separation between service orchestration logic and low-level generic computations.

Negative:
- Service implementations develop an architectural coupling to the shared internal utility package, requiring coordinated updates when utility signatures change.
- Careless additions to the shared module can accumulate bloat and inadvertently blur the boundary between generic utilities and domain-specific logic.

## Alternatives

- Local utility function definitions within individual service directories (rejected)
  Rejected because: Leads to duplicated helper implementations, divergent bug fixes, and inconsistent behavior across service domains.
  When valid: Valid only for domain-specific logic tightly coupled to a single service handler and never reused elsewhere.
- Direct reliance on fragmented third-party utility micro-packages in each service module (rejected)
  Rejected because: Increases surface area for dependency version drift and bloats service manifests without standardized project-wide conventions.
  When valid: Valid when specialized third-party algorithms are needed exclusively by one isolated service.

## Risks

- Shared utility library becoming a dumping ground for domain-specific logic, creating circular dependencies between services and utilities.
  Mitigation: Enforce strict code review and automated linting boundaries preventing domain entities or service-level packages from being imported into the shared utility module.
  Owner: engineering team
- Breaking changes in a shared utility routine impacting multiple dependent service handlers simultaneously.
  Mitigation: Require unit test coverage for all exported utility functions and adhere to semver-compatible internal refactoring practices.
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
- When adding functions to the shared utility library, ensure inputs and outputs use generic primitives rather than domain-specific entity models.
- Organize utility functions into cohesion-focused submodules to allow tree-shaking and avoid unnecessary transitive imports in consuming services.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting scripts to verify compliance with internal module import boundaries.
- Discover and run the project test suites covering both the shared utility module and dependent service handlers.

Accept when:
- All service handlers importing shared utilities successfully resolve dependencies against the internal utility module without circular reference errors.
- Static analysis and repository validation test suites pass cleanly with no redundant local utility implementations.

## Enforcement

- Verified by: Automated repository linting and static analysis checks validating module import paths.
- Verified by: Code review by senior engineers during pull requests modifying service handlers or utility definitions.
- Violation handling: Pull requests introducing duplicated general-purpose utilities or bypassing the shared utility module are blocked until refactored.
- Violation handling: Detected violations trigger review comments pointing to existing shared module primitives.
- Exception process: Exceptions must be submitted via architectural review with documented justification for why the internal utility module cannot fulfill the requirement.
- Exception process: Temporary waivers must specify a retirement or refactoring timeline to migrate ad-hoc helpers into the shared module.