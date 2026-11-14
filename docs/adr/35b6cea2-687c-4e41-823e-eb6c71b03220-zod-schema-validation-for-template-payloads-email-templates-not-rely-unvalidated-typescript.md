---
version: 2
name: 35b6cea2-687c-4e41-823e-eb6c71b03220-zod-schema-validation-for-template-payloads-email-templates-not-rely-unvalidated-typescript
description: "Zod Schema Validation for Template Payloads: Email Templates Not Rely Unvalidated Typescript"
metadata:
  actual:
    adr_id: 35b6cea2-687c-4e41-823e-eb6c71b03220
    status: accepted
    candidate_group_id: 6f7dcbfa-f547-40d6-b58a-ed827d7689d0
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - emails/**
      - src/**
---

# Zod Schema Validation for Template Payloads: Email Templates Not Rely Unvalidated Typescript

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/emails`

Applies to:
- `emails/**`
- `src/**`

## Context

- External input passed into email templates originates from dynamic application events and external triggers, presenting risks of malformed or unexpected data shapes during rendering.
- Compile-time type assertions do not provide runtime boundary defense when handling serialized JSON or event payload inputs.
- Schema-based validation with Zod provides deterministic parsing and coercion at execution time, preventing template failures caused by undefined or improperly typed properties.

## Problem Statement

Dynamic event payloads passed to transactional email templates and runtime environment variables can introduce invalid, unexpected, or unescaped values at execution time if boundary checking relies solely on static types. Without runtime schema enforcement, malformed payloads cause rendering exceptions or generate corrupted communications sent to external recipients.

## Decision

1. MUST_NOT: Email templates MUST NOT rely on unvalidated TypeScript interfaces alone for runtime boundaries where external or untrusted data is ingested.

## Policy Block

- MUST_NOT Email templates MUST NOT rely on unvalidated TypeScript interfaces alone for runtime boundaries where external or untrusted data is ingested.

In scope:
- Transactional email template components receiving dynamic runtime props.
- Runtime configuration modules parsing environment variables.

Out of scope:
- Internal static UI components that do not accept external event payload data.
- Internal utility helper functions operating purely on pre-validated domain types.

## Rationale

- Adopting Zod schemas creates a single source of truth for both static TypeScript types and runtime validation logic.
- Validating payloads at the component boundary ensures that malformed data fails immediately with clear validation errors before rendering or sending emails.
- Strict primitive constraints including string enums and URL validators eliminate downstream rendering vulnerabilities caused by unexpected input structures.

## Consequences

Positive:
- Runtime payload failures are caught at the entry boundary before email compilation or transmission.
- Component prop types remain synchronized with runtime validation schemas through inferred types.
- Template rendering components operate on verified shapes, reducing defensive conditional branching across markup.

Negative:
- Adding schema parsing introduces a minor runtime execution overhead during template instantiation.
- Schema definitions require ongoing synchronization when underlying domain data models evolve.

## Alternatives

- Compile-time only TypeScript type annotations (rejected)
  Rejected because: TypeScript types are erased at runtime and offer no protection against malformed external payloads or invalid environment variables.
  When valid: Valid only in fully internal contexts where data provenance is guaranteed by memory-safe internal callers.
- Manual imperative validation functions per template (rejected)
  Rejected because: Manual property checks are repetitive, error-prone, and do not provide automatic static type inference.
  When valid: Valid in micro-runtimes where bundle size constraints strictly prohibit validation library dependencies.

## Risks

- Validation failure at runtime may prevent critical alert or notification emails from sending if payload schema drifts from producer payload.
  Mitigation: Maintain schema compatibility through integration tests and define optional fallback fields for non-critical notification data.
  Owner: engineering team
- Schema divergence across distributed producers and consumers leading to validation rejection of valid operational events.
  Mitigation: Centralize shared schema definitions or establish contract tests between event emitters and template consumers.
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
- Define template schemas adjacent to their respective components and infer component prop types directly from schema definitions to maintain consistency.
- Use strict primitive validators and coercion helpers where incoming values originate from string-serialized environments.

## Continuation Context


Verify commands:
- Discover and run the repository type-check script to verify that all template component props align with schema definitions.
- Discover and run the repository test suite to validate that schemas reject invalid payload structures.

Accept when:
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

## Enforcement

- Verified by: Continuous integration type checking and automated test pipelines.
- Verified by: Peer review on pull requests touching template components and schema definitions.
- Violation handling: Pull requests lacking runtime schema validation for newly introduced external template inputs must be rejected during review.
- Violation handling: Continuous integration failures on schema mismatch will block merges to main branch.
- Exception process: Exceptions require engineering team consensus and documented architectural rationale explaining how payload safety is guaranteed without runtime schemas.