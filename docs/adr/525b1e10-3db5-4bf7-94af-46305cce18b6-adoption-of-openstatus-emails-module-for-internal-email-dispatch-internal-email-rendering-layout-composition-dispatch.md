---
version: 2
name: 525b1e10-3db5-4bf7-94af-46305cce18b6-adoption-of-openstatus-emails-module-for-internal-email-dispatch-internal-email-rendering-layout-composition-dispatch
description: "Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Rendering Layout Composition Dispatch"
metadata:
  actual:
    adr_id: 525b1e10-3db5-4bf7-94af-46305cce18b6
    status: accepted
    candidate_group_id: e7fd5b3d-c86c-4a8d-8a14-d0b1f3338248
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/internal/email/**
---

# Adoption of @openstatus/emails Module for Internal Email Dispatch: Internal Email Rendering Layout Composition Dispatch

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/api/internal/email/**`

## Context

- Internal API endpoints orchestrate automated notifications including team invitations, user feedback, and general email dispatch.
- Transactional email composition requires consistent layout, branding, and payload structure across distinct dispatch triggers.
- The codebase organizes email presentation logic within an internal module boundary to decouple rendering from HTTP request routing.

## Problem Statement

Transactional email composition scattered across disparate HTTP endpoint handlers leads to inconsistent styling, duplicated payload structures, and tight coupling between transport logic and template rendering.

## Decision

1. MUST: All internal email rendering, layout composition, and dispatch execution within server endpoints MUST use the @openstatus/emails library rather than inline markup or external notification clients.

## Policy Block

- MUST All internal email rendering, layout composition, and dispatch execution within server endpoints MUST use the @openstatus/emails library rather than inline markup or external notification clients.

In scope:
- Internal API route handlers responsible for transactional email creation and dispatch.
- Backend server endpoints triggering team invitations, customer feedback notifications, and system emails.

Out of scope:
- Client-side user interface components executing in browser runtime environments.
- Publicly accessible web endpoints that do not process transactional notifications.

## Rationale

- Static analysis demonstrates uniform adoption of @openstatus/emails across all internal email route endpoints, establishing an intentional architectural boundary for notification logic.
- Isolating email rendering inside a dedicated internal library ensures presentation templates remain reusable and testable independently of HTTP server route lifecycles.
- Coordinating email dispatch with database schemas and secret-authenticated route handlers keeps system-level notification operations centralized and secure.

## Consequences

Positive:
- Centralizes email markup, design consistency, and dispatch orchestration within a single reusable module.
- Decouples HTTP transport handling and route authentication from email rendering logic.
- Enables consistent type definitions across email payload schemas and consumer route handlers.

Negative:
- Requires synchronized cross-package builds and coordination when email template interfaces change.
- Adds module boundary abstraction overhead for simple or single-use notification messages.

## Alternatives

- Inline HTML template construction directly within individual route handlers (rejected)
  Rejected because: Scatters presentation markup across transport endpoints, resulting in maintenance divergence and duplicate notification logic.
  When valid: Rapid one-off prototyping where centralized template reuse is explicitly not required.
- Direct third-party email service provider SDK invocation in route handlers without an internal library abstraction (rejected)
  Rejected because: Tightly couples individual endpoint handlers to external provider APIs and prevents shared local template rendering.
  When valid: Simple applications lacking shared branding or centralized template lifecycles.

## Risks

- Breaking interface changes in @openstatus/emails can cause runtime failures across dependent internal email dispatch endpoints.
  Mitigation: Enforce automated workspace type checking to catch breaking contract updates across package boundaries during continuous integration.
  Owner: Engineering Team
- Schema divergence between recipient database records and email template properties leading to delivery errors.
  Mitigation: Establish end-to-end integration tests validating that database entity records map accurately to email dispatch parameters.
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
- Import email templates and delivery utilities directly from @openstatus/emails at the boundary of internal email dispatch endpoints.
- Ensure authorization verification executes prior to invoking email template rendering or delivery procedures.

## Continuation Context


Verify commands:
- Discover and execute the project verification task for the email module and consuming web route handlers.
- Discover and execute the workspace type check and test suites covering internal email API routes.

Accept when:
- All internal email route handlers import email generation and dispatch routines exclusively from the @openstatus/emails library.
- Workspace type checking and project verification test suites pass without errors across all internal email dispatch endpoints.

## Enforcement

- Verified by: Automated workspace type checking and verification suites in continuous integration pipelines.
- Verified by: Peer code reviews during pull request inspection verifying that route handlers do not construct raw email payloads.
- Violation handling: Pull requests containing direct inline email markup or bypassing @openstatus/emails will be rejected during review.
- Violation handling: Contributors will be required to refactor templates into the shared email module before merge approval.
- Exception process: Exceptions must be submitted via architectural review with documented justification for why the internal email library cannot fulfill the notification requirement.