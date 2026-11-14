---
version: 2
name: 54777c1a-78ac-4f4c-a164-a85c201a3921-adoption-of-openstatus-icons-and-brand-subpath-for-centralized-iconography-general-user-interface-symbols-action-controls
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: General User Interface Symbols Action Controls"
metadata:
  actual:
    adr_id: 54777c1a-78ac-4f4c-a164-a85c201a3921
    status: accepted
    candidate_group_id: 6657c8b1-ac0f-40f5-afa9-ba814c964fbd
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/login/**
      - src/components/**
      - src/config/**
      - src/data/**
---

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: General User Interface Symbols Action Controls

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/login/**`
- `src/components/**`
- `src/config/**`
- `src/data/**`

## Context

- User interface surfaces across dashboards, onboarding workflows, data tables, and integration settings require consistent graphical indicators and external provider brand logos.
- Direct embedding of vector paths in presentation components causes asset duplication, inconsistent stroke and scaling semantics, and maintenance friction across dashboard modules.
- Multiple independent components require brand emblems for notification channels and cloud importers alongside standard system action icons.
- Centralizing iconography into a shared workspace module with dedicated subpath exports establishes a single source of truth for visual assets.

## Problem Statement

User interface components across dashboard domains require both standard utility iconography and specialized external integration brand logos. Without a centralized library and defined import boundaries, views risk duplicating inline vector assets, consuming fragmented external icon sets, and blurring the boundary between generic functional symbols and licensed third-party brand marks.

## Decision

1. MUST: General user interface symbols, action controls, navigation markers, and status indicators MUST be imported from @openstatus/icons.

## Policy Block

- MUST General user interface symbols, action controls, navigation markers, and status indicators MUST be imported from @openstatus/icons.

In scope:
- User interface views and presentation components requiring graphical icons, action symbols, or visual indicators.
- Configurations, notification settings, and data table columns presenting external service logos and brand marks.

Out of scope:
- Non-visual backend services, API routes, and database access layers that do not render graphical interface elements.
- Raw dynamic image assets, user-uploaded media, and external content delivered via remote media endpoints.

## Rationale

- Partitioning brand assets into @openstatus/icons/brand separates domain-specific vendor marks from generic system actions, optimizing tree-shaking and preventing naming collisions.
- Centralizing icon definitions into @openstatus/icons ensures consistent sizing, styling, and visual language across all client components.
- Evidence across multiple dashboard modules demonstrates consistent adoption of both root and brand iconography entrypoints, indicating an established architectural convention.

## Consequences

Positive:
- Guarantees visual consistency across dashboard, table, form, and onboarding interfaces.
- Eliminates component boilerplate and bundle bloat by preventing duplicated inline vector code.
- Isolates external vendor brand assets into a dedicated subpath to avoid collision with general UI symbols.

Negative:
- Requires publishing or updating the shared iconography package whenever new system icons or brand logos are introduced.
- Introduces an internal workspace dependency that client-side UI components must coordinate with during feature development.

## Alternatives

- Embedding raw inline vector paths directly within individual UI components (rejected)
  Rejected because: Leads to asset duplication, inconsistent visual attributes, and maintenance overhead across application views
  When valid: One-off dynamic vector transformations that cannot be represented as static component icons
- Consuming uncurated third-party icon libraries directly inside feature modules (rejected)
  Rejected because: Encourages fragmented visual styling, varied stroke weights, and bloated client bundles
  When valid: Rapid isolated exploratory prototypes prior to iconography standardization
- Combining all system and brand iconography into a single flat namespace without subpath separation (rejected)
  Rejected because: Increases namespace pollution, risks naming collisions between commercial brands and generic actions, and impairs modular tree-shaking
  When valid: Small applications with fewer than a dozen total visual assets

## Risks

- Breaking changes in exported icon identifiers or subpath manifests during iconography package updates
  Mitigation: Strict semantic versioning and automated workspace type checking across dependent application modules
  Owner: engineering team
- Misplacement of brand assets in the root namespace rather than the dedicated brand subpath
  Mitigation: Automated lint rules and code reviews verifying that provider and vendor emblems use the brand export
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
- Ensure icon component imports explicitly distinguish between general UI symbols from @openstatus/icons and third-party provider marks from @openstatus/icons/brand.
- When introducing new visual marks, validate that stroke weights, viewports, and color inheritance attributes conform to existing workspace icon definitions.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and type checking suite to ensure all symbol imports from @openstatus/icons and @openstatus/icons/brand resolve cleanly.
- Discover and execute repository linting rules to validate that no ad-hoc third-party icon libraries or duplicate vector paths are introduced.
- Discover and run the project build scripts to confirm clean module bundling and tree-shaking of icon assets without circular dependencies.

Accept when:
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

## Enforcement

- Verified by: Automated static analysis and module import boundaries checked during continuous integration.
- Verified by: Peer code reviews for all pull requests modifying user interface presentation layers.
- Violation handling: Continuous integration blocks pull requests containing unapproved direct icon dependencies or inline vector duplicates.
- Violation handling: Authors must replace ad-hoc iconography with exports from @openstatus/icons or contribute missing assets to the iconography package.
- Exception process: Exceptions require documented approval from UI architectural leads with an issue tracking asset inclusion into the shared package.