---
version: 2
name: 0585fd27-d6b3-42f5-9e56-255bafb94c77-adoption-of-hookform-resolvers-zod-for-schema-driven-form-validation-client-form-components-not-implement-custom
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Not Implement Custom"
metadata:
  actual:
    adr_id: 0585fd27-d6b3-42f5-9e56-255bafb94c77
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Not Implement Custom

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- The dashboard interface features numerous interactive forms handling configuration, notifications, settings, and monitoring operations.
- User input requires robust client-side validation, prompt field-level feedback, and type safety before submission.
- A consistent integration layer is necessary to bridge form state management controllers with declarative schema validation rules.

## Problem Statement

Managing form state and input validation across diverse user interface components without a standardized adapter leads to disparate validation mechanisms, duplicated validation rules, inconsistent error states, and heightened risk of runtime validation discrepancies.

## Decision

1. MUST_NOT: Client form components MUST NOT implement custom manual validation routines when input rules can be represented via schema validation resolvers.

## Policy Block

- MUST_NOT Client form components MUST NOT implement custom manual validation routines when input rules can be represented via schema validation resolvers.

In scope:
- All interactive client-side components capturing user input within form structures.
- Dialogs, settings panels, and notification configuration interfaces requiring field-level data validation.

Out of scope:
- Server-side data mutation routines and background jobs operating outside user interface rendering trees.
- Read-only visualization components and dashboards that do not collect user input.

Exceptions:
- EXC-20-001: Trivial single-action forms interacting with unvalidated third-party widget embeds that manage their own state.

## Rationale

- Adopting @hookform/resolvers/zod establishes a single, reliable integration mechanism between form controllers and validation schemas across 50 observed components.
- Centralizing validation within schemas decoupled from UI presentation ensures input constraints are reusable and consistently enforced.
- Automatic error propagation from schemas to form states minimizes boilerplate code and mitigates manual error-handling oversights.

## Consequences

Positive:
- Standardizes client-side form validation across all dashboard user interface components.
- Eliminates repetitive manual validation checks by binding form controls directly to declarative schemas.
- Guarantees compile-time type synchronization between form field definitions and submission payloads.
- Provides automated mapping of schema validation issues directly to individual form field error states.

Negative:
- Creates a dependency bridge that requires schema versions and resolver adapter versions to remain mutually compatible.
- Increases the client JavaScript bundle size by incorporating resolver bridge code alongside schema parsing libraries.
- Tightens the coupling between UI form state management and the specific schema definition syntax.

## Alternatives

- Manual event-driven input validation using custom component state and handlers (rejected)
  Rejected because: Increases repetitive boilerplate across forms, fragments validation logic, and introduces inconsistency in error presentation.
  When valid: When implementing isolated, single-field inputs that have zero dependency on application-wide domain schemas.
- Alternative resolver adapters for separate third-party schema definition libraries (rejected)
  Rejected because: Diverges from the standardized schema validation definitions already established across the application workspace.
  When valid: When replacing the core domain schema validation library across all project packages.

## Risks

- Version mismatch between the resolver adapter and the underlying schema library could cause validation discrepancies or runtime build errors.
  Mitigation: Enforce lock-file version grounding to confirm version compatibility between the resolver package and schema libraries.
  Owner: Frontend Engineering Team
- Complex nested schemas evaluated synchronously on every input change could impair form responsiveness.
  Mitigation: Profile deeply nested schemas and configure deferred or blurred validation triggers rather than continuous re-evaluation.
  Owner: Frontend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure the form controller hook by passing zodResolver initialized with the target schema into the resolver configuration property.
- Ensure custom error messages defined within schemas conform to user experience design guidelines for field-level feedback.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting tasks to verify that form implementations adhere to resolver import standards.
- Discover and execute the workspace type-checking command to ensure form controller types match their corresponding schema definitions.
- Discover and run automated test suites covering form component validation and error handling behavior.

Accept when:
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

## Enforcement

- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Repository type-checking tasks confirming schema and resolver alignment.
- Verified by: Peer code review focusing on form component implementation standards.
- Violation handling: Pull requests containing manual validation logic in place of the standard resolver MUST be blocked.
- Violation handling: Flagged components MUST be refactored to employ the standard schema resolver prior to approval.
- Violation handling: Deviations without documented architecture approval MUST be rejected during review.
- Exception process: Submit an exception proposal detailing why declarative schema validation is unfeasible for the specific form interface.
- Exception process: Obtain formal review and sign-off from the frontend architectural lead prior to merge.
- Exception process: Document the approved exemption and technical rationale alongside the component source code.