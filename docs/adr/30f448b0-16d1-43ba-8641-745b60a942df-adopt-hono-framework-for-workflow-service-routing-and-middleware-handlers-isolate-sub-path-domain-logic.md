---
version: 2
name: 30f448b0-16d1-43ba-8641-745b60a942df-adopt-hono-framework-for-workflow-service-routing-and-middleware-handlers-isolate-sub-path-domain-logic
description: "Adopt Hono Framework for Workflow Service Routing and Middleware: Handlers Isolate Sub Path Domain Logic"
metadata:
  actual:
    adr_id: 30f448b0-16d1-43ba-8641-745b60a942df
    status: accepted
    candidate_group_id: 21dd5eee-ded5-43d1-9312-6f96f5101826
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
---

# Adopt Hono Framework for Workflow Service Routing and Middleware: Handlers Isolate Sub Path Domain Logic

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/checker/**`
- `src/cron/**`
- `src/incident/**`

## Context

- Workflow services and background cron tasks handle scheduled invocations, external service incident cleanups, and private location monitoring checks.
- HTTP triggers driving workflow and task execution require unified endpoint declaration, shared secret authorization verification, and structured payload extraction.
- The codebase observed across workflow modules adopts Hono to provide routing infrastructure and uniform middleware chaining across all workflow endpoints.

## Problem Statement

Workflow execution and cron scheduling services require a lightweight, consistent mechanism to declare HTTP route boundaries, enforce authorization checks on external triggers, and parse incoming event payloads. Without a standardized routing framework, individual background tasks and checker endpoints risk fragmented middleware handling, divergent request context extraction, and inconsistent authentication enforcement across dispatch triggers.

## Decision

1. SHOULD: Handlers SHOULD isolate sub-path domain logic into dedicated router instances that are mounted onto the primary application router.

## Policy Block

- SHOULD Handlers SHOULD isolate sub-path domain logic into dedicated router instances that are mounted onto the primary application router.

In scope:
- All HTTP-triggered workflow modules, background cron dispatchers, and monitor checker services.
- All middleware layers enforcing authentication, authorization, or context binding for background task endpoints.

Out of scope:
- Internal utility functions, database schema definitions, and standalone calculation algorithms without HTTP endpoints.
- Independent data processing tasks that do not receive incoming HTTP or webhook triggers.

## Rationale

- Adopting Hono as the standard routing layer provides a uniform API for declaring endpoints and managing request-response lifecycles across all workflow handlers.
- Framework-level middleware chaining allows centralized verification of authorization secrets across wildcard route paths, eliminating duplicate authentication logic.
- The framework context abstraction standardizes access to headers, query parameters, and parsed payloads while maintaining minimal runtime overhead.

## Consequences

Positive:
- Standardized endpoint declaration and parameter parsing across all background workflow handlers.
- Centralized security enforcement through reusable middleware applied across entire route subtrees.
- Consistent error handling and response status formatting across workflow dispatch interfaces.

Negative:
- Workflow handlers are coupled to the routing framework context and request abstractions.
- Changes to framework middleware semantics or routing APIs require updates across all registered workflow endpoints.

## Alternatives

- Native Runtime HTTP Handlers (rejected)
  Rejected because: Lacks built-in middleware composition, requiring custom boilerplate for routing, authorization header parsing, and sub-router isolation.
  When valid: Valid in minimal standalone scripts with a single endpoint and zero middleware requirements.
- Heavyweight Enterprise Web Frameworks (rejected)
  Rejected because: Introduces excessive startup latency and unnecessary runtime overhead for lightweight, high-frequency background workflow executions.
  When valid: Valid in monolithic web applications requiring integrated server-side rendering or complex dependency injection containers.

## Risks

- Route handlers might fail to attach mandatory authorization middleware if wildcard mounts are misconfigured.
  Mitigation: Enforce automated integration testing covering unauthorized requests across all exposed endpoints.
  Owner: Engineering Team
- Context variable collisions when setting and retrieving custom properties on the framework context.
  Mitigation: Define explicit type schemas for context variables across all router modules.
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
- Mount domain-specific handlers onto sub-routers and register them with the parent application router using clear path prefixes.
- Apply authorization middleware at the router root before attaching individual route endpoints to ensure blanket protection across all operations.

## Continuation Context


Verify commands:
- Discover the project testing script from the dependency manifest and execute the test suite covering workflow routing endpoints.
- Discover the project static analysis and lint script from the repository manifest and run validation against all route definition modules.
- Discover the type validation command defined in the project configuration and verify type compatibility across all route handlers.

Accept when:
- All workflow endpoints are registered on framework router instances and successfully pass automated HTTP integration tests.
- Requests omitting valid authorization headers are rejected with an unauthorized status prior to downstream handler execution.
- Type validation and lint verification scripts execute without errors across all router and handler modules.

## Enforcement

- Verified by: Automated continuous integration test suites verifying HTTP route responses and authorization enforcement.
- Verified by: Peer code review validating adherence to framework routing and middleware conventions.
- Violation handling: Pull requests containing unrouted handlers or missing authorization middleware are blocked from merging.
- Violation handling: Detected violations must be refactored to use standard framework routing and middleware patterns prior to deployment.
- Exception process: Architectural review and written approval by the engineering lead detailing the specific operational need to bypass framework routing.