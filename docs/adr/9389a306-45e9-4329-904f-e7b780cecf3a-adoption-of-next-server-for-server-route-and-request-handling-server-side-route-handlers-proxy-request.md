---
version: 2
name: 9389a306-45e9-4329-904f-e7b780cecf3a-adoption-of-next-server-for-server-route-and-request-handling-server-side-route-handlers-proxy-request
description: "Adoption of next/server for Server Route and Request Handling: Server Side Route Handlers Proxy Request"
metadata:
  actual:
    adr_id: 9389a306-45e9-4329-904f-e7b780cecf3a
    status: accepted
    candidate_group_id: 2dd81b33-c7f2-44a9-87ad-46fa4c700394
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of next/server for Server Route and Request Handling: Server Side Route Handlers Proxy Request

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/**`

## Context

- Server-side endpoints across API routes and proxy infrastructure require consistent handling of incoming HTTP headers, search parameters, and cookies.
- Multiple server entry points, including tRPC routers, single sign-on identity provider endpoints, webhook receivers, and proxy dispatchers, interface with client requests and upstream identity providers.
- The codebase standardizes on the next/server module across server route files to handle request resolution, cookie lifecycle operations, and response redirection.

## Problem Statement

Handling HTTP requests and responses across disparate server endpoints and proxy handlers without a standardized server abstraction leads to inconsistent header inspection, fragmented cookie management, and runtime boundary divergence between server and edge execution environments.

## Decision

1. MUST: All server-side route handlers and proxy request processors MUST use the next/server module to handle HTTP request inspection, cookie mutation, and response generation.

## Policy Block

- MUST All server-side route handlers and proxy request processors MUST use the next/server module to handle HTTP request inspection, cookie mutation, and response generation.

In scope:
- All server-side HTTP route handlers within the application.
- Edge and server proxy dispatchers processing incoming requests and responses.
- Webhook receivers and identity provider callback endpoints.

Out of scope:
- Client-rendered user interface components executing exclusively in browser environments.
- Standalone database schema definitions and migration scripts.
- Isolated utility libraries that have no transport-level HTTP request or response concerns.

Exceptions:
- EXC-20-001: A protocol adapter or external server framework requires native execution environments incompatible with the server module abstraction.

## Rationale

- Evidence across four server entry points confirms uniform adoption of next/server for header extraction, search parameter handling, and cookie manipulation.
- Using a unified server module across API routes, proxy dispatch, SSO handlers, and webhook receivers guarantees predictable cookie lifecycle and header propagation.
- Centralizing HTTP transport primitives through next/server prevents inconsistent request handling between edge execution contexts and server runtime environments.

## Consequences

Positive:
- Establishes a uniform API surface for cookie manipulation, header reading, and response redirection across all server endpoints.
- Ensures consistent behavior across both server and edge runtime execution environments.
- Maintains a clear boundary between HTTP transport extraction and domain service logic.

Negative:
- Couples server endpoints directly to the specific server module API surface and contracts.
- Upgrading framework dependencies requires auditing route handlers for changes to server request and response interfaces.

## Alternatives

- Native runtime HTTP server primitives (rejected)
  Rejected because: Native runtime HTTP abstractions lack cross-environment compatibility between edge and server environments and require manual parsing for cookies and query parameters.
  When valid: When executing within a custom standalone daemon architecture independent of the application framework.
- Framework-agnostic custom HTTP request abstraction layer (rejected)
  Rejected because: Introduces unnecessary abstraction overhead and maintenance burden when the application is already standardized on the framework runtime.
  When valid: When authoring multi-framework utility libraries designed to be published for external consumers.

## Risks

- Breaking API changes in future framework releases affecting response cookies or request header accessors.
  Mitigation: Verify exact locked versions and run repository test suites before upgrading framework dependencies.
  Owner: engineering team
- Leaking transport-specific request objects into domain service layers.
  Mitigation: Enforce architectural boundaries where transport extraction occurs in route handlers before passing parsed data to domain services.
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
- Extract request headers and query parameters at the handler boundary and map them to domain values before invoking internal services.
- Configure cookie security attributes, including path, expiration, and transport security flags, consistently using the server response cookie API.

## Continuation Context


Verify commands:
- Discover the project test runner from repository configuration and execute the server test suite.
- Discover the project type checker from repository configuration and verify type compliance across server route handlers.
- Discover the project linter from repository configuration and execute static analysis across server modules.

Accept when:
- All server route handlers and proxy modules compile without type errors using the server module interfaces.
- HTTP endpoint test suites pass successfully, verifying header extraction and cookie lifecycle management.
- Static analysis confirms no unauthorized direct transport mutations or unsupported runtime socket access in server route handlers.

## Enforcement

- Verified by: Continuous integration automated testing and type verification pipelines.
- Verified by: Peer review during code review for all new route handlers and proxy modifications.
- Violation handling: Automated build failures in pull requests that introduce raw transport mutations or bypass server module interfaces.
- Violation handling: Mandatory code rework during peer review prior to merging.
- Exception process: Submit an exception request to the architecture review team detailing why the server module abstraction cannot satisfy the protocol requirements.
- Exception process: Obtain approval and document the divergence and isolation boundary within the endpoint documentation.