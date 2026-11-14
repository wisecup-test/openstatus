---
version: 2
name: 585143c4-68ec-4f2e-a9ac-3f6d880e60d2-nuqs-server-adoption-for-server-side-search-parameter-parsing-server-components-not-directly-access-raw
description: "nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Components Not Directly Access Raw"
metadata:
  actual:
    adr_id: 585143c4-68ec-4f2e-a9ac-3f6d880e60d2
    status: accepted
    candidate_group_id: 0b22b5e0-eabb-47cb-98c4-c984563bd2c6
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# nuqs/server Adoption for Server-Side Search Parameter Parsing: Server Components Not Directly Access Raw

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`

## Context

- Server-rendered web application routes require consistent extraction, validation, and type coercion of URL search parameters.
- Ad-hoc string parsing across route boundaries leads to fragmented parameter handling and runtime type errors.
- The codebase standardizes server-side query parameter contracts by importing server parsers from the adopted library across multiple route entrypoints.

## Problem Statement

Server-rendered routes frequently consume URL search parameters to configure page state. Without a unified schema and validation boundary, route entrypoints resort to inconsistent manual type casting and string parsing, producing runtime exceptions and contract divergence between server rendering and client navigation.

## Decision

1. MUST_NOT: Server components MUST_NOT directly access raw query dictionaries without passing them through validated library parsers.

## Policy Block

- MUST_NOT Server components MUST_NOT directly access raw query dictionaries without passing them through validated library parsers.

In scope:
- All server-side components, route entrypoints, and server loaders that ingest URL query parameters.

Out of scope:
- Static routes and components that do not read or process URL search parameters.
- Pure background utility modules detached from web application routing contexts.

## Rationale

- The pattern is consistently observed across multiple application route surfaces requiring search parameter handling.
- Standardizing on server-specific parsing modules guarantees type safety and structural validation prior to page rendering.
- Centralizing parameter definitions reduces boilerplate and ensures route query state aligns with application contracts.

## Consequences

Positive:
- Ensures consistent, type-safe coercion and validation of URL query parameters across server-rendered routes.
- Eliminates duplicate, error-prone manual string parsing logic in page entrypoints.
- Maintains declarative parameter contracts across server components.

Negative:
- Couples server-side parameter parsing to the specific lifecycle and schema contracts of the selected library.
- Requires synchronizing server parser definitions whenever client-side query parameter structures change.

## Alternatives

- Native URLSearchParams parsing in server components (rejected)
  Rejected because: Lacks static type safety, requires repetitive manual casting, and increases risk of unvalidated runtime input errors.
  When valid: Isolated script execution outside web application route handling where external dependencies are strictly prohibited.
- Custom ad-hoc schema validation per route handler (rejected)
  Rejected because: Introduces fragmented parameter validation contracts and prevents unified state synchronization across rendering boundaries.
  When valid: Scenarios requiring proprietary binary wire formats rather than standard URL query strings.

## Risks

- Breaking changes in library parser APIs across major version releases could invalidate route parameter parsing.
  Mitigation: Verify compatibility against the resolved lock artifact before updating dependencies or refactoring parser schemas.
  Owner: engineering team
- Malformed or unexpected query parameters passed by external clients could trigger unexpected rendering behavior if schemas are overly restrictive.
  Mitigation: Enforce fallback values and optional field typing across all search parameter schemas.
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
- Define parameter schemas in dedicated route configuration modules to allow consumption by both server components and client state hooks.
- Ensure all declared search parameters provide fallback defaults or nullable types to handle missing query values safely.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script to confirm that all server-side search parameter parsers comply with library type definitions.
- Discover and run the project test suite to verify that route parameter parsing and validation behaviors succeed.

Accept when:
- Server-side search parameter declarations type-check cleanly against the library parser definitions.
- The repository verification suites execute without query-parsing or type mismatches.

## Enforcement

- Verified by: Automated continuous integration type checking and static analysis checks.
- Verified by: Peer review during pull request evaluation.
- Violation handling: Pull requests containing unvalidated query parameter access or manual string parsing in server routes will fail automated review gates.
- Exception process: Exceptions require architectural review and documentation outlining why standard library server parsers cannot satisfy the route parameter requirements.