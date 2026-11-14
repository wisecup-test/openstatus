---
version: 2
name: db449682-a2ba-4c8f-90ad-e65b4d54ed1b-adoption-of-react-email-for-transactional-email-composition-email-rendering-components-rely-compatible-style
description: "Adoption of react-email for Transactional Email Composition: Email Rendering Components Rely Compatible Style"
metadata:
  actual:
    adr_id: db449682-a2ba-4c8f-90ad-e65b4d54ed1b
    status: accepted
    candidate_group_id: b6d4a005-7d3f-4d2d-974c-51b7961ef7bc
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - emails/**
      - src/**
---

# Adoption of react-email for Transactional Email Composition: Email Rendering Components Rely Compatible Style

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/emails`

Applies to:
- `emails/**`
- `src/**`

## Context

- Transactional email generation requires client-compatible HTML structures and inlined styling to render reliably across heterogeneous mail user agents.
- Notification templates must be organized within a dedicated email package utilizing modular component abstractions rather than disparate string interpolations.
- Common structural elements including headers, footers, and brand styles must be reused across varied notification event templates.
- Notification systems require rigorous contract validation for template payloads before invoking email transmission clients.

## Problem Statement

Transactional email authoring without unified component abstractions leads to fragmented styling, brittle string-based HTML templating, and unpredictable rendering across heterogeneous email clients. The architecture requires a standardized, type-safe templating system that allows developers to compose declarative notification emails using modular layouts and validated payloads.

## Decision

1. MUST: Email rendering components MUST rely on email-compatible style primitives provided or supported by react-email rather than browser-specific layout abstractions.

## Policy Block

- MUST Email rendering components MUST rely on email-compatible style primitives provided or supported by react-email rather than browser-specific layout abstractions.

In scope:
- Authoring, structuring, and maintaining transactional email templates.
- Rendering component trees into email markup for outbound notification services.

Out of scope:
- Browser-based client application web user interfaces.
- Plaintext-only logging, console reporting, and internal text notifications.

## Rationale

- Evidence across notification templates demonstrates consistent adoption of react-email for declarative email rendering and component reusability.
- Adopting a dedicated React-based email abstraction mitigates cross-client rendering inconsistencies by producing standardized, compliant HTML tables and inlined styles.
- Component composition facilitates strict separation between layout framing, template content, and data contract validation.

## Consequences

Positive:
- Establishes a unified, declarative component architecture for transactional email composition across the system.
- Maximizes design consistency and eliminates markup duplication through reusable layout and styling primitives.
- Enhances reliability and contract enforcement by integrating component rendering with property schema validation.

Negative:
- Introduces dependency coupling to the component rendering lifecycle and library updates for email generation.
- Constrains layout and styling options to email-safe component primitives rather than full modern browser specifications.
- Incurs compilation and HTML string transformation overhead prior to message transmission.

## Alternatives

- Manual raw HTML and inline styling string templates (rejected)
  Rejected because: Manual string formatting lacks type safety, increases risk of malformed markup, and incurs high maintenance overhead across disparate email rendering clients.
  When valid: Valid only in minimal runtime environments lacking build-time compilation or component execution capabilities.
- External hosted visual template management services (rejected)
  Rejected because: Decouples template version control from application source code, complicates automated integration testing, and creates external operational dependencies.
  When valid: Valid when non-technical stakeholders must independently author and publish marketing campaigns without engineering deployment cycles.

## Risks

- Email client engine idiosyncrasies may cause unexpected layout rendering anomalies despite component-level abstractions.
  Mitigation: Validate rendered template output across major client engines and maintain shared layout wrappers that isolate rendering workarounds.
  Owner: engineering team
- Upstream breaking changes or dependency drift in rendering primitives across library updates could affect rendered HTML.
  Mitigation: Follow the lock-version grounding policy and perform automated snapshot and regression checks on rendered markup before updating dependencies.
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
- Structure each email notification template as a modular component consuming strongly typed property contracts.
- Centralize visual tokens, container sizing, and typography primitives in shared styling modules to ensure brand uniformity across templates.

## Continuation Context


Verify commands:
- Discover and run the project type checking script from the dependency manifest.
- Discover and run the project test script to validate template rendering and schema integrity.
- Discover and run the project linter to ensure component styling and structure conform to repository standards.

Accept when:
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

## Enforcement

- Verified by: Continuous integration pipelines running automated type verification, linting, and template test suites.
- Verified by: Peer code review ensuring that new templates adhere to component composition and layout standards.
- Violation handling: Pull requests introducing raw string-based HTML templates or bypassing component primitives fail continuous integration checks.
- Violation handling: Violations must be refactored to consume approved component primitives before pull requests can be merged.
- Exception process: Submit an architectural review request describing technical constraints preventing the use of standard component primitives.
- Exception process: Exceptions require formal review and approval by the engineering team before bypassing standard template pipelines.