---
version: 2
name: bd098d54-ef36-4826-adf0-3d09b86fdbdf-internal-authentication-module-lib-auth-adoption-application-boundaries-server-endpoints-execution-contexts
description: "Internal Authentication Module (@/lib/auth) Adoption: Application Boundaries Server Endpoints Execution Contexts"
metadata:
  actual:
    adr_id: bd098d54-ef36-4826-adf0-3d09b86fdbdf
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Application Boundaries Server Endpoints Execution Contexts

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/**`

## Context

- Multiple endpoints across API handlers, server actions, proxy layers, and context resolvers require consistent identity verification and session access.
- Direct dependency on external identity providers across heterogeneous application entry points introduces fragmented session parsing and inconsistent security enforcement.
- A path-aliased internal authentication module consolidates session extraction, credential inspection, and single sign-on flows into a uniform internal interface.

## Problem Statement

Decentralized authentication logic across distinct application layers leads to inconsistent security validation, duplicated session decoding logic, and direct coupling to third-party provider APIs. Without a unified internal interface, modifications to identity providers or session management strategies require coordinated alterations across multiple route handlers, edge proxy layers, and context builders.

## Decision

1. MUST: Application boundaries, server endpoints, and execution contexts requiring identity verification or session resolution MUST consume authentication primitives exclusively through the internal authentication module.

## Policy Block

- MUST Application boundaries, server endpoints, and execution contexts requiring identity verification or session resolution MUST consume authentication primitives exclusively through the internal authentication module.

In scope:
- All application route handlers, server actions, background task contexts, and edge proxy utilities requiring identity resolution or session access.
- Components and layouts gating user access based on authenticated identity state.

Out of scope:
- Publicly accessible static assets and unauthenticated public webhook routes that do not require identity evaluation.
- Low-level database schemas that define structural data types without evaluating request identity.

Exceptions:
- EXC-20-001: A third-party protocol callback requires specialized protocol-level parameter verification before session establishment.

## Rationale

- Centralizing authentication primitives through a single module boundary guarantees uniform session validation across diverse runtime contexts, including API handlers, proxy services, and UI layouts.
- Encapsulating third-party authentication libraries behind an internal interface isolates the application from upstream provider breaking changes and simplifies single sign-on handling.
- Co-locating session resolution logic reduces security vulnerabilities stemming from divergent cookie parsing or disparate identity extraction implementations.

## Consequences

Positive:
- Establishes a single source of truth for session and identity extraction across all application entry points.
- Reduces code duplication across API route handlers, server actions, and proxy routines.
- Isolates upstream identity provider updates and single sign-on flows behind a stable internal interface.

Negative:
- Creates a central dependency point whose failure or interface modification directly impacts multiple application layers.
- Requires developers to route all identity handling through the internal module rather than consuming third-party provider helpers directly.

## Alternatives

- Directly import and invoke third-party authentication client libraries within each route handler and server action (rejected)
  Rejected because: Leads to fragmented authentication logic, redundant credential validation code, and tight coupling to vendor-specific APIs across the application surface.
  When valid: In small single-purpose applications with only one route handler requiring authentication.
- Decouple authentication completely into an independent microservice proxying all application requests (rejected)
  Rejected because: Adds operational complexity, network overhead, and latency for an application architecture that shares a consolidated runtime.
  When valid: When scaling across heterogeneous language runtimes that cannot share an internal code module.

## Risks

- Interface instability in the internal authentication module could propagate breaking changes to all dependent route handlers and context builders.
  Mitigation: Enforce strict interface typing, comprehensive automated test coverage, and semantic interface versioning for shared exports.
  Owner: Architecture and Core Infrastructure Team
- Accidental bypass of the central module through ad-hoc cookie or header inspection in newly authored routes.
  Mitigation: Employ static analysis rules and peer code review gates to flag direct session inspection outside the authorized module.
  Owner: Security and Platform Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Ensure exported module functions maintain pure interfaces where possible, abstracting execution context differences between edge runtimes, server action handlers, and background execution workers.
- Implement uniform error handling within authentication exports to return typed failure results rather than leaking raw identity provider exceptions.

## Continuation Context


Verify commands:
- Discover the workspace static analysis script from repository configuration and execute it across all route and service boundaries.
- Locate the repository test runner from project configuration and run the test suite covering authentication utilities and dependent route handlers.
- Query repository imports to verify all session and identity operations originate from the designated internal authentication module.

Accept when:
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

## Enforcement

- Verified by: Automated continuous integration pipelines running repository linters and test suites.
- Verified by: Mandatory architectural and security peer review for pull requests modifying authentication interfaces or introducing new route handlers.
- Violation handling: Pull requests containing direct external authentication imports or unapproved bypasses will be blocked from merging.
- Violation handling: Violations identified during reviews must be refactored to consume the central module prior to approval.
- Exception process: Submit an architectural exception request detailing technical necessity and security mitigation to the platform engineering leads.
- Exception process: Approved exceptions must be tracked in repository documentation with a planned convergence timeline.