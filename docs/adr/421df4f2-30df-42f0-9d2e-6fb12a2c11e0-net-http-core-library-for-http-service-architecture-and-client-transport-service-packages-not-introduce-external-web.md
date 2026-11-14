---
version: 2
name: 421df4f2-30df-42f0-9d2e-6fb12a2c11e0-net-http-core-library-for-http-service-architecture-and-client-transport-service-packages-not-introduce-external-web
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Service Packages Not Introduce External Web"
metadata:
  actual:
    adr_id: 421df4f2-30df-42f0-9d2e-6fb12a2c11e0
    status: accepted
    candidate_group_id: 1853f30f-7e00-4db1-8d56-6feed02d0ef4
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
      - proto/private_location/v1/**
---

# net/http Core Library for HTTP Service Architecture and Client Transport: Service Packages Not Introduce External Web

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/private-location`

Applies to:
- `cmd/server/**`
- `internal/server/**`
- `internal/tinybird/**`
- `internal/workflows/**`
- `proto/private_location/v1/**`

## Context

- The service architecture requires concurrent HTTP server lifecycle handling, route dispatching, response instrumentation, and outbound HTTP integrations.
- Evidence across service entrypoints, routes, and client packages demonstrates uniform adoption of the net/http library and context package.
- Outbound client integrations and internal protocol handlers coordinate request dispatching, authentication headers, and timeout management through standard net/http client primitives.

## Problem Statement

Microservice architectures often fragment across multiple third-party HTTP frameworks and diverging client abstractions, leading to incompatible middleware layers, inconsistent timeout enforcement, and uncoordinated server shutdown procedures. A unified, standard HTTP transport foundation is required to ensure consistent request lifecycles, structured logging, and robust lifecycle controls across all service components.

## Decision

1. MUST_NOT: Service packages MUST_NOT introduce external web frameworks or custom networking abstractions that bypass standard http.Handler and http.ResponseWriter interfaces.

## Policy Block

- MUST_NOT Service packages MUST_NOT introduce external web frameworks or custom networking abstractions that bypass standard http.Handler and http.ResponseWriter interfaces.

In scope:
- HTTP server initialization and connection lifecycle management.
- HTTP route definitions and response handling.
- Outbound HTTP API client integrations.

Out of scope:
- In-memory message channels that do not communicate over network transport.
- Non-HTTP protocol transports managed by dedicated system drivers.

## Rationale

- Standardizing on net/http eliminates heavy external web framework dependencies, ensuring high portability and long-term API stability across the codebase.
- The net/http interfaces provide native compatibility with standard HTTP routing, middleware wrappers, and RPC protocol extensions.
- Enforcing explicit timeout configurations and context propagation on net/http clients prevents connection leakage and cascading timeouts under heavy network load.

## Consequences

Positive:
- Zero third-party framework lock-in and minimal dependency footprint for network transport.
- Uniform request handling, status code inspection, and context cancellation semantics across all server routes and client clients.
- Reliable process termination through standard server shutdown procedures.

Negative:
- Common routing and middleware conveniences found in higher-level frameworks must be implemented or composed manually.
- Developers must explicitly manage request headers, response writer wrappers, and client timeouts without framework-level automated defaults.

## Alternatives

- Third-Party Web Application Framework (rejected)
  Rejected because: Introduces heavy external dependencies, proprietary middleware signatures incompatible with standard handler interfaces, and increased maintenance overhead.
  When valid: When developing monolithic web applications requiring bundled templating, session state engines, and complex form handling.
- Custom Low-Level Socket Transport (rejected)
  Rejected because: Creates unnecessary protocol complexity, increases maintenance risk, and duplicates well-tested standard library transport mechanisms.
  When valid: When implementing custom proprietary binary network protocols where HTTP framing introduces unacceptable overhead.

## Risks

- Unconfigured outbound HTTP clients can hang indefinitely if remote endpoints become unresponsive.
  Mitigation: Enforce mandatory explicit timeout configurations and context propagation on all instantiated http.Client structures through automated linting and code review.
  Owner: engineering team
- Custom http.ResponseWriter wrappers might fail to propagate status codes or optional interface capabilities like flushing.
  Mitigation: Implement comprehensive unit tests for response wrapper behaviors validating status code defaults and delegate method invocations.
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
- Instantiate outbound http.Client instances with bounded timeout settings and pass context.Context to all request builders.
- Implement response instrumentation by wrapping http.ResponseWriter to capture the HTTP status code before delegating to the underlying response writer.
- Structure server startup routines to bind listeners and invoke graceful shutdown procedures upon receiving termination signals.

## Continuation Context


Verify commands:
- Discover and execute the repository test suite to verify HTTP server handlers, client transports, and response writer implementations.
- Discover and run the repository static analysis and linting checks to ensure all HTTP clients configure explicit timeouts and pass context objects.

Accept when:
- All HTTP route handlers and client integration tests pass successfully without connection leaks or timeout errors.
- Static analysis verification confirms no unconfigured http.Client instances or context-less network calls exist.

## Enforcement

- Verified by: Continuous integration test validation suites and automated static analysis.
- Verified by: Peer review of pull requests modifying server routing, middleware wrappers, or outbound HTTP clients.
- Violation handling: Pull requests introducing external web frameworks or omitting client timeout configurations are blocked from merging until revised.
- Violation handling: Static analysis warnings regarding missing context propagation must be resolved before deployment.
- Exception process: Proposals to introduce alternative transport libraries or specialized client configurations require architectural review board approval and documentation in an amending ADR.