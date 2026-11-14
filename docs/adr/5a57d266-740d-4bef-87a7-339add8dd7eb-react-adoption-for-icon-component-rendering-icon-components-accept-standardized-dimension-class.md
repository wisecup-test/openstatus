---
version: 2
name: 5a57d266-740d-4bef-87a7-339add8dd7eb-react-adoption-for-icon-component-rendering-icon-components-accept-standardized-dimension-class
description: "React Adoption for Icon Component Rendering: Icon Components Accept Standardized Dimension Class"
metadata:
  actual:
    adr_id: 5a57d266-740d-4bef-87a7-339add8dd7eb
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Icon Components Accept Standardized Dimension Class

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/icons`

Applies to:
- `src/nucleo/**`

## Context

- Application interfaces require a cohesive and scalable vector iconography system integrated seamlessly into the view layer.
- Centralizing iconography into modular functional components enables consistent property contracts, accessibility markup, and theme styling.
- Analysis of icon package modules reveals uniform adoption of React and centralized type definitions across all individual icon assets.

## Problem Statement

Distributing raw vector assets or maintaining heterogeneous rendering approaches creates friction in component styling, accessibility, and type safety across application interfaces. The iconography suite requires a unified rendering technology and contract standard.

## Decision

1. SHOULD: Icon components SHOULD accept standardized dimension, class name, and color attributes delegated directly to root vector elements.

## Policy Block

- SHOULD Icon components SHOULD accept standardized dimension, class name, and color attributes delegated directly to root vector elements.

In scope:
- All icon component modules and visual symbol exports within the iconography package
- Shared type definitions governing vector icon component interfaces

Out of scope:
- Non-visual utility functions and headless helper libraries
- Raw vector asset conversion build scripts and export pipelines

## Rationale

- Standardizing icon assets as React components ensures direct compatibility with the existing application component tree and state management.
- A shared contract imported across all icon files guarantees uniform prop signatures for dimensions, accessibility attributes, and theming.
- Evidence across 116 icon modules demonstrates established architectural consensus for React component encapsulation.

## Consequences

Positive:
- Predictable and typed developer experience when consuming icon components across application features
- Centralized maintenance of vector styling, accessibility attributes, and SVG DOM structure
- Granular module bundler optimization and tree-shaking support for individual icon imports

Negative:
- Tight coupling of icon assets to the React view rendering runtime
- Maintenance overhead required to update component signatures when upstream framework conventions evolve

## Alternatives

- Raw SVG asset distribution without component wrappers (rejected)
  Rejected because: Raw vector files do not provide typed component interfaces, dynamic styling props, or consistent accessibility attribute injection.
  When valid: When delivering assets to non-framework static websites or multi-platform environments lacking a common runtime.
- Icon font glyph generation (rejected)
  Rejected because: Icon fonts suffer from accessibility degradation, layout rendering artifacts, and lack multi-color support.
  When valid: When supporting legacy user agents with strict bandwidth constraints or lacking SVG rendering capabilities.

## Risks

- Large bundle footprint if consuming applications import icon modules without tree-shaking
  Mitigation: Structure package exports as discrete modular units and document granular import conventions
  Owner: engineering team
- Framework version divergence between the icon package and consuming applications
  Mitigation: Declare framework peer-dependency boundaries resolved against repository lock artifacts
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
- Ensure all new icon components follow functional component patterns returning semantic vector markup conforming to shared types.
- Verify that export configurations preserve modularity to facilitate tree-shaking during downstream application bundling.

## Continuation Context


Verify commands:
- # Discover and execute repository typecheck script across workspace packages
- # Discover and execute component test and verification scripts from repository manifest

Accept when:
- All icon component modules compile cleanly with zero type errors against the resolved framework types
- Each icon component exports a functional interface conforming to the shared icon type definition
- Verification scripts for workspace package compilation pass without warnings

## Enforcement

- Verified by: Automated workspace typecheck and test verification in continuous integration pipelines
- Verified by: Code review enforcing standardized component signatures and shared type usage
- Violation handling: Continuous integration check failure on unresolved types or non-standard component contracts
- Violation handling: Pull request rejection for icon components introducing divergent rendering libraries or ad-hoc types
- Exception process: Submit an architecture exception request detailing why the icon cannot be represented as a standard component
- Exception process: Obtain formal approval from the UI architecture team prior to merging alternative asset representations