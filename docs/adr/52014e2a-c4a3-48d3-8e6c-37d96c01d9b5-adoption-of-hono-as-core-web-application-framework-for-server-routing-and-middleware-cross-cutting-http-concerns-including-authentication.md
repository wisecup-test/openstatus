---
version: 2
name: 52014e2a-c4a3-48d3-8e6c-37d96c01d9b5-adoption-of-hono-as-core-web-application-framework-for-server-routing-and-middleware-cross-cutting-http-concerns-including-authentication
description: "Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Cross Cutting Http Concerns Including Authentication"
metadata:
  actual:
    adr_id: 52014e2a-c4a3-48d3-8e6c-37d96c01d9b5
    status: accepted
    candidate_group_id: ab635fa0-3163-43ac-b634-a36e9733e3a2
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/**
      - src/routes/**
---

# Adoption of Hono as Core Web Application Framework for Server Routing and Middleware: Cross Cutting Http Concerns Including Authentication

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/libs/**`
- `src/routes/**`

## Context

- Server application modules require a unified, lightweight mechanism for HTTP routing, request parsing, and response dispatching.
- Endpoints across public status APIs, webhook handlers, and internal services demand consistent middleware execution for authentication, error handling, and performance tracing.
- Request lifecycle state such as identifiers, authentication metadata, and client headers must be safely propagated through handlers without relying on global mutable state.

## Problem Statement

Lacking a standardized web application framework leads to fragmented routing implementations, inconsistent request lifecycle handling, and divergent middleware execution models across server endpoints.

## Decision

1. MUST: Cross-cutting HTTP concerns including authentication, error formatting, cross-origin resource sharing, and performance metrics MUST be implemented as composable framework middleware functions.

## Policy Block

- MUST Cross-cutting HTTP concerns including authentication, error formatting, cross-origin resource sharing, and performance metrics MUST be implemented as composable framework middleware functions.

In scope:
- All HTTP route definitions, API endpoints, and webhook receivers within the server application.
- All request-scoped middleware responsible for security, observability, or request metadata decoration.

Out of scope:
- Standalone asynchronous workers and message queue consumers that do not process inbound HTTP requests.
- Isolated utility libraries that have no awareness of HTTP transport or routing concepts.

## Rationale

- Evidence across thirteen distinct server route and middleware units demonstrates pervasive adoption of Hono for routing and request context management.
- Standardizing on a unified routing and middleware abstraction eliminates disparate dispatching conventions across public and internal API surfaces.
- Native context storage primitives provide a standardized path for propagating correlation identifiers, authenticated tenant details, and tracing state across middleware chains.

## Consequences

Positive:
- Provides a consistent, lightweight HTTP request pipeline across all server endpoints and routes.
- Enables composable, typed middleware chaining for cross-cutting observability, authentication, and error handling.
- Maintains platform-agnostic portability across diverse server runtime environments without modifying business handlers.

Negative:
- Handlers and middleware become coupled to framework-specific context conventions and lifecycle abstractions.
- Upgrades across major framework releases require audit of context variable typing and middleware signature changes.

## Alternatives

- Adopting an alternative server framework with heavier transport bindings (rejected)
  Rejected because: Heavier runtime frameworks introduce substantial memory overhead and platform-specific primitives that hinder deployment portability across heterogeneous serverless and edge runtimes.
  When valid: Valid when enterprise infrastructure strictly standardizes on a specific monolithic container ecosystem requiring proprietary plugin architectures.
- Building custom HTTP request dispatching and middleware logic directly on platform primitives (rejected)
  Rejected because: In-house routing and middleware engines increase maintenance burden, risk security oversights in path parsing, and lack community-maintained middleware plugins.
  When valid: Valid only when runtime constraints preclude any third-party dependency overhead.

## Risks

- Tight coupling of route handler logic to framework context structures complicates future framework migration or transport layer changes.
  Mitigation: Maintain comprehensive endpoint contract test suites and isolate domain business logic into decoupled services independent of HTTP context.
  Owner: engineering team
- Misconfigured or missing context keys injected by upstream middleware can lead to runtime exceptions if context variables are improperly typed.
  Mitigation: Enforce strict shared context type interfaces across all middleware to catch missing or mistyped context keys at compile time.
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
- Define route handlers as modular sub-routers mounted to path prefixes to preserve clean domain boundary separation.
- Enforce strict TypeScript interface declarations for custom environment context variables across middleware boundaries.

## Continuation Context


Verify commands:
- Discover and execute the repository dependency audit script to verify all framework dependencies resolve to approved lock entries.
- Discover and execute the repository static analysis and type checking scripts to verify framework route and context typing compliance.
- Discover and execute the automated test runner to ensure all HTTP endpoint handlers and middleware pipelines pass validation.

Accept when:
- All server route handlers and middleware compile without type errors against the resolved framework context type contracts.
- Automated integration and unit test suites pass, verifying route dispatching and middleware chain execution.
- Static analysis confirms absence of unmanaged global request state in favor of framework context storage.

## Enforcement

- Verified by: Automated type checking and linting rules enforced during continuous integration pipelines.
- Verified by: Peer architecture review on pull requests introducing or modifying HTTP route definitions.
- Violation handling: Continuous integration builds fail upon detection of non-standard routing or direct transport socket manipulation.
- Violation handling: Pull requests are blocked until handlers are refactored into standard framework routes and middleware.
- Exception process: Submit an architectural RFC outlining runtime constraints that prevent framework adoption.
- Exception process: Obtain documented approval from lead backend architects prior to merging non-framework transport handlers.