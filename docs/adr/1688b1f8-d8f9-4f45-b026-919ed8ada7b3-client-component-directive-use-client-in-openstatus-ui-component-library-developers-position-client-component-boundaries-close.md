---
version: 2
name: 1688b1f8-d8f9-4f45-b026-919ed8ada7b3-client-component-directive-use-client-in-openstatus-ui-component-library-developers-position-client-component-boundaries-close
description: "Client Component Directive (\"use client\") in @openstatus/ui Component Library: Developers Position Client Component Boundaries Close"
metadata:
  actual:
    adr_id: 1688b1f8-d8f9-4f45-b026-919ed8ada7b3
    status: accepted
    candidate_group_id: 4802f371-80d7-4d9d-91c4-2dfa24bf57b8
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
---

# Client Component Directive ("use client") in @openstatus/ui Component Library: Developers Position Client Component Boundaries Close

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/**`
- `src/hooks/**`

## Context

- The shared UI component library provides reusable visual primitives and composite blocks consumed across application frontends.
- Interactive components require client-side runtime capabilities, including React state management hooks, lifecycle side effects, and DOM measurement APIs.
- Headless UI primitives and visualization components depend on client-side browser runtime environments and event listeners to manage focus, overlays, and animations.
- In hybrid server-driven rendering architectures, modules missing client boundary directives cause compilation or runtime errors when rendered within server component contexts.

## Problem Statement

Shared UI component libraries containing interactive primitives and composite blocks fail when rendered inside server component contexts unless client boundaries are explicitly demarcated. Without a standardized rendering boundary policy, developers face runtime execution errors, broken component isolation, and unintended leakage of client-side dependencies into server rendering phases.

## Decision

1. SHOULD: Developers SHOULD position client component boundaries as close to the leaves of the component render tree as possible to maximize server-side rendering opportunities for non-interactive parent structures.

## Policy Block

- SHOULD Developers SHOULD position client component boundaries as close to the leaves of the component render tree as possible to maximize server-side rendering opportunities for non-interactive parent structures.

In scope:
- Interactive UI primitives and composite blocks authored within the shared UI component package.
- Components utilizing client hooks for state, effects, or memoization.
- Components wrapping client-only headless UI, chart, or overlay libraries.

Out of scope:
- Static presentation components that require no browser runtime APIs or React hooks.
- Utility functions and data formatters that operate purely on inputs without DOM interaction.

## Rationale

- Static analysis of 52 component files in the shared UI package demonstrates a consistent pattern of marking interactive primitives and composite blocks with the client component boundary directive.
- Declaring the client directive directly at the component library wrapper level ensures consumers can safely import and render interactive components within server-rendered application trees without needing consumer-side boundary wrappers.
- Demarcating the boundary at the module level explicitly separates interactive browser logic from server rendering logic, preserving predictable component composition across the monorepo.

## Consequences

Positive:
- Ensures seamless interoperability of shared UI library components across both server and client rendering contexts.
- Encapsulates client-specific dependencies such as headless UI primitives, charts, and notification overlays behind well-defined module boundaries.
- Prevents build-time and runtime execution failures caused by invoking browser APIs or client hooks during server rendering.

Negative:
- Increases client bundle payload by including client component JavaScript for all modules marked with the client directive.
- Restricts props passed from server components to client component wrappers to serializable values only.

## Alternatives

- Require consuming applications to wrap third-party and shared UI primitives in local client boundary components (rejected)
  Rejected because: Shifts client boundary management burden onto every consuming application, leading to repetitive boilerplate code and frequent boundary omission bugs
  When valid: When the shared UI component library must remain completely agnostic of server component rendering architectures
- Mark the entire shared UI component package as a client-only bundle (rejected)
  Rejected because: Eliminates the possibility of server rendering pure presentational components from the shared package, unnecessarily bloating client JavaScript bundles
  When valid: When a library consists exclusively of client-only interactive widgets with no static or presentational primitives

## Risks

- Developers may prematurely mark non-interactive presentational components with the client directive, degrading initial page load performance.
  Mitigation: Conduct code reviews and linting checks to ensure client directives are only added when client hooks or browser APIs are genuinely required.
  Owner: Frontend Engineering Team
- Passing non-serializable props across the client boundary will trigger runtime serialization errors.
  Mitigation: Enforce contract boundaries that pass primitive or serialized data structures to client components.
  Owner: Frontend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Place the client component directive as the very first statement in the module, preceding all import statements and comments.
- When composing composite blocks from multiple UI primitives, ensure that any child component requiring DOM event handlers or state hooks is enclosed within a client boundary.

## Continuation Context


Verify commands:
- # Discover and run repository lint and check scripts from project manifests
MANIFEST_SCRIPTS=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"lint"' {} + 2>/dev/null | head -n 1)
echo "Discovered lint configuration in: $MANIFEST_SCRIPTS"
- # Discover component files importing interactive hooks and verify client boundary declarations
find . -type f -name '*.*' -exec grep -lE 'useState|useEffect|useMemo' {} + | while read -r comp; do head -n 5 "$comp" | grep -q '"use client"' || echo "Verify client boundary in: $comp"; done
- # Discover and run repository build validation scripts to confirm rendering model integrity
MANIFEST_BUILD=$(find . -maxdepth 2 -name '*.*' -exec grep -l '"build"' {} + 2>/dev/null | head -n 1)
echo "Discovered build configuration in: $MANIFEST_BUILD"

Accept when:
- All UI component modules utilizing client-side hooks or browser event listeners declare the client boundary directive as their initial statement.
- Repository build and lint verification scripts complete with zero exit code and no client-server boundary violation errors.
- Component rendering in both server-side and client-side execution environments completes without boundary mismatch warnings.

## Enforcement

- Verified by: Automated lint rules scanning component files for client hook usage without the client boundary directive.
- Verified by: Pull request code reviews by frontend engineering peers.
- Verified by: Continuous integration build validation testing server component compilation and bundle separation.
- Violation handling: Pull requests containing interactive components without the required client directive will fail automated checks and be blocked from merging.
- Violation handling: Developers must add the client directive or refactor client logic into dedicated client leaf components.
- Exception process: Exceptions for non-interactive pure functions or components operating in dedicated server-only packages require approval from a principal frontend engineer and documented rationale in the pull request.