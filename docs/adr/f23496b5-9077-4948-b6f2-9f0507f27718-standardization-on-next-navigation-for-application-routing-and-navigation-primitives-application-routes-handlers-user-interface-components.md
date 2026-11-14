---
version: 2
name: f23496b5-9077-4948-b6f2-9f0507f27718-standardization-on-next-navigation-for-application-routing-and-navigation-primitives-application-routes-handlers-user-interface-components
description: "Standardization on next/navigation for Application Routing and Navigation Primitives: Application Routes Handlers User Interface Components"
metadata:
  actual:
    adr_id: f23496b5-9077-4948-b6f2-9f0507f27718
    status: accepted
    candidate_group_id: b5794d4a-1fde-45ec-8132-55f07184628d
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
---

# Standardization on next/navigation for Application Routing and Navigation Primitives: Application Routes Handlers User Interface Components

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/content/**`

## Context

- The web application requires consistent client and server routing, URL parameter consumption, and programmatic redirects across page endpoints and reusable navigation components.
- Legacy routing packages from earlier framework paradigms do not support server-side rendering boundaries or asynchronous streaming component trees.
- Direct dependency analysis reveals widespread adoption of the navigation module across static redirects, dynamic content handlers, route endpoints, and interactive search interface components.

## Problem Statement

Without a standardized routing and navigation module, application components risk mixing legacy router packages with server-side primitives, leading to runtime routing errors, broken server transitions, and inconsistent URL parameter management across rendering boundaries.

## Decision

1. MUST: All application routes, handlers, and user interface components requiring navigation control, programmatic redirection, or URL state inspection MUST use the navigation primitives provided by next/navigation.

## Policy Block

- MUST All application routes, handlers, and user interface components requiring navigation control, programmatic redirection, or URL state inspection MUST use the navigation primitives provided by next/navigation.

In scope:
- All application route definitions, redirect pages, dynamic slug resolvers, API route handlers, and navigation UI components.

Out of scope:
- Third-party external redirect targets that hand off execution entirely outside the domain boundaries of the application.

Exceptions:
- EXC-20-001: An external identity provider callback requires an unconditional low-level HTTP response header redirect not managed by framework primitives.

## Rationale

- Standardizing on next/navigation unifies control flow across both server components and client components within the application.
- The pattern is evidenced by consistent usage across twenty-six files encompassing redirects, dynamic content pages, API callback routes, and search sub-navigation elements.
- Adopting the dedicated navigation module prevents fragmentation between legacy page-based routing paradigms and modern component architectures.

## Consequences

Positive:
- Provides cohesive navigation semantics across server rendering, client navigation, and API route handling.
- Reduces transition latency and avoids full document reloads through framework-managed client navigation.
- Ensures search parameter parsing and route state access respect server and client boundary constraints.

Negative:
- Tightly couples route components to the specific navigation module semantics and lifecycle.
- Requires developers to manage client boundary declarations when consuming navigation hooks.

## Alternatives

- Legacy router package imports (rejected)
  Rejected because: Legacy routing primitives fail in server-rendered component environments and do not support streaming navigation lifecycle.
  When valid: Only valid in legacy codebases maintaining pre-modern framework architectures.
- Direct browser location object manipulation (rejected)
  Rejected because: Imperative browser location changes force complete page reloads, destroying client-side state and bypassing framework routing cache.
  When valid: Valid only when navigating completely away from the host application domain.

## Risks

- Calling client-only navigation hooks within server component contexts causing build or runtime failures
  Mitigation: Enforce explicit client boundary declarations and lint rules governing hook usage in component files.
  Owner: engineering team
- Breaking behavioral changes in navigation primitives across major framework updates
  Mitigation: Strict adherence to lock-version grounding before updating dependencies and executing comprehensive route verification.
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
- Verify that client components consuming search parameters are wrapped within appropriate fallback boundaries to prevent rendering suspensions during dynamic tree evaluation.
- Centralize navigation helper utilities to abstract repetitive redirect patterns across static landing pages and callback routes.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and linting scripts from the repository configuration and execute them across the application source tree.
- Discover the project type verification script and run it to ensure navigation hook and parameter types adhere to upstream contracts.
- Discover and run the project automated test suite to validate route resolution and redirect behaviors across all endpoints.

Accept when:
- Static analysis and type checking complete without import violations or navigation hook syntax errors.
- All redirect routes, dynamic slug pages, and API callbacks return expected navigation actions in automated test runs.
- No legacy routing packages are referenced in active route and component files.

## Enforcement

- Verified by: Continuous integration static analysis rules checking import declarations
- Verified by: Automated end-to-end and unit test suites covering route responses
- Verified by: Peer code review verification against navigation architectural standards
- Violation handling: Pull requests containing disallowed router imports or direct window location navigation will fail automated validation checks.
- Violation handling: Non-compliant navigation implementations must be refactored prior to merge approval.
- Exception process: Submit an architectural deviation request detailing why framework navigation primitives cannot satisfy the routing requirement.
- Exception process: Obtain explicit sign-off from the architecture team prior to merging non-standard routing logic.