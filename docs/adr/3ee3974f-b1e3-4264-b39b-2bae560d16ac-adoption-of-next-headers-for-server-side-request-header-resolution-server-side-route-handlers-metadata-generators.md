---
version: 2
name: 3ee3974f-b1e3-4264-b39b-2bae560d16ac-adoption-of-next-headers-for-server-side-request-header-resolution-server-side-route-handlers-metadata-generators
description: "Adoption of next/headers for Server-Side Request Header Resolution: Server Side Route Handlers Metadata Generators"
metadata:
  actual:
    adr_id: 3ee3974f-b1e3-4264-b39b-2bae560d16ac
    status: accepted
    candidate_group_id: 0a7b592f-ca4b-43d4-a530-9faeef433969
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of next/headers for Server-Side Request Header Resolution: Server Side Route Handlers Metadata Generators

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/lib/auth/**`

## Context

- Server-side endpoints and metadata handlers must dynamically determine the incoming request host, protocol, and client headers to serve multi-tenant status pages.
- Static configuration is insufficient when requests arrive through proxy layers and custom domains requiring forwarded header inspection.
- The codebase requires a standardized server-side API to retrieve incoming HTTP headers across route handlers, metadata routes, and authentication utilities.

## Problem Statement

Dynamic status page applications serving custom domains and reverse-proxied traffic must resolve hostnames, protocols, and request-specific headers directly at the server runtime boundary. Hardcoding domain configurations or attempting to read request headers through non-standard server interfaces creates fragmented domain resolution, breaks multi-tenant support, and risks runtime errors across server endpoints and metadata generators.

## Decision

1. MUST: Server-side route handlers, metadata generators, and authentication utilities requiring incoming HTTP header information MUST retrieve request headers using the next/headers module.

## Policy Block

- MUST Server-side route handlers, metadata generators, and authentication utilities requiring incoming HTTP header information MUST retrieve request headers using the next/headers module.

In scope:
- Server-side API route handlers requiring dynamic request metadata or client-forwarded headers.
- Metadata generation endpoints producing site navigation artifacts based on dynamic request hostnames.
- Server-side authentication helper modules resolving request domain and protocol boundaries.

Out of scope:
- Client-rendered components and browser-executed modules.
- Static assets and build-time static pages evaluated without an incoming request context.

## Rationale

- Adopting next/headers provides a unified and supported mechanism for reading incoming request headers across server route handlers, metadata files, and authentication libraries.
- Inspecting forwarded host and protocol headers through next/headers enables reliable multi-tenant status page rendering and dynamic sitemap generation.
- Centralizing header access around standard server framework exports avoids reliance on bespoke server adapters or brittle global request state.

## Consequences

Positive:
- Standardizes header retrieval across API routes, metadata endpoints, and server authentication helpers.
- Enables accurate dynamic domain and protocol resolution for multi-tenant and reverse-proxied traffic.
- Maintains compatibility with server-rendered routes and metadata generation pipelines.

Negative:
- Accessing dynamic request headers opts routes into dynamic server rendering, bypassing static build caching.
- Requires consistent error handling when optional proxy headers are absent from incoming requests.

## Alternatives

- Hardcoded Environment Configuration for Domain Resolution (rejected)
  Rejected because: Static environment variables cannot accommodate dynamic multi-tenant domain routing or client-forwarded host resolution across diverse deployment environments.
  When valid: Single-tenant deployments where all incoming traffic strictly targets one immutable host and protocol.
- Custom Middleware Header Ingestion and Context Propagation (deferred)
  Rejected because: Direct invocation of next/headers fulfills current requirements across routes and metadata handlers without introducing custom context-passing infrastructure.
  When valid: Complex multi-step request processing pipelines where header values must undergo uniform transformation and enrichment prior to reaching handlers.

## Risks

- Calling header retrieval functions outside dynamic request evaluation triggers unexpected server rendering failures or bailouts from caching.
  Mitigation: Isolate header store access to dynamic server routes and metadata handlers, and implement automated verification to catch unauthorized header access during static evaluation.
  Owner: Backend engineering team
- Inconsistent handling of forwarded host headers versus direct host headers leads to incorrect base URL construction across endpoints.
  Mitigation: Centralize domain resolution logic into shared helper functions that uniformly parse forwarded and direct host headers.
  Owner: Platform architecture team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Encapsulate incoming host and protocol header resolution within shared domain helper functions to prevent duplicated header inspection logic.
- Verify that all endpoints invoking header access functions operate within server-side dynamic request boundaries to avoid runtime errors during build-time rendering.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute it to verify server route compilation and type conformance.
- Identify and run the repository automated test suite to confirm dynamic header inspection behaves as expected across route handlers and metadata endpoints.

Accept when:
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

## Enforcement

- Verified by: Continuous integration pipeline running automated type checking and linting across all server modules.
- Verified by: Peer code review for all modifications touching server route handlers, metadata generators, or authentication utilities.
- Violation handling: Pull requests attempting to read headers through unsupported mechanisms or in client components are blocked by automated checks and rejected in code review.
- Exception process: Exceptions require documented architectural review and sign-off from the platform architecture team explaining why standard header resolution cannot be used.