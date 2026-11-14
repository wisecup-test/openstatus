---
version: 2
name: 9cf51333-f071-4d49-b752-4027305aff01-react-client-hook-interaction-pattern-interactive-components-custom-interaction-hooks-declare
description: "React Client Hook Interaction Pattern: Interactive Components Custom Interaction Hooks Declare"
metadata:
  actual:
    adr_id: 9cf51333-f071-4d49-b752-4027305aff01
    status: accepted
    candidate_group_id: b1157a91-94b5-4a82-91f1-2f5f2caddd83
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
      - src/lib/**
---

# React Client Hook Interaction Pattern: Interactive Components Custom Interaction Hooks Declare

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/**`
- `src/hooks/**`
- `src/lib/**`

## Context

- Interactive UI components and custom interaction hooks require client-side execution contexts to attach DOM event listeners, evaluate viewport dimensions, access browser storage, and manage component lifecycles.
- Across the UI package, multiple interactive blocks, primitives, and custom hooks consistently adopt the client component rendering directive alongside React state and effect primitives.
- Standardizing interaction state patterns ensures uniform behavior, predictable lifecycle cleanup, and consistent separation between rendering and client-side side effects.

## Problem Statement

Interactive UI components and hooks frequently need to interact with browser runtime APIs, handle complex user gestures, manage transient component state, and perform debounced or responsive operations. Without a uniform interaction architecture, components risk inconsistent state lifecycles, uncleaned event listeners leading to memory leaks, hydration mismatches between server and client environments, and tight coupling between presentation markup and browser side effects.

## Decision

1. MUST: Interactive UI components and custom interaction hooks MUST declare the client component rendering directive and manage local state, effects, and memoized values through standard React hook primitives.

## Policy Block

- MUST Interactive UI components and custom interaction hooks MUST declare the client component rendering directive and manage local state, effects, and memoized values through standard React hook primitives.

In scope:
- All interactive UI components, interactive blocks, and custom interaction hooks within the frontend presentation package.
- Modules requiring DOM event subscriptions, browser runtime access, animated transitions, or responsive layout tracking.

Out of scope:
- Stateless presentation components that render purely from passed properties without local interaction state.
- Server-rendered layout shells and static markup blocks with no client interactivity.
- Non-visual data access utilities, internal service clients, and domain type definitions.

## Rationale

- Evidence across sixteen modules demonstrates consistent reliance on React hook primitives to handle interactive state, timers, and browser APIs under explicit client boundary directives.
- Encapsulating client interactions within reusable custom hooks isolates complex state transitions away from visual component structures, improving testability and code reuse.
- Standardizing callback memoization and computed value caching preserves predictable rendering performance across interactive component subtrees.

## Consequences

Positive:
- Encapsulates volatile browser APIs and interaction lifecycles into modular, testable, and reusable custom hooks.
- Stabilizes child component re-renders through explicit callback memoization and computation caching primitives.
- Maintains clean architectural boundaries between server rendering logic and interactive browser runtime capabilities.

Negative:
- Developers must rigorously maintain hook dependency lists to prevent stale closures or unintended effect invocations.
- Client component boundaries increase client-side JavaScript bundle weight compared to purely static presentation components.
- Asynchronous and browser-dependent interactions require defensive null-checking and post-mount gating to avoid hydration discrepancies.

## Alternatives

- Direct inline browser API invocations and ad-hoc DOM event subscriptions within presentation components (rejected)
  Rejected because: Couples visual presentation to runtime environment side effects, duplicates event listener cleanup logic across components, and increases risk of memory leaks.
  When valid: Disposable single-file prototypes where long-term maintainability, testability, and reuse are not required.
- Centralized external global state management store for all component-level interactions and transient UI states (rejected)
  Rejected because: Introduces excessive boilerplate and global coupling for localized transient interaction states that are naturally scoped to component subtrees.
  When valid: Interactive application states that must persist across page navigations or synchronize across disparate route boundaries.

## Risks

- Incomplete hook dependency declarations causing stale closures or unintended execution loops during state updates.
  Mitigation: Enforce automated linter rules for exhaustive hook dependencies and require unit tests validating effect triggers and teardown.
  Owner: Frontend Engineering Team
- Hydration mismatch errors when browser-specific properties are evaluated during initial server rendering.
  Mitigation: Isolate client-only state evaluations to post-mount effect hooks or client boundary wrappers to ensure initial server and client render parity.
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
- Encapsulate distinct browser capabilities into dedicated custom hooks rather than co-locating unrelated effects in a single component.
- Ensure all event subscriptions and timers established inside effect hooks return explicit cleanup functions.
- Gate browser-dependent state initializations to avoid hydration mismatches between server output and client mounting.

## Continuation Context


Verify commands:
- Discover and run the project test runner to execute unit and interaction test suites across interactive components and custom hooks.
- Discover and execute the project linter to verify compliance with React hook dependency rules and client boundary directives.
- Discover and run the project type checker to validate component properties, hook return types, and event handler contracts.

Accept when:
- All interactive UI components and custom interaction hooks pass unit and interaction test suites with clean lifecycle cleanup and zero memory leak warnings.
- Linter verification confirms compliance with React hook dependency rules and client boundary directives across all UI modules.
- Project type verification passes with zero type errors for all component properties, hook return contracts, and event callback signatures.

## Enforcement

- Verified by: Automated pull request checks enforcing linting rules for React hook conventions and client directive usage.
- Verified by: Automated test execution verifying hook lifecycle behaviors, cleanup execution, and component rendering states.
- Verified by: Peer code review by frontend team members for interaction semantics and boundary definitions.
- Violation handling: Pull requests containing hook dependency omissions or unencapsulated browser API access are blocked from merging.
- Violation handling: Violations detected during code review must be refactored into compliant custom hooks before acceptance.
- Exception process: Submit an architectural exception request detailing the specific technical limitation preventing standard hook-based interaction patterns.
- Exception process: Obtain formal sign-off from frontend architecture leads prior to merging code that bypasses client boundary or hook lifecycle standards.
- Exception process: Document the temporary exception and track resolution in the team issue tracker with an agreed sunset milestone.