---
version: 2
name: c46d271e-29a1-4a5b-83a8-a8db6c8a358a-internal-shared-module-adoption-for-isomorphic-client-server-boundaries-server-tier-entrypoints-not-reference-browser
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Server Tier Entrypoints Not Reference Browser"
metadata:
  actual:
    adr_id: c46d271e-29a1-4a5b-83a8-a8db6c8a358a
    status: accepted
    candidate_group_id: a89df71c-b426-4d94-a699-ab4046839363
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/preferred-settings/**
      - src/trpc/**
---

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Server Tier Entrypoints Not Reference Browser

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/lib/preferred-settings/**`
- `src/trpc/**`

## Context

- Full-stack web applications execute logic across distinct execution environments that possess incompatible runtime capabilities and global objects.
- Server-side runtime operations require access to incoming request headers and server session contexts that are unavailable and invalid within browser environments.
- Client-side runtime operations interact with browser storage mechanisms and reactive UI bindings that cannot be evaluated during server execution without throwing reference errors.
- Directly importing server-specific utilities into client execution paths risks leaking sensitive server logic and dramatically increasing client bundle weight.
- Subsystems require shared data contracts, validation schemas, and common serialization primitives without coupling client and server execution contexts together.

## Problem Statement

Coupling environment-specific logic within shared application modules causes server-side dependencies to leak into client bundles and triggers runtime exceptions when browser-specific APIs execute on the server. Subsystems that require isomorphic contracts across client and server tiers require an architectural pattern that guarantees runtime isolation while maintaining a single source of truth for shared schemas and types.

## Decision

1. MUST_NOT: Server-tier entrypoints MUST NOT reference browser document models, client window globals, or reactive view lifecycle hooks.

## Policy Block

- MUST_NOT Server-tier entrypoints MUST NOT reference browser document models, client window globals, or reactive view lifecycle hooks.

In scope:
- Subsystems and library modules in the web application that provide functionality to both browser and server runtime contexts.
- Codebases requiring shared data schemas, type contracts, and protocol constants across isomorphic network boundaries.

Out of scope:
- Modules that execute exclusively within server-side background processes or build-time scripts.
- Pure client-side UI view components that have no corresponding server-side data fetching or processing layer.

Exceptions:
- EXC-20-001: A third-party library natively provides automated tree-shaking and runtime environment detection without introducing bundle leakage.

## Rationale

- Static analysis confirms that feature subsystems enforce clean environmental separation by routing client logic through browser APIs and server logic through server header stores while sharing `./shared` contracts.
- Unidirectional dependencies from runtime entrypoints into `./shared` prevent circular dependencies and prevent server-only code from contaminating client bundles.
- Centralizing validation schemas and type definitions in `./shared` ensures compile-time and runtime contract parity across network and execution boundaries.

## Consequences

Positive:
- Eliminates accidental inclusion of server-exclusive modules and credentials in client bundles.
- Prevents runtime execution errors caused by accessing environment-specific globals in incompatible runtimes.
- Establishes a single source of truth for validation schemas and types across client and server execution tiers.

Negative:
- Increases module count by requiring tripartite entrypoint division for isomorphic features.
- Requires developers to maintain disciplined import boundaries and prevent cross-entrypoint relative imports.

## Alternatives

- Single unified module with dynamic runtime environment checking (rejected)
  Rejected because: Bundlers cannot reliably eliminate server-only dependencies when grouped in a unified module, leading to bundle bloat and leakage of server abstractions into browser artifacts.
  When valid: Valid only in small micro-utilities where no environment-specific dependencies or globals are imported.
- Separate standalone workspace packages for client and server logic (rejected)
  Rejected because: Introduces excessive package management and build tooling overhead for internal feature-level utilities that belong to the same domain.
  When valid: Valid when distributing shared capabilities across multiple distinct microservices or external client applications.

## Risks

- Developers might inadvertently import server entrypoints from client files or vice versa, bypassing the `./shared` boundary.
  Mitigation: Enforce module boundary lint rules and automated static dependency analysis in continuous integration.
  Owner: Architecture Team
- Accidental inclusion of environment-specific imports within the `./shared` module itself.
  Mitigation: Automate bundle isolation testing and static analysis to reject environment-specific dependencies inside shared modules.
  Owner: Platform Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Structure isomorphic features into dedicated client and server runtime entrypoints accompanied by a common `./shared` module for cross-tier contracts.
- Verify that all data validation schemas declared in the shared module are decoupled from platform-specific input and output drivers.
- Ensure that transport header alterations and session store accesses remain strictly confined to the server entrypoint.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute module boundary and type checking.
- Discover the repository test runner script and execute integration tests verifying client and server entrypoint isolation.
- Discover the build analysis script and execute production bundle evaluation to ensure no server-side imports appear in client artifacts.

Accept when:
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

## Enforcement

- Verified by: Automated static analysis and module boundary checks executed during continuous integration workflows.
- Verified by: Pull request architectural review inspecting new isomorphic modules for strict adherence to tripartite entrypoint structure.
- Violation handling: Continuous integration builds fail upon detection of unauthorized cross-tier imports or environment leaks into `./shared`.
- Violation handling: Pull requests violating runtime isolation are blocked until module structure conforms to the tripartite convention.
- Exception process: Exceptions must be submitted via formal architectural review with justification for alternative bundling or tree-shaking guarantees.