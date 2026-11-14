---
version: 2
name: a703426a-862f-4a93-ad2f-f0d2de0f4eb2-fetch-api-client-boundary-integration-fetch-responses-containing-dynamic-untyped-payload
description: "Fetch API Client Boundary Integration: Fetch Responses Containing Dynamic Untyped Payload"
metadata:
  actual:
    adr_id: a703426a-862f-4a93-ad2f-f0d2de0f4eb2
    status: accepted
    candidate_group_id: 57dd9a6d-921b-457c-a0a1-87439126474c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Fetch API Client Boundary Integration: Fetch Responses Containing Dynamic Untyped Payload

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/content/**`
- `src/lib/**`

## Context

- Client components and server utilities must regularly communicate with internal route handlers, background checkers, and third-party web services.
- Introducing specialized third-party HTTP client libraries increases client bundle overhead and introduces architectural divergence between browser and server environments.
- The codebase establishes a unified boundary client pattern relying on the Fetch API for all outbound and local HTTP requests.

## Problem Statement

Without a unified pattern for cross-boundary network requests, disparate client and server components risk introducing conflicting HTTP client libraries, inconsistent timeout handling, and divergent request-response validation semantics.

## Decision

1. SHOULD: Fetch responses containing dynamic or untyped payload bodies SHOULD be validated against runtime schemas before domain consumption to guarantee type safety at boundary borders.

## Policy Block

- SHOULD Fetch responses containing dynamic or untyped payload bodies SHOULD be validated against runtime schemas before domain consumption to guarantee type safety at boundary borders.

In scope:
- All client-side components and hooks initiating requests to internal API routes.
- All server-side utility functions, background checkers, and route handlers communicating across external HTTP boundaries.

Out of scope:
- Direct database datastore queries managed through persistent data access layers.
- Internal intra-process module imports and direct function invocations.

## Rationale

- Standardizing on the Fetch API avoids external client bundle bloat and provides consistent HTTP semantics across both browser and server runtime environments.
- Native fetch allows seamless integration with platform caching controls, revalidation directives, and standard abort signal timeouts.
- Direct fetch usage enables flexible pairing with runtime schema validation libraries without binding application code to a proprietary HTTP transport client.

## Consequences

Positive:
- Zero external dependencies required for network boundary communication, keeping application bundle sizes lean.
- Consistent HTTP request mechanics and abort controller signal handling across browser and server contexts.
- Direct compatibility with platform-level caching, revalidation flags, and streaming response protocols.

Negative:
- Boilerplate required at call sites for setting content headers, checking HTTP status codes, and parsing response bodies.
- Callers must explicitly configure abort signals and error catching to prevent unhandled rejection states.

## Alternatives

- Third-party HTTP client library (rejected)
  Rejected because: Adds third-party bundle weight and abstracts over standard platform request primitives without delivering essential architectural advantages.
  When valid: When specialized transport interceptors or legacy browser compatibility layers are strictly mandated.
- Generated typed RPC client SDK (rejected)
  Rejected because: Increases coupling and tooling overhead for simple local route triggers and external third-party webhooks.
  When valid: When all endpoints share a unified machine-readable schema definition and strict contract generation is required.

## Risks

- Inconsistent HTTP error checking where non-two-hundred status codes do not reject the fetch promise automatically.
  Mitigation: Enforce response status verification and schema parsing policies across boundary integration points.
  Owner: engineering team
- Unbounded network latency causing hanging promises when abort signals are omitted.
  Mitigation: Require signal timeouts on all external network requests through linting and code review checks.
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
- Configure timeout and abort controllers uniformly using standard abort signals across client-side interaction triggers and background probing routines.
- Pair fetch operations with schema-based validation layers when ingesting third-party payloads or parsing route responses.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting command from the root configuration.
- Discover and execute the project test runner across all integration test suites.
- Discover and execute the project type checking script from the workspace manifest.

Accept when:
- Static analysis validates that all boundary network requests conform to the standard client rules without unapproved HTTP dependencies.
- All integration test suites covering internal routes and external boundary clients pass successfully.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and boundary linter rules.
- Verified by: Pull request architectural code reviews evaluating network request structure and timeout configurations.
- Violation handling: Pull requests introducing unapproved HTTP client libraries or unhandled fetch promises will be blocked by continuous integration checks.
- Violation handling: Detected violations require refactoring to use the standard Fetch API with appropriate signal and status handling before merge.
- Exception process: Exceptions for specialized third-party integration SDKs require architectural review approval and documented justification in the service specification.