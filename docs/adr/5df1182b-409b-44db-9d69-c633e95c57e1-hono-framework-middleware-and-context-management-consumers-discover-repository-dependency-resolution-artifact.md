---
version: 2
name: 5df1182b-409b-44db-9d69-c633e95c57e1-hono-framework-middleware-and-context-management-consumers-discover-repository-dependency-resolution-artifact
description: "Hono Framework Middleware and Context Management: Consumers Discover Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 5df1182b-409b-44db-9d69-c633e95c57e1
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Consumers Discover Repository Dependency Resolution Artifact

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/libs/middlewares/**`

## Context

- Server HTTP endpoints require cross-cutting handling for authentication scoping, plan tier verification, telemetry tracking, and header inspection.
- Passing resolved entities across middleware boundaries requires a standardized request context propagation mechanism to avoid redundant resolution or unstructured request mutations.
- The server codebase standardizes on the Hono web framework and its context abstraction to coordinate request-scoped data storage, header retrieval, and exception handling across middleware layers.

## Problem Statement

Without a unified request context convention in HTTP middleware, cross-cutting concerns such as identity extraction, authorization checks, and analytics logging resort to inconsistent state-sharing strategies or redundant upstream database lookups. The application requires a disciplined, framework-native mechanism to read request metadata, share verified domain entities across middleware layers, and raise structured HTTP exceptions.

## Decision

1. MUST: Consumers MUST discover the repository dependency resolution artifact to confirm the exact resolved version of the Hono library and verify API compatibility prior to implementing middleware handlers.

## Policy Block

- MUST Consumers MUST discover the repository dependency resolution artifact to confirm the exact resolved version of the Hono library and verify API compatibility prior to implementing middleware handlers.

In scope:
- Server HTTP middleware modules and route processing pipelines managing request lifecycle, authorization, and telemetry.
- Request-scoped state accessors resolving domain entities or client request headers.

Out of scope:
- Background asynchronous workers and scheduled tasks that run outside HTTP request-response cycles.
- Shared utility functions, domain entity models, and client-side modules that do not handle HTTP routing.

## Rationale

- The IR evidence across multiple middleware modules demonstrates consistent reliance on Hono context methods, specifically context entity retrieval and header inspection, establishing Hono as the standard HTTP abstraction.
- Standardizing request-scoped data storage within the Hono context decouples individual middleware responsibilities, allowing downstream middlewares and handlers to consume verified entities without re-parsing headers or querying storage layers.
- Using native framework abstractions for exception handling ensures that authorization and verification failures generate predictable HTTP responses across all server endpoints.

## Consequences

Positive:
- Establishes a uniform, composable pattern for reading headers and sharing validated domain entities across middleware chains.
- Eliminates duplicate header parsing and redundant upstream database lookups across successive middleware layers.
- Standardizes error propagation using framework-native HTTP exception abstractions for predictable client response contracts.

Negative:
- Couples server middleware functions and request-processing logic directly to Hono framework context signatures.
- Requires disciplined key naming and type synchronization across context accessors to prevent runtime undefined entity lookups.

## Alternatives

- Mutating raw runtime request objects or global request property dictionaries (rejected)
  Rejected because: Direct mutation of low-level runtime request primitives lacks type safety, risks key collision across middleware layers, and impairs portability across execution runtimes.
  When valid: Valid only in minimal standalone scripts operating without an HTTP framework middleware pipeline.
- Propagating request state through asynchronous execution local storage (rejected)
  Rejected because: Asynchronous local storage introduces runtime overhead and tracking complexity without providing native integration with HTTP headers, routing, or response envelopes.
  When valid: Valid when tracing operations across deep background service boundaries that cannot receive framework context parameters.

## Risks

- Mismatched or unvalidated context keys could result in undefined entity lookups at runtime.
  Mitigation: Enforce centralized type definitions for context variables across all middleware accessors.
  Owner: Server Engineering Team
- Framework context API changes across dependency updates could break middleware accessor contracts.
  Mitigation: Follow the mandatory lock-version grounding process before updating framework dependencies.
  Owner: Server Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define unified type schemas for Hono context variables to guarantee compile-time safety across context setter and getter invocations.
- Structure middleware dependencies such that prerequisite context providers execute prior to dependent authorization or logging middlewares in the HTTP pipeline.

## Continuation Context


Verify commands:
- Discover the repository dependency manifest and execute the project type-checking script to confirm all Hono context accessors adhere to defined context type contracts.
- Discover and run the project test suite to verify that middleware context propagation and HTTP exception handling pass all automated test scenarios.

Accept when:
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

## Enforcement

- Verified by: Automated continuous integration checks executing repository type verification and test suites.
- Verified by: Peer code review confirming that new server middlewares use Hono context methods and standard exception classes.
- Violation handling: Pull requests attempting direct request mutation or bypassing Hono context accessors will be rejected during review.
- Exception process: Exceptions require documented architectural review and formal approval from the server architecture team.