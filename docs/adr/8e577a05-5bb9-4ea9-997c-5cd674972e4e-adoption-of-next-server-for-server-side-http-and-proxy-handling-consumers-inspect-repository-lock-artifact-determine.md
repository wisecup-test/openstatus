---
version: 2
name: 8e577a05-5bb9-4ea9-997c-5cd674972e4e-adoption-of-next-server-for-server-side-http-and-proxy-handling-consumers-inspect-repository-lock-artifact-determine
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 8e577a05-5bb9-4ea9-997c-5cd674972e4e
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Consumers Inspect Repository Lock Artifact Determine

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/**`

## Context

- The application exposes multiple public and private server entry points, including badge generators, markdown rendering endpoints, TRPC handlers, and dynamic status APIs.
- A shared server proxy and domain resolution mechanism orchestrates routing, host forwarding header inspection, and request filtering before downstream execution.
- Handling diverse HTTP workflows across route handlers requires consistent request parsing, header extraction, cookie management, and structured response construction.
- The application standardizes on next/server as the unified server abstraction boundary to manage HTTP interactions across both API handlers and middleware proxies.

## Problem Statement

Without a unified server abstraction layer across route handlers and proxy modules, disparate endpoints risk implementing conflicting request parsing patterns, inconsistent header forwarding, divergent cookie handling, and incompatible response structures across server runtime environments.

## Decision

1. MUST: Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the framework dependency before implementing or consuming APIs from next/server.

## Policy Block

- MUST Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the framework dependency before implementing or consuming APIs from next/server.

In scope:
- All server-side route handlers, API endpoints, proxy handlers, and domain resolution utilities within the status page application.

Out of scope:
- Client-side component rendering and browser-only execution contexts.

## Rationale

- Evidence across eight server modules confirms consistent adoption of next/server for reading incoming headers, query parameters, cookies, and generating HTTP responses.
- Standardizing on next/server encapsulates HTTP lifecycle mechanics within framework-provided server primitives, preventing vendor divergence between route handlers and edge proxy logic.
- Centralizing request and response representations ensures predictable propagation of tracing headers, cache directives, and custom proxy routing tags.

## Consequences

Positive:
- Unified request and response programming model across all server-side endpoints and proxy layers.
- Eliminates redundant low-level HTTP parsing logic and ensures consistent header and cookie manipulation.
- Seamless integration between domain routing utilities, proxy middleware, and downstream route handlers.

Negative:
- Tightly couples server-side routing and middleware logic to the next/server runtime API contract.
- Requires migration effort if underlying framework server primitives change across major platform releases.

## Alternatives

- Native web Fetch API Request and Response objects without next/server extensions (rejected)
  Rejected because: Native fetch primitives lack framework-integrated helpers for cookie serialization, URL rewrite and redirect handling, and proxy header forwarding.
  When valid: When running inside runtime-agnostic microservices decoupled from the framework server environment.
- Custom in-house HTTP abstraction layer wrapping server requests and responses (rejected)
  Rejected because: Creating proprietary server wrappers introduces maintenance overhead and duplicates capabilities already provided natively by the framework module.
  When valid: When targeting multi-cloud environments requiring cross-framework portability.

## Risks

- Breaking API changes in next/server during major framework upgrades could disrupt route handlers and proxy routing.
  Mitigation: Pin resolved framework versions in the lock artifact and validate server entry points against locked version documentation before upgrading.
  Owner: engineering team
- Leaking server-only next/server primitives into shared or client-rendered code paths causing compilation failures.
  Mitigation: Enforce module boundary checks and automated type verification across all application entry points.
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
- Ensure route handlers import server primitives exclusively from next/server and adhere to supported handler function signatures for HTTP methods.
- Utilize next/server response helpers for status code assignment, header mutation, and cookie writing to ensure downstream proxy middleware compatibility.

## Continuation Context


Verify commands:
- Discover the repository dependency manifest and run the declared type checking script to verify next/server import compliance.
- Discover and execute the repository test runner script targeting route handlers and proxy modules to validate server response contracts.
- Discover and run the project linting script to ensure no unauthorized HTTP response primitives are imported in server-side entry points.

Accept when:
- All server route handlers and proxy modules compile successfully using next/server types without type errors.
- Automated test suites for route handlers, badges, and proxy redirection pass with zero regressions.
- No deprecated or unsupported server-side response methods are invoked across server endpoints.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type verification and test suites.
- Verified by: Peer code review verifying server endpoints utilize next/server primitives rather than ad-hoc abstractions.
- Violation handling: Pull requests introducing divergent server request or response implementations or unapproved imports will fail continuous integration and be blocked from merge.
- Exception process: Exceptions require submission of an architectural change request detailing runtime constraints, approved by technical leads.