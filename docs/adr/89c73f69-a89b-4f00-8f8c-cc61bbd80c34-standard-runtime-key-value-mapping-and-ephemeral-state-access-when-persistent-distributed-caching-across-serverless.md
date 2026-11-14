---
version: 2
name: 89c73f69-a89b-4f00-8f8c-cc61bbd80c34-standard-runtime-key-value-mapping-and-ephemeral-state-access-when-persistent-distributed-caching-across-serverless
description: "Standard Runtime Key-Value Mapping and Ephemeral State Access: When Persistent Distributed Caching Across Serverless"
metadata:
  actual:
    adr_id: 89c73f69-a89b-4f00-8f8c-cc61bbd80c34
    status: accepted
    candidate_group_id: 012be8c2-fbee-4f98-a859-b7530f4838b8
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Standard Runtime Key-Value Mapping and Ephemeral State Access: When Persistent Distributed Caching Across Serverless

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/content/**`
- `src/lib/**`
- `src/trpc/**`

## Context

- Static analysis identified recurring key-value retrieval and mutation invocations across route handlers and client-side presentation components.
- The observed patterns predominantly reflect standard runtime dictionary operations and request parameter parsing rather than an architectural primary datastore cache.
- Isolated usage of distributed caching clients occurs in single-purpose handlers without an established repository-wide caching abstraction.

## Problem Statement

The codebase requires a clear architectural distinction between local, single-invocation dictionary state manipulation and persistent distributed caching, preventing ad-hoc in-memory structures from being conflated with primary datastore caching layers.

## Decision

1. SHOULD: When persistent or distributed caching across serverless invocations is required, modules SHOULD adopt a standardized shared client rather than ad-hoc in-memory structures.

## Policy Block

- SHOULD When persistent or distributed caching across serverless invocations is required, modules SHOULD adopt a standardized shared client rather than ad-hoc in-memory structures.

In scope:
- Transient in-memory data associations and request query parameter lookups within isolated execution boundaries.

Out of scope:
- Distributed session storage, cross-instance caching, and persistent primary datastore operations.

## Rationale

- Standardizing local dictionary access around runtime primitives minimizes external infrastructure latency and complexity for non-persistent computations.
- Preventing ad-hoc in-memory caching in multi-instance or serverless environments prevents state desynchronization and cache coherence failures.
- Evidence indicates that centralized caching infrastructure is only warranted for high-cardinality rate limiting and persistent telemetry rather than generic request handling.

## Consequences

Positive:
- Eliminates network round-trips for transient computations within a single execution cycle.
- Maintains zero operational dependency overhead for local UI state and parameter transformations.
- Prevents state leakage across decoupled serverless function invocations.

Negative:
- In-memory state cannot be shared across concurrent requests or distributed workers.
- Requires deliberate migration to distributed caching clients when data volume or cross-session persistence needs exceed single-request lifecycles.

## Alternatives

- Mandating distributed cache infrastructure for all intermediate key-value mappings (rejected)
  Rejected because: Introduces unnecessary network latency and operational cost for purely ephemeral request-scoped data.
  When valid: Valid when data must persist across multiple serverless execution instances.
- Unconstrained process-level global in-memory caching (rejected)
  Rejected because: Causes memory leaks and non-deterministic cache drift in multi-instance and serverless deployment environments.
  When valid: Valid in single-instance long-running daemon processes with deterministic eviction policies.

## Risks

- Developers may mistakenly rely on local dictionaries for cross-request caching in multi-tenant environments.
  Mitigation: Enforce strict boundaries between request-scoped memory and shared datastore caching via static analysis and code review.
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
- Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- When introducing external caching clients, ensure keys follow structured namespaces to prevent collision across independent application domains.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type verification scripts to verify that dictionary accessors conform to type definitions.
- Execute the repository test suite to ensure request parameter handling and caching routines pass all behavioral assertions.

Accept when:
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test and linting suites.
- Verified by: Peer code review verifying separation between local state and persistent caching.
- Violation handling: Pull requests exhibiting unauthorized shared in-memory caching or unmanaged state retention are blocked from merging until corrected.
- Exception process: Exceptions for performance-critical caching must be documented with latency benchmarks and approved by the architecture review group.