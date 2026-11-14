---
version: 2
name: a079a249-6cdb-406b-8086-0b53b970e50d-adoption-of-openstatus-icons-for-application-iconography-before-integrating-modifying-references-openstatus-icons
description: "Adoption of @openstatus/icons for Application Iconography: Before Integrating Modifying References Openstatus Icons"
metadata:
  actual:
    adr_id: a079a249-6cdb-406b-8086-0b53b970e50d
    status: accepted
    candidate_group_id: 189c3723-9793-4c50-9b60-a1dd4ce669b2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/icons for Application Iconography: Before Integrating Modifying References Openstatus Icons

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`
- `src/components/**`
- `src/data/**`

## Context

- Client user interface components across the status page application require standardized iconography for navigation, status reporting, and user interactions.
- Direct inclusion of diverse icon libraries or inline SVG assets across components leads to stylistic divergence and bundle bloat.
- The codebase establishes @openstatus/icons as a dedicated internal library to provide a single source of truth for visual iconography.

## Problem Statement

Inconsistent icon usage and fragmented asset sourcing across UI components lead to visual discrepancies, maintenance overhead, and redundant asset payloads. A unified iconography strategy is required to enforce visual consistency and simplify icon consumption across client modules.

## Decision

1. MUST: Before integrating or modifying references to @openstatus/icons, developers MUST discover the workspace dependency manifest and resolution artifact to resolve the exact locked version.

## Policy Block

- MUST Before integrating or modifying references to @openstatus/icons, developers MUST discover the workspace dependency manifest and resolution artifact to resolve the exact locked version.

In scope:
- All client-side user interface components rendering visual symbols, status badges, and action controls
- Client-side data definitions and descriptors that assign icon symbols to status categories or domain entities

Out of scope:
- Server-only logic, background jobs, and data persistence layers with no user interface rendering responsibilities
- Non-iconographic image assets such as photographs, large product illustrations, and branding logos

## Rationale

- Evidence across twenty-three files demonstrates widespread adoption of @openstatus/icons for UI navigation, controls, and client-side data representations.
- Centralizing iconography into a dedicated package ensures consistent sizing, styling, and visual cohesion across all application views.
- Encapsulating icon definitions allows asset updates and optimizations to propagate across consuming components without localized refactoring.

## Consequences

Positive:
- Guarantees unified visual appearance and design system compliance across all client interfaces.
- Simplifies component development by providing a pre-configured, standardized set of icon components.
- Facilitates centralized tree-shaking and asset optimization at the package boundary.

Negative:
- Introducing new icons requires updating the shared icon package before component consumption.
- Creates a shared dependency on the internal icon library across all frontend presentation modules.

## Alternatives

- Inline SVG elements within individual client components (rejected)
  Rejected because: Duplicating SVG markup across components increases bundle size, creates visual inconsistencies, and complicates global icon asset maintenance.
  When valid: One-off decorative illustrations that are never reused across the application.
- Directly consuming external third-party icon packages in consuming applications (rejected)
  Rejected because: Coupling consumer applications directly to external icon libraries causes version drift and prevents centralizing iconography curation.
  When valid: Prototyping standalone applications prior to integrating with the organization design system.

## Risks

- Breaking interface changes in @openstatus/icons updates can impact all consuming client components.
  Mitigation: Strict adherence to version resolution workflows and automated type checking across workspace boundaries.
  Owner: engineering team
- Accumulation of unused icon exports within @openstatus/icons can increase bundle footprints if tree-shaking fails.
  Mitigation: Export icons as individual discrete symbols and enforce module-level tree-shaking verification.
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
- Import icon components directly as named exports from @openstatus/icons to enable efficient tree-shaking during client bundle assembly.
- Verify that accessibility attributes such as aria-hidden or accessible title tags are applied based on whether the icon is decorative or interactive.

## Continuation Context


Verify commands:
- Discover the workspace static analysis and linting command from the dependency manifest and execute it to verify all icon imports originate from @openstatus/icons.
- Discover and run the project type checking script to validate that all icon component properties and imported symbols conform to the library definitions.
- Execute the workspace dependency audit script discovered from the root configuration to confirm no unauthorized external icon packages are declared in dependency manifests.

Accept when:
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

## Enforcement

- Verified by: Continuous integration automated linting and dependency verification checks
- Verified by: Peer code review validating that new visual symbols are imported from @openstatus/icons
- Violation handling: Pull requests containing inline SVG declarations or unauthorized icon imports will be blocked from merging until corrected.
- Exception process: Exceptions for unique graphic assets must be reviewed and approved by the frontend architecture team with documented rationale.