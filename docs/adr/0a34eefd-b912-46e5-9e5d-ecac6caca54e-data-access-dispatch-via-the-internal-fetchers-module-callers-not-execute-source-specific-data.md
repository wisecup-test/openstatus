---
version: 2
name: 0a34eefd-b912-46e5-9e5d-ecac6caca54e-data-access-dispatch-via-the-internal-fetchers-module-callers-not-execute-source-specific-data
description: "Data Access Dispatch via the Internal fetchers Module: Callers Not Execute Source Specific Data"
metadata:
  actual:
    adr_id: 0a34eefd-b912-46e5-9e5d-ecac6caca54e
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Callers Not Execute Source Specific Data

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/status-fetcher`

Applies to:
- `scripts/**`

## Context

- Data retrieval across diverse entry sources requires a mechanism to decouple query routing from source-specific extraction details.
- The status fetching subsystem defines an internal fetchers module where individual handlers advertise compatibility with entries via predicate checks.
- Execution scripts query available fetcher definitions dynamically using collection search rather than maintaining centralized conditional routing branches.

## Problem Statement

Centralized conditional ladders for data retrieval create tight coupling between dispatcher code and concrete fetcher implementations, increasing maintenance friction when adding new source handlers.

## Decision

1. MUST_NOT: Callers MUST NOT execute source-specific data fetching operations by directly instantiating or coupling to concrete fetcher classes.

## Policy Block

- MUST_NOT Callers MUST NOT execute source-specific data fetching operations by directly instantiating or coupling to concrete fetcher classes.

In scope:
- Data retrieval operations dispatched across heterogeneous entries within the status fetching subsystem.

Out of scope:
- Direct data access operations with static, single-source bindings that require no polymorphic dispatch.

## Rationale

- Selecting handlers dynamically via canHandle isolates individual fetcher behaviors and preserves open-closed extensibility for data sources.
- Encapsulating source retrieval logic behind uniform fetcher interfaces prevents orchestration scripts from taking direct dependencies on concrete data extraction mechanisms.
- Predicate-based selection allows each fetcher implementation to self-determine entry compatibility without exposing internal parsing rules to the caller.

## Consequences

Positive:
- Decouples data access dispatching routines from concrete source retrieval implementations.
- Enables addition of new source fetchers without modifying central orchestration workflows.
- Standardizes entry compatibility evaluation across independent fetcher modules.

Negative:
- Introduces linear search latency proportional to the count of registered fetchers during entry dispatch.
- Defers missing-handler detection to runtime when an unsupported entry type is evaluated.

## Alternatives

- Centralized switch dispatch based on entry type discriminator fields (rejected)
  Rejected because: Requires modifying a central dispatch function whenever a new fetcher is introduced, violating the open-closed principle.
  When valid: Valid when entry variants are strictly finite and fixed across the entire application lifecycle.
- Keyed registry map indexed by entry category strings (rejected)
  Rejected because: Restricts matching logic to exact string equality and prevents multi-attribute compatibility evaluation supported by canHandle.
  When valid: Valid when entry classification relies strictly on discrete, normalized identifier keys.

## Risks

- Multiple fetchers may satisfy canHandle for the same entry, leading to unintended handler precedence based on registration order.
  Mitigation: Ensure predicate conditions are mutually exclusive or establish explicit priority ordering in the fetcher registry.
  Owner: engineering team
- Runtime failure when an entry matches no registered fetcher.
  Mitigation: Enforce exhaustive error handling or fallback handler mechanisms for unmatched queries.
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
- Register all available fetcher implementations in an ordered collection before executing entry resolution routines.
- Implement canHandle as a pure predicate function without side effects or external network calls.

## Continuation Context


Verify commands:
- Discover and run the project test suite using the active environment test runner to verify fetcher dispatch and canHandle behavior.
- Discover and execute the repository static type verification script to ensure fetcher interface compliance.

Accept when:
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

## Enforcement

- Verified by: Continuous integration test passes validating fetcher resolution and canHandle execution.
- Verified by: Peer code review for all new fetcher additions to verify predicate specificity.
- Violation handling: Pull requests bypassing canHandle dispatch in favor of hardcoded concrete fetcher calls must be blocked.
- Exception process: Architectural exceptions require written sign-off by the engineering team documenting performance constraints that necessitate direct coupling.