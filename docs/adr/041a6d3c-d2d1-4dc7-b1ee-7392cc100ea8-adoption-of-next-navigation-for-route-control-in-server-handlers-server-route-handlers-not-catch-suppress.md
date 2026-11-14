---
version: 2
name: 041a6d3c-d2d1-4dc7-b1ee-7392cc100ea8-adoption-of-next-navigation-for-route-control-in-server-handlers-server-route-handlers-not-catch-suppress
description: "Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Not Catch Suppress"
metadata:
  actual:
    adr_id: 041a6d3c-d2d1-4dc7-b1ee-7392cc100ea8
    status: accepted
    candidate_group_id: 064cba9e-e3a9-42d4-8a30-a0da305cf972
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of next/navigation for Route Control in Server Handlers: Server Route Handlers Not Catch Suppress

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`

## Context

- Server-side route handlers for public content feeds require consistent mechanisms to abort processing and signal missing or inaccessible resources.
- Standardizing on framework-provided navigation primitives prevents disparate endpoints from implementing incompatible status responses or ad-hoc redirection logic.
- Public feed endpoints validate authentication and authorization preconditions against incoming request parameters before initiating feed serialization.

## Problem Statement

Public server route handlers require a standardized, framework-integrated mechanism to halt execution and signal state changes such as not-found or redirection conditions without duplicating custom response construction or misaligning with framework-level request lifecycle handling.

## Decision

1. MUST_NOT: Server route handlers MUST NOT catch or suppress framework control flow exceptions thrown by next/navigation primitives, allowing the framework runtime to intercept and process the navigation boundary.

## Policy Block

- MUST_NOT Server route handlers MUST NOT catch or suppress framework control flow exceptions thrown by next/navigation primitives, allowing the framework runtime to intercept and process the navigation boundary.

In scope:
- Server-side route handlers and endpoint handlers within the application framework.
- Public feed endpoints that evaluate access preconditions and enforce route-level boundary conditions.

Out of scope:
- Client-side components managing interactive browser-level navigation.
- Utility functions and data-layer services that do not operate within server request handling contexts.

## Rationale

- Direct adoption of next/navigation aligns server endpoint flow control with framework lifecycle conventions, ensuring runtime boundary handlers intercept termination signals correctly.
- Consistently invoking framework navigation primitives eliminates boilerplate response assembly and guarantees uniform error signaling across all feed delivery routes.
- Coupling precondition verification with framework-level not-found triggers prevents unauthorized information disclosure when access parameters fail validation.

## Consequences

Positive:
- Uniform handling of not-found and redirect states across all server route handlers.
- Elimination of custom HTTP error response construction and redundant status header logic.
- Seamless integration with framework-level error boundaries and logging infrastructure.

Negative:
- Tight coupling of server route handler flow control to the specific framework navigation module.
- Requirement that developers avoid catching framework-specific navigation control flow exceptions.
- Limited flexibility for returning custom structured payload bodies when triggering standard navigation boundaries.

## Alternatives

- Manual HTTP Response Construction for Route Termination (rejected)
  Rejected because: Constructing raw HTTP response objects manually across individual route handlers causes divergent error formats and bypasses framework-level boundary interception.
  When valid: Valid when implementing standalone server handlers decoupled from the application framework.
- Global Middleware Interception for Feed Preconditions (deferred)
  Rejected because: Route-specific feed query parameter validation and service dependencies are more cleanly maintained at the endpoint boundary until generalized across all routes.
  When valid: Valid when uniform authentication rules apply identically across all public and private routes without route-specific parameter logic.

## Risks

- Accidental capture of framework navigation control exceptions in general try-catch blocks causing suppressed navigation.
  Mitigation: Ensure exception handling blocks rethrow framework-specific control signals or position navigation calls outside broad try-catch scopes.
  Owner: engineering team
- API drift or breaking changes in framework navigation exports across dependency updates.
  Mitigation: Enforce strict lock-resolution verification and validate navigation call sites against authoritative documentation before updates.
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
- Invoke framework navigation primitives such as notFound or redirect directly in route handlers prior to executing expensive feed aggregation logic.
- When wrapping endpoint logic in error-handling blocks, verify that internal framework navigation errors are not intercepted or transformed into generic server error responses.

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis and type checking script to verify correct import paths and signature compliance for framework navigation primitives.
- Discover and execute the project's automated test suite to ensure server route handlers return expected framework boundary responses upon invalid parameters.

Accept when:
- Static analysis passes with zero type errors or unresolved module errors for navigation imports across server route handlers.
- Automated test suites confirm that route handlers invoke navigation boundary primitives when preconditions or access parameters are unsatisfied.

## Enforcement

- Verified by: Automated continuous integration checks executing static analysis and test validation suites.
- Verified by: Peer code reviews verifying compliance with navigation module adoption guidelines.
- Violation handling: Pull requests introducing manual response construction where navigation primitives apply will be blocked until refactored.
- Violation handling: Static analysis failures for unresolved or deprecated imports must be resolved prior to merge.
- Exception process: Exceptions require documented architectural review demonstrating why framework navigation boundaries cannot satisfy the specific protocol requirements.