---
version: 2
name: 94f7b8c2-aee4-4b40-940d-e8b7253da61f-adoption-of-trpc-client-for-type-safe-api-client-communication-application-use-trpc-client-primary-library
description: "Adoption of @trpc/client for Type-Safe API Client Communication: Application Use Trpc Client Primary Library"
metadata:
  actual:
    adr_id: 94f7b8c2-aee4-4b40-940d-e8b7253da61f
    status: accepted
    candidate_group_id: 14b97067-d036-4c05-b4e7-cb525f0cce77
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/forms/**
      - src/lib/trpc/**
---

# Adoption of @trpc/client for Type-Safe API Client Communication: Application Use Trpc Client Primary Library

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/components/forms/**`
- `src/lib/trpc/**`

## Context

- Presentation layers and server action handlers require reliable, type-safe data fetching and mutation mechanisms to interact with backend services.
- Manually writing fetch requests and synchronizing API response types across client and server modules introduces runtime validation drift and maintenance overhead.
- Integrating a centralized procedure call client unifies error handling, serialization, and telemetry instrumentation across both client and server rendering boundaries.

## Problem Statement

Managing client-server data synchronization through untyped or loosely typed network requests leads to interface mismatch errors, duplicate type declarations, and inconsistent error-handling workflows across UI forms and page views. The application requires a unified, compile-time verified client architecture to execute remote procedure calls seamlessly across both client-rendered components and server-rendered contexts without manual contract maintenance.

## Decision

1. MUST: The application MUST use @trpc/client as the primary client library for executing type-safe queries, mutations, and remote procedure calls between the presentation layer and the backend service definition.

## Policy Block

- MUST The application MUST use @trpc/client as the primary client library for executing type-safe queries, mutations, and remote procedure calls between the presentation layer and the backend service definition.

In scope:
- All client components, UI forms, page views, and server action handlers requiring remote procedure invocation or data synchronization with backend services.

Out of scope:
- Third-party webhooks, external identity callbacks, and public static asset retrieval that do not interact with internal application procedures.

## Rationale

- Import evidence across seven application modules establishes @trpc/client as the standard mechanism for client-server procedure execution.
- Centralizing on @trpc/client guarantees compile-time type safety across client queries, mutations, and server actions, catching breaking contract changes during build verification.
- Shared link configuration and serialization adapters eliminate duplicate transformation logic and streamline telemetry and error tracking.

## Consequences

Positive:
- Provides end-to-end type safety between backend procedures and frontend consumers without requiring code generation steps.
- Standardizes query and mutation lifecycle states across user interface components.
- Centralizes serialization, credential propagation, and error observation across network boundaries.

Negative:
- Tightly couples client build artifacts to the backend API procedure definitions.
- Increases client bundle weight compared to primitive HTTP fetch operations.
- Requires client and server modules to coordinate shared transformer dependencies and runtime configurations.

## Alternatives

- Custom REST API fetch wrappers with manually defined TypeScript interfaces (rejected)
  Rejected because: Manually maintained types drift from backend implementation changes over time and fail to enforce compile-time verification across boundaries.
  When valid: Valid when interacting with third-party external services outside the application codebase.
- GraphQL client adoption with schema code generation (rejected)
  Rejected because: Introduces significant build-step tooling complexity and schema maintenance overhead for an integrated full-stack TypeScript architecture.
  When valid: Valid in multi-language or polyglot environments where disparate external services consume a shared federated graph.

## Risks

- Server-only authentication tokens or request context headers may leak into client bundles if boundary isolation is breached.
  Mitigation: Enforce explicit environment separation boundaries and verify module imports through automated boundary check scripts.
  Owner: Full-Stack Engineering Team
- Mismatched dependency versions between client utilities and shared API packages may cause runtime serialization failures.
  Mitigation: Follow the Lock-Version Grounding policy by inspecting repository lock artifacts prior to updating dependency specifications.
  Owner: Full-Stack Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure client initialization with unified transformer serialization and link configurations that handle authentication cookies and error logging uniformly across client and server execution contexts.
- Separate server-only procedure callers from client-side hooks to ensure environment boundary markers prevent server logic from being bundled into client assets.

## Continuation Context


Verify commands:
- Discover the workspace dependency manifest and execute the project verification script to ensure all client and server module types align.
- Discover the repository build script and run it to verify that client bundles do not contain prohibited server-only boundary imports.
- Discover and run the project test script to ensure all query and mutation hooks execute as expected.

Accept when:
- Type verification completes with zero errors across all client queries, mutations, and server action procedure callers.
- Application build completes successfully without bundle boundary violations or missing module exports.
- All automated tests covering data fetching and mutation workflows pass successfully.

## Enforcement

- Verified by: Continuous integration type checking and build verification steps.
- Verified by: Automated dependency boundary audits during pull request reviews.
- Verified by: Peer review of newly added data fetching hooks and server actions.
- Violation handling: Pull requests containing untyped fetch calls for internal procedures or boundary violations are blocked by automated continuous integration gates.
- Violation handling: Violations identified during code review must be refactored to use the standardized client before approval.
- Exception process: Exceptions for external third-party integrations must be documented in the corresponding module and approved by an architecture lead.