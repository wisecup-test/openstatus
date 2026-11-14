---
version: 2
name: d55cbf87-01de-4438-bbb9-86b6afe2a248-internal-docs-module-architecture-for-content-resolution-and-listing-entity-resolution-utilities-resolve-specific-content
description: "Internal Docs Module Architecture for Content Resolution and Listing: Entity Resolution Utilities Resolve Specific Content"
metadata:
  actual:
    adr_id: d55cbf87-01de-4438-bbb9-86b6afe2a248
    status: accepted
    candidate_group_id: b4a95d1d-deaf-41fb-a6d5-d31fd971047b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/content/utils/**
---

# Internal Docs Module Architecture for Content Resolution and Listing: Entity Resolution Utilities Resolve Specific Content

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/content/utils/**`

## Context

- The web application requires structured access to multiple content collections across distinct domain areas.
- Content utilities require uniform mechanisms for entity lookup and collection listing without duplicating filesystem path resolution.
- The codebase establishes an internal docs module and docs configuration to encapsulate content loading and reading logic.

## Problem Statement

Without a centralized content access abstraction, individual utilities and pages would implement divergent filesystem traversal, path resolution, and entity lookup logic, leading to inconsistent entity resolution, redundant filesystem parsing, and tight coupling between consumer components and raw content files.

## Decision

1. MUST: Entity resolution utilities MUST resolve specific content items by executing slug-equality predicate lookups against collection accessors exposed by the docs module.

## Policy Block

- MUST Entity resolution utilities MUST resolve specific content items by executing slug-equality predicate lookups against collection accessors exposed by the docs module.

In scope:
- Applies to all content utility modules and services responsible for resolving, listing, and reading document collections.

Out of scope:
- Does not apply to general application routing, third-party API clients, or non-content UI components.

## Rationale

- Centralizing content collection queries within the docs module provides a single authoritative boundary for entity metadata and resolution.
- Slug-equality predicate queries on collections provide predictable, deterministic retrieval across all content types.
- Isolating path manipulation and read logic within utility helpers prevents path leakage across consumer modules.

## Consequences

Positive:
- Uniform content access interface decouples presentation pages from storage mechanisms.
- Entity resolution logic remains centralized and consistent across all content domains.
- Filesystem handling is isolated within dedicated read utilities.

Negative:
- All new content collections require corresponding accessor functions and listing integration in the docs module.
- In-memory predicate filtering across collections requires disciplined caching or indexing as collection size scales.

## Alternatives

- Direct filesystem traversal at call sites (rejected)
  Rejected because: Scattering raw path resolution and file reads across consumer utilities introduces duplicated path logic and brittle content retrieval.
  When valid: Valid only in standalone scripts operating outside the main application runtime.
- External headless content management service (rejected)
  Rejected because: Adds external runtime network overhead and operational complexity when local file-backed content satisfies project requirements.
  When valid: Valid when content authorship requires non-developer collaborative workflows.

## Risks

- Performance degradation if collection retrieval functions reload or reparse content on every slug lookup invocation.
  Mitigation: Ensure collection data functions memoize parsed results or leverage build-time caching.
  Owner: engineering team
- Schema drift between docs configuration and actual document frontmatter attributes.
  Mitigation: Validate collection entities against defined content schemas during build verification.
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
- Expose standardized collection accessors from the docs module that return collections of typed content entities.
- Implement slug lookup helpers that perform equality matching against collection records and handle missing entities gracefully.

## Continuation Context


Verify commands:
- Discover the project verification script from the root manifest and run static type checking across the content utility modules.
- Discover the project test runner from repository configuration and execute test suites targeting content resolution and listing utilities.

Accept when:
- Static type analysis passes with zero errors across all content utility modules.
- Automated test suites pass, verifying that entity lookup functions return matching items by slug and handle non-existent slugs without unhandled exceptions.

## Enforcement

- Verified by: Automated continuous integration pipeline executing type checking and test suites on pull requests.
- Verified by: Peer code review confirming content access operations route exclusively through the docs module.
- Violation handling: Pull requests containing direct filesystem reads or bypassed content lookups are blocked from merging.
- Exception process: Exceptions require documented architectural review and approval by the engineering team leads.