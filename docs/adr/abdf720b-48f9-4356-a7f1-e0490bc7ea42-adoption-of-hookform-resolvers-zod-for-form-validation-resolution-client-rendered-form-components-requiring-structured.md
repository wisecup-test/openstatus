---
version: 2
name: abdf720b-48f9-4356-a7f1-e0490bc7ea42-adoption-of-hookform-resolvers-zod-for-form-validation-resolution-client-rendered-form-components-requiring-structured
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Client Rendered Form Components Requiring Structured"
metadata:
  actual:
    adr_id: abdf720b-48f9-4356-a7f1-e0490bc7ea42
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Client Rendered Form Components Requiring Structured

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/forms/**`

## Context

- Interactive form components within client application workflows require consistent input validation to ensure data integrity prior to mutation execution.
- Multiple client forms across the application incorporate schema definitions alongside form component state primitives, creating a need for a uniform validation bridge.
- The @hookform/resolvers/zod package provides a standardized integration adapter that translates declarative schema constraints into form controller validation state.

## Problem Statement

Without a standardized resolver integration, form state controllers require manual error mapping or disparate validation logic across client form components, leading to inconsistent validation behavior, increased boilerplate code, and potential discrepancies between schema specifications and rendered field errors.

## Decision

1. MUST: All client-rendered form components requiring structured input validation MUST integrate @hookform/resolvers/zod to resolve validation schemas within form controller definitions.

## Policy Block

- MUST All client-rendered form components requiring structured input validation MUST integrate @hookform/resolvers/zod to resolve validation schemas within form controller definitions.

In scope:
- Interactive client-rendered form components that manage user input state and require declarative schema validation.
- Form workflows requiring pre-submission client-side validation and structured field error rendering.

Out of scope:
- Server-rendered static presentation pages with no interactive form state or client input handling.
- Non-form data transformations or background processing components operating outside interactive UI workflows.

## Rationale

- Adopting @hookform/resolvers/zod establishes a unified contract for client-side form validation across all interactive form modules.
- Connecting schema definitions directly to form hooks eliminates manual error-handling logic and ensures type-safe schema synchronization.
- The pattern is substantiated by recurring evidence across client form implementations within the repository, ensuring architectural consistency.

## Consequences

Positive:
- Guarantees unified and declarative schema validation across all client-side form components.
- Reduces repetitive manual validation logic and minimizes error-handling boilerplate in form controllers.
- Ensures validation error structures remain synchronized with component field definitions.

Negative:
- Introduces an architectural dependency on the external resolver adapter package between the schema library and form controllers.
- Increases client bundle size slightly by including the resolver integration layer alongside validation logic.

## Alternatives

- Manual validation error mapping without an external resolver package (rejected)
  Rejected because: Requires authoring custom validation routines and manual state binding for every form component, increasing code duplication and bug risk.
  When valid: Acceptable only in minimal forms with zero external schema libraries and single-field validation requirements.
- Server-only validation with zero client-side form schema resolution (rejected)
  Rejected because: Degrades user experience by delaying form feedback until full network roundtrips complete for simple constraint checks.
  When valid: Acceptable for workflows where validation rules cannot be executed in the client environment due to sensitive data constraints.

## Risks

- API drift or schema incompatibility across major version updates of the underlying validation library or form framework.
  Mitigation: Adhere to the mandatory lock-version grounding process before updating dependencies, confirming API compatibility via official release documentation.
  Owner: Frontend engineering team
- Divergence between client-side resolver schemas and downstream server validation requirements.
  Mitigation: Share or synchronize validation schema definitions between client form modules and API boundary contracts.
  Owner: Full-stack engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure form state controllers by passing the resolver instance initialized with the component validation schema during form initialization.
- Ensure all interactive form inputs connect their registered field handlers and error states directly to the form controller managed by the resolver.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type checking suite to verify resolver integration across form components.
- Discover and execute the project automated test suite covering interactive form components and validation workflows.

Accept when:
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

## Enforcement

- Verified by: Continuous integration pipeline static analysis and automated test verification suites.
- Verified by: Peer code review verification for all new and modified client form components.
- Violation handling: Pull requests introducing manual validation logic or bypassing the schema resolver in form components must be blocked until compliant.
- Exception process: Exceptions require documented architectural justification and formal approval by the frontend architecture lead.