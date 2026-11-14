---
version: 2
name: 4aaf8c64-b0e2-4e3b-bd6d-774553718aec-adoption-of-openstatus-ui-components-ui-input-group-for-composite-input-fields-before-integrating-updating-dependencies-internal-external
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Before Integrating Updating Dependencies Internal External"
metadata:
  actual:
    adr_id: 4aaf8c64-b0e2-4e3b-bd6d-774553718aec
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Before Integrating Updating Dependencies Internal External

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- Dashboard feature components require consistent visual presentation and interaction behavior for text fields combined with action buttons, icons, or prefixes.
- Multiple independent feature domains across the application interface independently construct composite input controls for onboarding flows, domain configuration, general settings, and conversational prompts.
- Importing standardized composite component modules from the shared internal component package establishes structural consistency across user interface boundaries.

## Problem Statement

Constructing composite input fields with ad-hoc styling and disparate structural layouts across dashboard forms leads to visual drift, duplicated container markup, and fragmented accessibility semantics.

## Decision

1. MUST: Before integrating or updating dependencies from internal or external package namespaces, developers MUST discover the repository dependency manifest and resolution artifact to verify the locked version specifications.

## Policy Block

- MUST Before integrating or updating dependencies from internal or external package namespaces, developers MUST discover the repository dependency manifest and resolution artifact to verify the locked version specifications.

In scope:
- Client-rendered interactive user interface components requiring grouped input fields, action buttons, or inline adornments.
- Form interfaces and interactive prompt inputs within the dashboard application boundary.

Out of scope:
- Non-interactive text displays and purely read-only data representations.
- Standalone single-element inputs that do not incorporate prefixes, suffixes, or integrated action buttons.

## Rationale

- Four distinct dashboard component modules across varied domains demonstrate unified reliance on the shared input group module, indicating an intentional structural standard.
- Centralizing composite input container logic in the internal component library reduces maintenance overhead and guarantees uniform spacing, focus states, and border styling.
- Standard module encapsulation allows future styling or accessibility enhancements to propagate immediately across all consuming feature forms.

## Consequences

Positive:
- Uniform visual appearance and accessibility semantics across all dashboard input groupings.
- Elimination of redundant layout styling and wrapper containers in individual form components.
- Simplified maintenance when updating input border, focus ring, or padding tokens across feature surfaces.

Negative:
- Creates direct coupling between client form features and the internal design system component API structure.
- Custom input layouts that deviate from the standard grouped pattern require explicit extension or adaptation within the shared module.

## Alternatives

- Ad-hoc utility class wrappers defined individually in each form component (rejected)
  Rejected because: Results in visual design divergence, inconsistent focus states, and widespread container markup duplication across feature forms
  When valid: Prototyping an experimental input layout prior to standardization in the shared component library
- Third-party headless input composite primitives imported directly into feature modules (rejected)
  Rejected because: Bypasses internal design system theme tokens and fragments dependency management across client components
  When valid: A specialized interaction pattern is not supported by the internal component architecture

## Risks

- Breaking changes to the internal input group module interface can cascade across multiple dashboard forms
  Mitigation: Enforce strict semantic versioning and repository type verification across workspace boundaries before module releases
  Owner: Frontend Core Team
- Developers may bypass the shared module for edge-case styling needs
  Mitigation: Provide configurable slot and variant props within the shared component and enforce module usage through automated code review checks
  Owner: Frontend Core Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Discover the internal design system component package exports to inspect the composable subcomponents and properties provided by the input group module.
- Verify that client components consuming the input group module declare the appropriate client-side rendering boundary directive.

## Continuation Context


Verify commands:
- Discover and run the project repository type verification suite to validate module import paths and interface contracts.
- Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules.

Accept when:
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

## Enforcement

- Verified by: Automated static analysis and module import linting in the continuous integration pipeline
- Verified by: Peer code review verification of form components and input container structures
- Violation handling: Pull requests introducing ad-hoc input grouping containers or duplicate wrapper styling are blocked pending refactoring to the shared module.
- Exception process: Exceptions require review and approval from the design system maintainers, documented with a design rationalization issue.