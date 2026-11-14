---
version: 2
name: 2b26b5ce-13d4-466e-ba78-1360dc3f99c1-adoption-of-openstatus-api-internal-module-for-service-contracts-and-procedure-routing-server-side-execution-contexts-client-consume
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Server Side Execution Contexts Client Consume"
metadata:
  actual:
    adr_id: 2b26b5ce-13d4-466e-ba78-1360dc3f99c1
    status: accepted
    candidate_group_id: 6a007d6a-909a-425c-8c5a-15faed6eeac3
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Server Side Execution Contexts Client Consume

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/**`
- `src/trpc/**`

## Context

- Application endpoints, webhook handlers, and user interface components require a shared contract layer to avoid schema divergence and duplicate procedure declarations across runtime boundaries.
- Direct multi-package imports show that route handlers, server-rendered components, client query wrappers, and metadata generators rely on a common internal module for type definitions, error mappings, and procedure callers.
- Maintaining API contracts independently within each consumer boundary leads to drift and coordination overhead between frontend consumers and backend handlers.

## Problem Statement

Decentralized API interface declarations and error structures across server routes, background webhooks, and client-side data consumers introduce schema synchronization failures, duplicate serialization logic, and broken type safety across runtime boundaries.

## Decision

1. MUST: Server-side execution contexts and client-side UI contexts MUST consume procedure callers and error transformers exported by the dedicated subpath interfaces of the internal API module.

## Policy Block

- MUST Server-side execution contexts and client-side UI contexts MUST consume procedure callers and error transformers exported by the dedicated subpath interfaces of the internal API module.

In scope:
- All application endpoints, route handlers, and webhook receivers invoking or defining remote procedures
- All client components and server rendering components fetching data through shared service procedures
- Metadata generators and auxiliary HTTP handlers querying domain entities via internal API interfaces

Out of scope:
- Static asset delivery and non-procedure HTTP endpoints that do not interface with application domain models
- Third-party external webhooks requiring raw unparsed payloads prior to signature validation

Exceptions:
- EXC-20-001: An external webhook requires isolated raw payload parsing before delegation to the internal API module

## Rationale

- Consolidating API contract definitions and procedure handlers into the @openstatus/api module establishes an authoritative source of truth across client and server execution environments.
- Evidence across eight distinct files shows recurring reliance on @openstatus/api for server-side routing, webhook processing, client query state, and error handling, confirming intentional centralized architecture.
- Centralizing contracts minimizes runtime translation defects, eliminates duplicate interface maintenance, and guarantees type synchronization across distributed runtime contexts.

## Consequences

Positive:
- Enforces strict compile-time type consistency between procedure providers and consumers across the entire codebase.
- Eliminates duplicate schema definitions and centralizes error handling logic across diverse runtime contexts.
- Simplifies maintenance and refactoring of remote procedures by isolating changes within a single shared module.

Negative:
- Creates tight coupling between application consumer layers and the internal API package structure.
- Requires rebuilds or republishing of the internal API module whenever procedure signatures or error schemas evolve.

## Alternatives

- Decentralized inline route handlers with manual REST endpoints and local TypeScript interfaces (rejected)
  Rejected because: Increases maintenance overhead, risks contract drift between client and server, and lacks unified schema validation
  When valid: When developing simple monolithic applications with minimal cross-boundary contract sharing
- Third-party schema-only definition packages without shared procedure runtime helpers (rejected)
  Rejected because: Fails to provide unified runtime error handling, client adapters, and server execution wrappers
  When valid: When consumers and producers are polyglot services deployed across completely independent repositories

## Risks

- Breaking changes in the shared API module can simultaneously impact client interfaces and server route handlers
  Mitigation: Establish strict contract testing and backward-compatible procedure evolution protocols across the monorepo
  Owner: engineering team
- Monolithic growth of the shared API module leading to slow build and bundling times
  Mitigation: Structure module exports into targeted subpath entry points to enable selective import resolution and tree shaking
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
- Import remote procedures and error types through modular package subpaths rather than importing top-level namespace aggregates.
- Configure client query providers and server callers using the standardized context adapters defined by the shared module.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type checking script to verify that all procedure calls match module contracts.
- Discover and run the workspace test suite to ensure server route handlers and client callers integrate without contract violations.

Accept when:
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

## Enforcement

- Verified by: Automated continuous integration checks executing workspace type verification and contract test suites.
- Verified by: Architectural pull request reviews enforcing module boundary compliance and prohibiting ad-hoc contract declarations.
- Violation handling: Pull requests with manual contract declarations or unmapped procedure errors are blocked from merging until corrected.
- Exception process: Submit an architectural exception request detailing runtime constraints to the principal engineering team for formal sign-off.