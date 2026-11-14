---
version: 2
name: a24249c8-cd34-4338-a7f7-08407cbbff06-map-based-ephemeral-in-memory-indexing-in-openstatus-ui-components-components-performing-keyed-lookup-over-client
description: "Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Components Performing Keyed Lookup Over Client"
metadata:
  actual:
    adr_id: a24249c8-cd34-4338-a7f7-08407cbbff06
    status: accepted
    candidate_group_id: 450fc1ba-8745-4bc8-87c9-0ea4fbb7ff7d
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Components Performing Keyed Lookup Over Client

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/blocks/**`

## Context

- Client-rendered UI components render status calendar events and markers across dynamic time windows requiring repeated lookups by date key and identifier.
- Data passed into rendering hooks must be queried frequently during layout calculation without triggering linear scans across unindexed marker arrays.
- Evidence indicates localized use of Map structures within component hooks to perform key-value association and indexed lookup rather than integration with a dedicated datastore or caching layer.

## Problem Statement

Status calendar interfaces require high-frequency lookups of markers and events by day and event identifier during render cycles, but introducing external caching or datastore infrastructure adds unnecessary network boundaries and runtime complexity for ephemeral client-side presentation data.

## Decision

1. MUST: UI components performing keyed lookup over client-side event datasets MUST use localized Map indexing structures scoped to component lifecycle hooks to ensure constant-time data access.

## Policy Block

- MUST UI components performing keyed lookup over client-side event datasets MUST use localized Map indexing structures scoped to component lifecycle hooks to ensure constant-time data access.

In scope:
- Client-side UI components handling keyed event and marker collections for presentation rendering.

Out of scope:
- Persistent server-side datastores, distributed caching layers, and cross-session application state management.

## Rationale

- Using localized Map indexing within client components provides constant-time lookups without introducing distributed caching dependencies or datastore overhead for purely visual state.
- Encapsulating index construction within component rendering hooks confines memory allocation to active view lifecycles, ensuring garbage collection when components unmount.

## Consequences

Positive:
- Constant-time O(1) lookups eliminate repeated linear array scans during calendar day rendering.
- Zero external runtime dependencies or network overhead for ephemeral view-model indexing.
- Memory footprint remains strictly scoped to the active component lifecycle.

Negative:
- Indexed datasets duplicate memory already held by raw component property arrays.
- Data indexing does not persist across route changes, requiring index reconstruction upon component remounting.

## Alternatives

- Linear array traversal using predicate searches on every render cycle (rejected)
  Rejected because: Linear searches across event markers cause unacceptable rendering overhead when scaling to large calendar day spans.
  When valid: Acceptable only when datasets contain negligible record counts where indexing overhead exceeds scan cost.
- Global client-side caching store for calendar markers and events (rejected)
  Rejected because: Global caching introduces cache invalidation complexity and lifecycle decoupling for transient display state.
  When valid: Valid when calendar event data must be shared concurrently across multiple decoupled view modules.

## Risks

- Large event collections may induce memory pressure if indices are recalculated without memoization.
  Mitigation: Encapsulate Map index generation within memoization hooks tied strictly to collection reference identity.
  Owner: frontend engineering team
- Developers may mistake ephemeral in-memory indexing for durable caching and omit proper remote data fetching.
  Mitigation: Enforce clear architectural boundaries separating transient component view models from persistent data access layers.
  Owner: frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct index maps by day key or unique identifier immediately prior to rendering iteration, ensuring that composite keys maintain deterministic formatting.
- Avoid mutating index structures outside hook boundaries to maintain referential predictability across UI component renders.

## Continuation Context


Verify commands:
- Discover the repository test runner script from the project manifest and execute component unit tests covering status calendar rendering.
- Discover and run the project typecheck and linting commands to validate component hook signatures and memory indexing patterns.

Accept when:
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

## Enforcement

- Verified by: Automated continuous integration pipeline executing repository test and linting validation suites.
- Verified by: Peer code review verifying that in-memory indexing remains scoped to component presentation layers.
- Violation handling: Continuous integration checks block pull requests introducing unindexed linear scans in high-frequency render loops or leaking component indices to global scope.
- Violation handling: Code reviewers mandate refactoring any component-level caching that attempts to bypass persistent data storage architecture.
- Exception process: Exceptions allowing global or shared client caching require review and approval by the frontend architecture team with documented performance profiling.