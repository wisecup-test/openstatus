---
version: 2
name: 54c37d76-bd2a-46c4-a9b5-37da88f0e678-adoption-of-radix-ui-react-slot-for-component-composition-components-supporting-polymorphic-rendering-declare-aschild
description: "Adoption of @radix-ui/react-slot for Component Composition: Components Supporting Polymorphic Rendering Declare Aschild"
metadata:
  actual:
    adr_id: 54c37d76-bd2a-46c4-a9b5-37da88f0e678
    status: accepted
    candidate_group_id: ade8b01e-79c5-4003-b747-cd49e26b6a01
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
---

# Adoption of @radix-ui/react-slot for Component Composition: Components Supporting Polymorphic Rendering Declare Aschild

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/ui`

Applies to:
- `src/components/ui/**`

## Context

- Shared user interface components require polymorphism to render as alternate elements, such as anchors or custom buttons, without modifying markup semantics
- Standardizing element composition across the design system avoids fragile custom property forwarding and avoids injecting extraneous DOM container nodes
- Multiple foundational user interface components across the shared UI package consistently import the @radix-ui/react-slot library to satisfy composability needs

## Problem Statement

User interface components frequently need to alter their underlying rendered DOM element while preserving their styled appearance, accessibility behaviors, and event handlers. Traditional approaches relying on custom tag switching or container wrapping introduce type complexity, bloated DOM trees, and layout bugs.

## Decision

1. MUST: Components supporting polymorphic rendering MUST declare an asChild boolean property on their interface and conditionally render the Slot primitive when the property is true.

## Policy Block

- MUST Components supporting polymorphic rendering MUST declare an asChild boolean property on their interface and conditionally render the Slot primitive when the property is true.

In scope:
- Reusable user interface primitive components within the design system package requiring polymorphic element rendering
- Components that support rendering as alternate tags or navigation elements

Out of scope:
- Single-purpose views or domain-specific layouts that do not expose polymorphic rendering properties
- Non-visual utilities and server-only modules

## Rationale

- Evidence demonstrates consistent reliance on @radix-ui/react-slot across foundational UI primitives in the workspace
- Slot delegation provides reliable merging of styles, event handlers, and DOM attributes onto child components
- Standardizing on a single battle-tested primitive prevents fragmented approaches to polymorphism across the component library

## Consequences

Positive:
- Enables clean element polymorphism without adding unnecessary wrapper DOM nodes
- Guarantees consistent property and event handler merging when swapping underlying elements
- Standardizes composition patterns across all shared user interface components

Negative:
- Requires slotted children to be valid elements capable of receiving and merging forwarded properties and references
- Creates an external library dependency across the user interface component module surface
- Increases cognitive load on engineers unfamiliar with slot composition semantics and event merging behavior

## Alternatives

- Dynamic tag prop switching using an as polymorphic property (rejected)
  Rejected because: Creates brittle TypeScript type definitions for HTML elements and complicates event handler and attribute forwarding without standard prop merging semantics
  When valid: Simple micro-components with minimal props where slot delegation overhead is unnecessary
- Wrapping nested elements in container tags (rejected)
  Rejected because: Injects superfluous DOM elements that disrupt CSS grid, flexbox layout structures, and accessibility tree hierarchies
  When valid: Presentational contexts where DOM node depth and layout hierarchy are completely unconstrained

## Risks

- Consumers passing invalid children or multiple children to a slotted component will trigger runtime composition failures
  Mitigation: Enforce runtime type checks or invariant warnings during development when multiple children or non-element children are provided to slotted components
  Owner: Engineering Team
- Upstream API breaking changes across major library version increments could disrupt component interfaces
  Mitigation: Verify lock resolution files during dependency updates and run regression test suites prior to version upgrades
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
- Expose the composition capability via a standardized boolean property on the component interface
- Ensure that child components consuming slot properties use reference forwarding to allow access to underlying DOM nodes

## Continuation Context


Verify commands:
- Discover the project package runner and execute the automated test suite across the user interface package to verify component rendering and slot delegation.
- Discover and run the project type checking and linting scripts to validate slot property typing and component interface contracts.

Accept when:
- All user interface components delegating element rendering via slot primitives pass static type checking and property validation without compilation errors
- Automated component test suites confirm that slotted child elements correctly receive merged event handlers, attributes, and class names without extraneous DOM wrapper elements

## Enforcement

- Verified by: Static analysis and type checking within the automated continuous integration pipeline
- Verified by: Peer code reviews verifying compliance with design system component composition guidelines
- Violation handling: Pull request checks fail when non-standard element wrapper or dynamic tag patterns are introduced in shared components
- Violation handling: Maintainers request refactoring to slot delegation before code changes can be merged
- Exception process: Submit an architectural review request outlining technical blockers that prevent using slot-based composition
- Exception process: Obtain explicit approval from the design system maintainers before introducing alternative polymorphic element patterns