---
version: 2
name: d6a3fbbb-8049-476c-b9f3-ca754923cc79-server-side-trpc-client-request-header-and-cookie-forwarding-modules-providing-server-side-clients-enforce
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Modules Providing Server Side Clients Enforce"
metadata:
  actual:
    adr_id: d6a3fbbb-8049-476c-b9f3-ca754923cc79
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Modules Providing Server Side Clients Enforce

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/lib/trpc/**`

## Context

- Server-side data fetching requires invoking internal API procedures within server execution boundaries prior to client rendering.
- Server-rendered components do not inherently transmit ambient client session cookies to internal services during server-side HTTP dispatch.
- Standardizing internal API access through a configured type-safe client ensures uniform session propagation and security posture across server modules.

## Problem Statement

When executing internal API requests from server-side rendering contexts, ambient browser credentials are not automatically attached to HTTP fetch requests, leading to authentication failures or requiring repetitive boilerplate across data loaders.

## Decision

1. MUST: Modules providing server-side API clients MUST enforce server-only execution boundaries to prevent accidental inclusion in client-side bundles.

## Policy Block

- MUST Modules providing server-side API clients MUST enforce server-only execution boundaries to prevent accidental inclusion in client-side bundles.

In scope:
- Server-side execution contexts invoking internal application API procedures.
- Components and data loaders operating in server-only execution boundaries requiring authenticated session propagation.

Out of scope:
- Client-side browser code communicating directly with public API routes.
- External third-party API integrations utilizing dedicated client libraries with API key authorization.

## Rationale

- Configuring a centralized @trpc/client instance for server execution guarantees that incoming request cookies are uniformly forwarded across internal API boundaries.
- Utilizing custom fetch transport options preserves request-scoped headers while enforcing credential inclusion across all procedure invocations.
- Constraining the client module to server-only environments mitigates the risk of exposing sensitive session handling logic to client bundles.

## Consequences

Positive:
- Transparent authentication and tenant propagation from incoming server requests to downstream internal services.
- Consistent type-safe client interface across server-rendered components without repetitive header wiring.
- Enforced runtime isolation preventing server credential handlers from leaking into browser bundles.

Negative:
- Server execution context is tightly coupled to HTTP header availability and server runtime cookie stores.
- Improper cache configurations on forwarded requests can risk leaking user-specific data across concurrent invocations.
- Increased complexity in mocking server request contexts during isolated unit testing.

## Alternatives

- Direct Unauthenticated Server Fetching (rejected)
  Rejected because: Bypasses request-scoped session context and fails when accessing protected internal endpoints requiring caller identity.
  When valid: Valid only for completely public endpoints with no tenant or user context requirements.
- Explicit Token Parameter Passing on Every Procedure Call (rejected)
  Rejected because: Pollutes API procedure signatures with infrastructure concerns and increases boilerplate across calling components.
  When valid: Valid when interacting with external third-party APIs that mandate distinct bearer token authorization headers per request.

## Risks

- Cookie header forwarding across internal fetch requests may cause unintended caching of authenticated responses in shared caches.
  Mitigation: Audit server-side fetch caching configurations and ensure dynamic request headers force per-request resolution where user data is returned.
  Owner: Engineering Team
- Accidental client-side inclusion of server-scoped client utilities could expose internal network endpoints or trigger execution errors.
  Mitigation: Enforce server-only boundary directives and automated boundary checks to prevent server client code from entering browser bundles.
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct the server API client transport using a dedicated request-scoped context that extracts existing incoming cookies.
- Ensure custom fetch options retain incoming request headers while overriding target credential and cookie parameters.

## Continuation Context


Verify commands:
- Discover and execute the project verification script for static boundary and lint enforcement.
- Discover and run the project automated test suite covering server-side data fetching and internal API transport.

Accept when:
- Internal API calls executed within server-side environments successfully convey authenticated session headers without manual per-call header construction.
- Static boundary checks prevent client-side evaluation of server-scoped API client configurations.
- Automated test suites simulating server-rendered data fetching execute without authentication transport errors.

## Enforcement

- Verified by: Automated architectural boundary linting verifying that server client configurations remain restricted to server execution contexts.
- Verified by: Continuous integration test suites verifying session cookie propagation and error handling during server data retrieval.
- Verified by: Mandatory peer code review for all internal API client transports and wrapper definitions.
- Violation handling: Pull requests containing custom unapproved internal API fetch transports will be blocked during automated verification.
- Violation handling: Violations identified in code reviews must be refactored to use the standardized server client configuration before merging.
- Violation handling: Any architectural drift causing unauthenticated internal service invocations triggers critical review alerts.
- Exception process: Submit an architectural review request detailing why an internal service must be accessed outside the standardized client transport.
- Exception process: Obtain explicit security approval for any alternate authentication or credential delegation mechanism.
- Exception process: Document and register the exception in the architectural tracking log.