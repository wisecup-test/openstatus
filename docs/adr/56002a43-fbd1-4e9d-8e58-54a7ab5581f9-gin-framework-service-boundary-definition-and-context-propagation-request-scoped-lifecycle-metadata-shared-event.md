---
version: 2
name: 56002a43-fbd1-4e9d-8e58-54a7ab5581f9-gin-framework-service-boundary-definition-and-context-propagation-request-scoped-lifecycle-metadata-shared-event
description: "Gin Framework Service Boundary Definition and Context Propagation: Request Scoped Lifecycle Metadata Shared Event"
metadata:
  actual:
    adr_id: 56002a43-fbd1-4e9d-8e58-54a7ab5581f9
    status: accepted
    candidate_group_id: 35868210-db45-458b-9030-71b77645c31c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/server/**
      - handlers/**
      - pkg/otel/**
      - pkg/tinybird/**
---

# Gin Framework Service Boundary Definition and Context Propagation: Request Scoped Lifecycle Metadata Shared Event

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `checker/**`
- `cmd/server/**`
- `handlers/**`
- `pkg/otel/**`
- `pkg/tinybird/**`

## Context

- The service exposes multi-protocol health check and probe execution endpoints requiring consistent boundary routing and execution semantics.
- Incoming requests require correlation identifiers and mutable event state injected at the HTTP boundary before delegation to protocol check handlers.
- A unified routing and context abstraction is necessary to propagate lifecycle data across middleware and boundary handlers without polluting domain handler signatures.

## Problem Statement

The service must expose multiple protocol checking endpoints while maintaining consistent lifecycle metadata, correlation identifiers, and execution events across middleware and handlers without tightly coupling protocol execution logic to transport routing signatures.

## Decision

1. MUST: Request-scoped lifecycle metadata and shared event state passed across middleware and boundary handlers MUST be stored and retrieved using context Set and Get methods.

## Policy Block

- MUST Request-scoped lifecycle metadata and shared event state passed across middleware and boundary handlers MUST be stored and retrieved using context Set and Get methods.

In scope:
- HTTP server entrypoints and route registration components.
- Boundary handler functions receiving external requests.
- Middleware components managing request-scoped metadata and correlation state.

Out of scope:
- Standalone background workers operating independently of the request-response lifecycle.
- Low-level protocol client implementations that do not interface with service boundaries.

## Rationale

- Standardizing route definitions on the Gin router establishes uniform entrypoint semantics across all protocol check endpoints.
- Utilizing Gin context key-value storage allows middleware to inject correlation identifiers and mutable event models that downstream handlers access without polluting handler signatures.
- Consistent context-based boundary passing simplifies cross-cutting concerns including logging, metrics collection, and assertion evaluation.

## Consequences

Positive:
- Standardized route declarations and context conventions across all service boundary endpoints.
- Decoupled propagation of request-scoped correlation identifiers and execution event objects across middleware and handlers.
- Clear separation between network transport routing and protocol evaluation logic.

Negative:
- Key-value context retrieval relies on string identifiers and runtime type assertions rather than compile-time type validation.
- Protocol check handlers become structurally coupled to the web framework context abstraction.

## Alternatives

- Standard Library HTTP Handlers with Custom Context Wrap (rejected)
  Rejected because: Requires significant boilerplate for route parameterization and lacks established conventions for unified context map mutation across middleware.
  When valid: When building lightweight services prioritizing zero third-party framework dependencies.
- Explicit Strongly Typed Parameter Passing Across Handlers (rejected)
  Rejected because: Couples handler function signatures directly to cross-cutting concerns and forces widespread refactoring when adding correlation or tracing attributes.
  When valid: When compile-time verification of all request attributes takes precedence over middleware composition.

## Risks

- Runtime panics or missing state resulting from inaccurate context key retrieval or incorrect type assertions.
  Mitigation: Encapsulate context key retrieval within helper functions that enforce safe type assertions and error handling.
  Owner: engineering team
- Overloading the request context with excessive domain state, blurring boundary responsibilities.
  Mitigation: Restrict context storage to request-scoped metadata, correlation identifiers, and shared event carriers.
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
- Define centralized constant identifiers for all context storage keys to avoid typographical retrieval errors across boundary components.
- Extract context values early within boundary handlers and validate types safely before initiating downstream protocol evaluations.

## Continuation Context


Verify commands:
- Inspect the project manifest to identify the configured test runner and run all test suites covering API boundary handlers.
- Execute the repository static analysis and linting tools to verify route definitions and context usage compliance.

Accept when:
- All service boundary routes compile and pass automated test suites covering HTTP and protocol checking endpoints.
- Context metadata propagation operations complete without runtime type assertion failures or missing key errors.

## Enforcement

- Verified by: Automated continuous integration checks executing test suites and static analysis on service boundary components.
- Verified by: Mandatory peer code review for changes affecting route declarations and request context propagation.
- Violation handling: Pull requests introducing bypasses to context storage or non-compliant routing declarations will be blocked.
- Violation handling: Code violating context safety rules must be refactored to use standard context retrieval abstractions before deployment.
- Exception process: Exceptions must be documented in an architectural exception request specifying the technical constraints necessitating an alternative boundary pattern.
- Exception process: Approval from service maintainers is required prior to integrating non-standard routing or context handling mechanisms.