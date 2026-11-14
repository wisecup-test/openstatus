---
version: 2
name: 2ee8e7a0-85cf-49b9-82eb-7396f451c991-adoption-of-nanoid-for-unique-identifier-generation-components-not-implement-custom-random-string
description: "Adoption of nanoid for Unique Identifier Generation: Components Not Implement Custom Random String"
metadata:
  actual:
    adr_id: 2ee8e7a0-85cf-49b9-82eb-7396f451c991
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Components Not Implement Custom Random String

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/**`

## Context

- Backend services manage asynchronous operations such as multi-step confirmation workflows and distributed cache storage requiring unique, non-colliding action identifiers.
- RPC interceptors require consistent correlation identifiers across request contexts and metadata propagation headers.
- The codebase requires a standardized identifier generation mechanism to avoid divergent entropy characteristics and redundant third-party dependencies.

## Problem Statement

Server-side workflows and RPC interceptors require compact, collision-resistant unique identifiers for correlation tracking, cache keying, and asynchronous confirmation states. Lacking a unified library standard risks fragmented identifier formatting, disparate entropy guarantees, and unnecessary dependency bloat across backend services.

## Decision

1. MUST_NOT: Components MUST_NOT implement custom random string generators or introduce alternative unique identifier libraries for server-side correlation keys and action tracking.

## Policy Block

- MUST_NOT Components MUST_NOT implement custom random string generators or introduce alternative unique identifier libraries for server-side correlation keys and action tracking.

In scope:
- Generation of unique identifiers for server-side asynchronous workflows, confirmation tokens, and state keys.
- Generation of request correlation identifiers and context metadata within RPC interceptors.

Out of scope:
- Database primary keys managed directly by database auto-increment or relational sequences.
- Client-side identifier generation managed outside backend service boundaries.

Exceptions:
- EXC-20-001: An external integration requires a specific standardized identifier format mandated by external protocols.

## Rationale

- Evidence across route handlers and interceptors shows nanoid consistently adopted as a core library dependency for generating non-sequential identifiers.
- Using nanoid provides uniform string representation suitable for distributed key-value caches, HTTP headers, and RPC context maps.
- Standardizing on a single identifier utility prevents redundant generator packages and guarantees consistent collision resistance across backend modules.

## Consequences

Positive:
- Consistent string format and entropy characteristics across all server action keys and request correlation identifiers.
- Reduced dependency proliferation by preventing concurrent usage of multiple disparate identifier libraries.
- Direct interoperability with string-based cache stores, schema validators, and RPC context maps.

Negative:
- Creates a shared third-party dependency that must be maintained and audited across server packages.
- Custom string length or alphabet requirements require explicit configuration parameters rather than standardized defaults.

## Alternatives

- Standard UUID generation (rejected)
  Rejected because: Produces longer formatted strings that increase memory and storage overhead in high-throughput key-value caches and does not support compact variable lengths.
  When valid: Valid when strict compliance with external standardized UUID formats is required by external protocols.
- Database-generated sequential sequences (rejected)
  Rejected because: Couples identifier creation to persistent database round-trips, making it unsuitable for ephemeral cache keys and pre-persistence action tokens.
  When valid: Valid for strictly ordered internal database primary keys.

## Risks

- Entropy degradation or collision risk if custom lengths are improperly specified in generation calls.
  Mitigation: Enforce standard invocation defaults and validate identifier schemas using strict input validation.
  Owner: Backend Engineering Team
- API breaking changes across major library versions causing runtime failures during upgrades.
  Mitigation: Strictly adhere to the mandatory lock-version grounding policy prior to dependency updates.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Combine generated identifier strings with domain-specific prefix constants when forming distributed storage keys to ensure clear isolation across cache partitions.
- Integrate identifier generation within interceptors prior to downstream context propagation to guarantee traceability throughout the entire request lifecycle.

## Continuation Context


Verify commands:
- Discover the repository build configuration and execute the project test suite to validate identifier generation and cache key resolution.
- Execute the repository static analysis and type verification routines to ensure all identifier consumers conform to the library interface.

Accept when:
- All unit and integration tests exercising asynchronous action stores and RPC interceptors pass without identifier generation errors.
- Static type verification confirms nanoid is imported and called in accordance with its resolved library type definitions.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites and type checkers.
- Verified by: Peer code review verifying that new routes and interceptors do not introduce unapproved identifier utilities.
- Violation handling: Pull requests introducing alternative identifier generators or unverified custom random functions are blocked until refactored to use nanoid.
- Exception process: Submit an architectural exception request detailing external integration constraints for Architecture Lead review.