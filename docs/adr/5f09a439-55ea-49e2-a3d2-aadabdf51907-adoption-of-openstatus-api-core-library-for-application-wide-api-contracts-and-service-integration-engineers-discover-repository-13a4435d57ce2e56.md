---
version: 2
name: 5f09a439-55ea-49e2-a3d2-aadabdf51907-adoption-of-openstatus-api-core-library-for-application-wide-api-contracts-and-service-integration-engineers-discover-repository-13a4435d57ce2e56
description: "Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Engineers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 5f09a439-55ea-49e2-a3d2-aadabdf51907
    status: accepted
    candidate_group_id: 680fce54-561c-430b-a8f6-11b8816f240e
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/api/trpc/lambda/**
      - src/components/**
      - src/content/**
      - src/data/**
      - src/lib/trpc/**
---

# Adoption of @openstatus/api Core Library for Application-Wide API Contracts and Service Integration: Engineers Discover Repository Dependency Manifest Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/api/trpc/lambda/**`
- `src/components/**`
- `src/content/**`
- `src/data/**`
- `src/lib/trpc/**`

## Context

- Application features across server routes, content generators, data metrics, and client components require access to consistent domain entities and API interaction contracts.
- Multiple presentation and routing modules exhibited direct reliance on @openstatus/api for type definitions, error structures, and client-server communication channels.
- Consolidating API contracts into a centralized library establishes an architectural boundary between underlying database schemas and application consumption layers.

## Problem Statement

Dispersed full-stack applications often suffer from contract drift, divergent error representations, and duplicated schema validation when client components and server route handlers define network interfaces independently. The architecture requires a unified, authoritative library for domain contracts, endpoint definitions, and client-server bindings.

## Decision

1. MUST: Engineers MUST discover the repository dependency manifest and inspect the authoritative lock artifact to verify the exact resolved version of all shared workspace and external dependencies prior to implementation.

## Policy Block

- MUST Engineers MUST discover the repository dependency manifest and inspect the authoritative lock artifact to verify the exact resolved version of all shared workspace and external dependencies prior to implementation.

In scope:
- All application server route handlers, API lambda routes, and data aggregation utilities.
- Client-side forms, navigation bars, metrics dashboards, and interactive status components requiring backend domain data.
- Build-time markdown and structured content generators assembling status domain feeds.

Out of scope:
- Third-party client integrations operating strictly via public webhook protocols or isolated external vendor SDKs.
- Standalone stylistic and design system primitives that do not interact with domain data models.

## Rationale

- Thirteen distinct files spanning API handlers, shared libraries, static generators, and UI components consistently import @openstatus/api, demonstrating an established architectural decision to centralize service interfaces.
- Centralizing endpoint schemas and error definitions into @openstatus/api reduces defect rates caused by serialization discrepancies between client and server runtimes.
- Adopting an authoritative internal library contract accelerates feature iteration by enabling automated type propagation across the entire codebase.

## Consequences

Positive:
- Enforces compile-time type safety across route handlers, data generators, and client UI components.
- Eliminates redundant contract definitions and standardizes error responses across application boundaries.
- Streamlines full-stack feature development by sharing types, routing protocols, and validation schemas directly.

Negative:
- Client and server application layers are tightly coupled to the @openstatus/api module interface, requiring coordinated updates when breaking contract changes occur.
- Increased bundle footprint if tree-shaking or export boundary separation within @openstatus/api is not strictly maintained.

## Alternatives

- Ad-hoc decoupled client fetch schemas and independent REST endpoint contracts (rejected)
  Rejected because: Introduces schema drift between server-side implementations and client-side consumption, increasing runtime parsing errors and duplicating contract maintenance.
  When valid: When consuming third-party external services outside the organization control boundary.
- Independent domain API libraries per feature or route domain (rejected)
  Rejected because: Fragments domain models, complicates shared error handling, and creates version skew across sub-packages.
  When valid: When sub-domains operate as autonomous microservices with independent release cycles.

## Risks

- Breaking changes in @openstatus/api can ripple across both server handlers and client presentation components simultaneously.
  Mitigation: Maintain strict semantic versioning and backward-compatible contract migrations within the core API library.
  Owner: engineering team
- Accidental inclusion of server-only utilities into client bundles when consuming shared contracts.
  Mitigation: Enforce modular packaging and entrypoint boundaries within the library so client bundles do not include server-only routines.
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
- Ensure module import paths adhere to the public export boundaries defined by the core library to preserve encapsulation.
- Structure client-side reactive consumers to use shared router bindings rather than manual request invocations.

## Continuation Context


Verify commands:
- Discover and execute the repository typechecking suite to verify end-to-end type compliance with @openstatus/api contracts.
- Discover and execute the dependency architecture verification script to detect unapproved network fetch calls or unauthorized external schema definitions.

Accept when:
- All API interaction points, route adapters, and interactive components import contracts and client bindings from @openstatus/api.
- Static analysis and verification suites pass with zero unregistered network schemas or conflicting contract definitions.
- Resolved lockfile dependencies are verified and aligned across the workspace.

## Enforcement

- Verified by: Automated dependency linting and module import boundary verification checks in continuous integration.
- Verified by: Peer code review of changes touching API endpoints, client data hooks, and route handlers.
- Verified by: Typecheck validation ensuring all consumed contracts originate from @openstatus/api.
- Violation handling: Build failure in continuous integration when unauthorized duplicate contracts or network wrappers are introduced.
- Violation handling: Code review rejection requiring refactoring to adopt the standard @openstatus/api exports.
- Exception process: Submit an architectural request detailing why the core API library cannot support the specific integration pattern.
- Exception process: Obtain approval from the lead architecture reviewer prior to introducing non-standard network interfaces.
- Exception process: Document the isolated contract boundary and plan for eventual migration into @openstatus/api.