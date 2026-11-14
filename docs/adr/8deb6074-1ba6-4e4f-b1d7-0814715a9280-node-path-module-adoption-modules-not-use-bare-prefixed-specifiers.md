---
version: 2
name: 8deb6074-1ba6-4e4f-b1d7-0814715a9280-node-path-module-adoption-modules-not-use-bare-prefixed-specifiers
description: "node:path Module Adoption: Modules Not Use Bare Prefixed Specifiers"
metadata:
  actual:
    adr_id: 8deb6074-1ba6-4e4f-b1d7-0814715a9280
    status: accepted
    candidate_group_id: 6b837a6c-0fec-4652-a0fa-6eb8b6a5ab1e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - next.config.ts
      - src/app/.well-known/agent-skills/index.json/**
      - src/content/**
      - src/lib/**
---

# node:path Module Adoption: Modules Not Use Bare Prefixed Specifiers

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `next.config.ts`
- `src/app/.well-known/agent-skills/index.json/**`
- `src/content/**`
- `src/lib/**`

## Context

- Server-side content processing, route handling, image analysis, and configuration require deterministic filesystem path calculations.
- The codebase consistently utilizes explicit protocol-prefixed imports for core runtime modules across multiple architectural layers.
- Un-prefixed imports and ad-hoc path concatenation introduce resolution ambiguity and platform-specific path separator failures.

## Problem Statement

Un-prefixed module imports and manual path string manipulation introduce resolution ambiguity, potential dependency shadowing, and cross-platform path delimiter defects across server-side and build-time operations.

## Decision

1. MUST_NOT: Modules MUST NOT use bare un-prefixed specifiers or manual string concatenation to construct or manipulate filesystem paths.

## Policy Block

- MUST_NOT Modules MUST NOT use bare un-prefixed specifiers or manual string concatenation to construct or manipulate filesystem paths.

In scope:
- Server-side utilities performing content parsing and file resolution
- Route handlers and server endpoints requiring path manipulation
- Build configuration logic resolving local assets and packages
- Component rendering logic executing strictly within server runtime boundaries

Out of scope:
- Client-side interactive components rendered within browser environments
- Shared pure utility functions operating solely on in-memory structures without filesystem path semantics

## Rationale

- Evidence across seven files indicates uniform adoption of the protocol-prefixed node:path module for resolving and normalizing filesystem paths.
- Adopting the protocol prefix provides deterministic resolution to core runtime APIs and eliminates the risk of shadowing by third-party packages.
- Using standardized module methods avoids platform-specific path separator issues across distinct runtime environments.

## Consequences

Positive:
- Eliminates module resolution ambiguities and shields against package shadowing in the dependency hierarchy.
- Provides reliable cross-platform filesystem path resolution across varied deployment environments.
- Enforces an explicit and uniform import pattern across content processing, routing, and configuration.

Negative:
- Restricts importing modules to server-side and build-time runtime contexts without polyfills.
- Requires developers to maintain clear boundary segregation between server-only logic and client-facing code.

## Alternatives

- Using bare un-prefixed module specifiers for path resolution (rejected)
  Rejected because: Bare specifiers create module resolution ambiguity and risk shadowing by packages in the dependency tree
  When valid: Legacy runtime environments lacking support for protocol-prefixed module specifiers
- Manual string concatenation for filesystem path resolution (rejected)
  Rejected because: Manual string concatenation introduces path delimiter discrepancies across operating environments and causes fragile path resolution
  When valid: Generic uniform resource identifier path strings where abstract path objects are already in use

## Risks

- Accidental leakage of server-only path imports into client-side bundles resulting in build or runtime failure
  Mitigation: Establish static analysis rules to prevent core runtime module imports within client-side code bundles.
  Owner: engineering team
- Inconsistent path separator handling across differing host operating systems
  Mitigation: Mandate the use of join and resolve methods from the module instead of manual separator string formatting.
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
- Ensure that all server-side and build-time path operations import the module exclusively through the protocol prefix.
- Confine path resolution logic to server-side utilities, route handlers, and build configuration to prevent client bundle leakage.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify that all path module imports use the protocol-prefixed specifier.
- Execute the repository test suite to ensure that path resolution and content processing pass across execution environments.

Accept when:
- Static analysis confirms all path operations import the module using the protocol-prefixed specifier.
- Automated build and test suites execute without module resolution or runtime boundary errors.

## Enforcement

- Verified by: Continuous integration static analysis and import linting rules.
- Verified by: Automated test suite and build verification passes.
- Verified by: Pull request review processes.
- Violation handling: Pull requests containing bare un-prefixed module imports or ad-hoc path concatenations fail automated verification.
- Violation handling: Violations must be updated to the protocol-prefixed module specifier before merge approval.
- Exception process: Submit an architectural exception request detailing specific compatibility constraints that preclude protocol-prefixed specifiers.
- Exception process: Ensure any proposed exception is vetted for potential package name collision risks.