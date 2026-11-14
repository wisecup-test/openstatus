---
version: 2
name: 46dddcf8-84af-4353-a018-2416f66568bc-zod-schema-validation-for-form-and-component-input-boundaries-form-component-types-inferred-directly-zod
description: "Zod Schema Validation for Form and Component Input Boundaries: Form Component Types Inferred Directly Zod"
metadata:
  actual:
    adr_id: 46dddcf8-84af-4353-a018-2416f66568bc
    status: accepted
    candidate_group_id: df6eea8a-3ab7-4843-bbef-10688bb78119
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/**
      - src/scripts/**
---

# Zod Schema Validation for Form and Component Input Boundaries: Form Component Types Inferred Directly Zod

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`
- `src/components/**`
- `src/lib/**`
- `src/scripts/**`

## Context

- Client-facing applications ingest user-generated input across diverse forms, settings panels, notification configurations, and parameter caches.
- Unvalidated or loosely validated input creates risks of malformed payloads, runtime script exceptions, and inconsistent boundary behavior.
- Static analysis across 70 files demonstrates pervasive adoption of Zod runtime schemas integrated with form resolvers and safe parsing handlers to enforce structural validation at component boundaries.

## Problem Statement

Client-side input boundaries without strict runtime validation expose the application to invalid state mutations, submission of malformed payloads to backend APIs, and inconsistent error presentation across user interfaces.

## Decision

1. SHOULD: Form component types SHOULD be inferred directly from Zod schemas using schema type inference utilities to maintain synchronization between runtime validation and static interface definitions.

## Policy Block

- SHOULD Form component types SHOULD be inferred directly from Zod schemas using schema type inference utilities to maintain synchronization between runtime validation and static interface definitions.

In scope:
- All user-facing dashboard forms, input dialogs, and configuration panels capturing user input.
- Deserialization and hydration boundaries for client-persisted session data and search parameter caches.

Out of scope:
- Static presentational components that do not capture or mutate user input.
- Internal component property passing between trusted, statically typed parent and child components.

Exceptions:
- EXC-42-001: Canvas or WebGL interaction layers that process high-frequency raw pointer coordinates without form state.

## Rationale

- Zod provides declarative runtime validation with automatic TypeScript type inference, guaranteeing that form values match expected domain constraints both at runtime and at compile time.
- Binding schemas directly to form resolvers decouples UI presentation from validation logic while ensuring uniform validation lifecycle execution.
- Static evidence confirms a consistent pattern across 70 files where form inputs and deserialized payloads are guarded by explicit Zod schemas and safe parsing routines.

## Consequences

Positive:
- Eliminates malformed and unexpected user input before client state mutation or network dispatch.
- Enforces strict type safety and compile-time synchronization between input schemas and form data structures via schema inference.
- Standardizes error handling and validation messaging across all user-facing dashboard components.

Negative:
- Schema definitions and custom refinement logic increase client bundle size across form components.
- Deeply nested schema parsing on rapid form state updates can introduce minor client runtime overhead if not scoped outside render loops.

## Alternatives

- Manual Imperative Input Validation (rejected)
  Rejected because: Manual validation requires bespoke conditional checks per input field, increasing maintenance overhead, causing inconsistent error messaging, and lacking automated type inference.
  When valid: Valid only in isolated micro-components with single primitive inputs where external schema libraries are explicitly prohibited.
- Alternative Schema Libraries (e.g., Yup or Joi) (rejected)
  Rejected because: The codebase has standardized on Zod for static TypeScript type inference and form resolver compatibility; introducing alternative schema engines would fragment validation conventions and increase client bundle size.
  When valid: Valid when integrating with legacy third-party form modules that strictly require alternative validation resolvers.

## Risks

- Complex schema validation graphs and deeply nested refinements may impact client input responsiveness during frequent re-renders.
  Mitigation: Define schemas outside component render cycles and restrict computationally expensive refinement logic to form submission boundaries.
  Owner: Frontend Engineering Team
- Schema divergence between client-side form validation and backend endpoint contracts can cause unexpected submission rejections.
  Mitigation: Share domain constants and schema definitions through shared workspace packages to keep client and server validation rules synchronized.
  Owner: Fullstack Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Instantiate validation schemas at module scope rather than within component render functions to prevent unnecessary schema re-allocation on re-renders.
- Compose complex object schemas hierarchically using shared sub-schemas and domain constants to preserve cross-form consistency.

## Continuation Context


Verify commands:
- Discover and execute the project static analysis scripts to verify that all form components bind corresponding Zod schemas without type or lint errors.
- Discover and execute the test runner suite to validate that input schemas reject malformed values and accept compliant payloads across component test fixtures.

Accept when:
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

## Enforcement

- Verified by: Automated continuous integration checks executing static analysis and type verification.
- Verified by: Peer code review verifying that all new form inputs and external data decoders incorporate corresponding Zod schemas.
- Violation handling: Pull requests containing unvalidated form inputs or bypassed resolvers must be blocked until compliant schemas are implemented.
- Exception process: Exceptions require an architectural review detailing why declarative schema validation cannot be applied and documenting the alternate input sanitization mechanism.