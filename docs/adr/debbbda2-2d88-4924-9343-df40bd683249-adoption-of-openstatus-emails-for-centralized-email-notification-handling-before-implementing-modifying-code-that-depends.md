---
version: 2
name: debbbda2-2d88-4924-9343-df40bd683249-adoption-of-openstatus-emails-for-centralized-email-notification-handling-before-implementing-modifying-code-that-depends
description: "Adoption of @openstatus/emails for Centralized Email Notification Handling: Before Implementing Modifying Code That Depends"
metadata:
  actual:
    adr_id: debbbda2-2d88-4924-9343-df40bd683249
    status: accepted
    candidate_group_id: 83171de2-78b9-4015-98c5-6b0874312f42
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails for Centralized Email Notification Handling: Before Implementing Modifying Code That Depends

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/internal/email/**`

## Context

- Internal API routes trigger transactional email workflows for events such as team invitations and feedback submissions.
- Multiple internal routes require consistent email structuring, styling, and delivery abstractions across the application boundary.
- The codebase provides a dedicated workspace package to encapsulate email templates and rendering logic away from endpoint handlers.

## Problem Statement

Scattering email template formatting and layout logic across disparate internal API routes creates duplicated markup, inconsistent styling, and tight coupling between HTTP routing layers and notification delivery mechanisms.

## Decision

1. MUST: Before implementing or modifying code that depends on @openstatus/emails, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API compatibility against that version.

## Policy Block

- MUST Before implementing or modifying code that depends on @openstatus/emails, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API compatibility against that version.

In scope:
- All internal server route handlers and endpoints responsible for creating, formatting, or dispatching transactional emails.

Out of scope:
- Outbound webhooks and external notification channels that do not involve transactional email delivery.

## Rationale

- Adopting a centralized email library module ensures all transactional emails share identical styling guidelines and component foundations.
- Centralization reduces maintenance costs when updating branding or underlying delivery providers.
- Decoupling template compilation from API route logic facilitates isolated unit testing and previewing of email templates.

## Consequences

Positive:
- Ensures uniform branding, layout, and rendering behavior across all internal transactional email triggers.
- Isolates email template generation logic from internal HTTP routing and request processing.
- Enables shared email components to be maintained and tested independently of web server endpoints.

Negative:
- Changes to email template contracts require coordinated updates across the package boundary and consuming route handlers.
- Local testing of internal endpoints requires linking or building the shared workspace email package.

## Alternatives

- Inline template authoring directly within route handlers (rejected)
  Rejected because: Causes template logic duplication across multiple internal endpoints, weakens design consistency, and increases testing overhead.
  When valid: Only acceptable in rapid disposable prototypes where template reuse and consistency are not required.
- Direct third-party email provider SDK integration inside every route handler (rejected)
  Rejected because: Tightly couples each internal endpoint to external vendor delivery APIs and leaks vendor-specific template formats across the application.
  When valid: Valid when an endpoint has unique delivery requirements completely unsupported by internal packages.

## Risks

- Breaking contract changes in shared email components can cause runtime rendering failures in dependent route handlers.
  Mitigation: Maintain strict semantic versioning and typed interfaces for template components exported by the email package.
  Owner: engineering team
- Over-centralization could slow down template development if route authors cannot easily preview changes locally.
  Mitigation: Provide local mock renderers or test harnesses within the shared package to allow rapid template verification.
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
- Expose typed parameter interfaces from the email package so consuming route handlers receive compile-time verification for all required template variables.
- Decouple email rendering from HTTP response generation to maintain clean boundaries within internal route handlers.

## Continuation Context


Verify commands:
- Discover and execute the project verification script declared in the repository manifest to validate module dependency boundaries and type contracts.
- Run the automated test runner discovered from the build configuration to verify internal notification route execution against email contracts.

Accept when:
- All internal route handlers generating notifications compile cleanly against the shared email module without type or module resolution errors.
- Automated test suites for internal endpoints execute successfully and verify email generation contracts.
- Static dependency analysis confirms that notification handlers import email templates exclusively through the designated internal package.

## Enforcement

- Verified by: Automated module import lint rules and dependency boundary analysis within continuous integration.
- Verified by: Mandatory peer review on pull requests touching internal notification route handlers.
- Violation handling: Continuous integration checks fail when unauthorized email formatting or untracked email imports are detected in route handlers.
- Violation handling: Pull requests violating email package encapsulation are blocked until refactored to consume the shared package.
- Exception process: Submit an architectural variance request detailing why standard email package templates cannot accommodate the notification format.
- Exception process: Obtain sign-off from the core architecture team prior to merging inline formatting or alternate dispatch implementations.
- Exception process: Track all approved exceptions in the repository decision register with a scheduled review date.