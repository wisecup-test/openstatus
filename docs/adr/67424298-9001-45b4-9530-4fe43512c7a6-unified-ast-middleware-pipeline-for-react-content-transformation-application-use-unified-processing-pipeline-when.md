---
version: 2
name: 67424298-9001-45b4-9530-4fe43512c7a6-unified-ast-middleware-pipeline-for-react-content-transformation-application-use-unified-processing-pipeline-when
description: "unified AST Middleware Pipeline for React Content Transformation: Application Use Unified Processing Pipeline When"
metadata:
  actual:
    adr_id: 67424298-9001-45b4-9530-4fe43512c7a6
    status: accepted
    candidate_group_id: 5551e761-053e-437f-abcc-5fc0d0854907
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
---

# unified AST Middleware Pipeline for React Content Transformation: Application Use Unified Processing Pipeline When

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/components/content/**`

## Context

- Content rendering requires parsing markdown text and transforming syntax trees into React component hierarchies.
- The codebase contains an implementation of unified chained with remarkParse, remarkRehype, and rehypeReact to handle markdown-to-React processing.
- Custom component overrides are provided directly in the rehypeReact plugin options to attach styling classes to semantic elements.
- The pattern is currently isolated to a single message processing component rather than abstracted as a shared service across the application.

## Problem Statement

Rendering markdown status messages requires a secure, structured mechanism to convert raw text into styled React components without resorting to unsafe raw HTML injection.

## Decision

1. MUST: The application MUST use the unified processing pipeline when converting markdown content into React component structures.

## Policy Block

- MUST The application MUST use the unified processing pipeline when converting markdown content into React component structures.

In scope:
- Components rendering markdown content into React element trees

Out of scope:
- Static text elements not requiring markdown parsing or AST manipulation

## Rationale

- Chaining unified with remarkParse, remarkRehype, and rehypeReact provides a modular AST transformation boundary that avoids unsafe raw HTML injection.
- Configuring component mappings inside rehypeReact binds design system utility classes directly to semantic tags at AST compile time.
- Because this implementation is currently observed in only a single file, it represents a candidate pattern that requires wider evaluation before standardized cross-application enforcement.

## Consequences

Positive:
- Prevents direct HTML string injection vulnerabilities by converting markdown into React virtual DOM elements.
- Provides granular control over the styling and behavior of individual semantic elements via component mapping.
- Establishes a pluggable middleware boundary where additional AST transformation plugins can be introduced.

Negative:
- Increases client-side bundle size due to parsing and transformation libraries.
- Adds runtime CPU overhead during component rendering for markdown parsing and AST traversal.
- Couples content rendering to unified plugin APIs and AST node schemas.

## Alternatives

- Direct raw HTML injection via dangerouslySetInnerHTML (rejected)
  Rejected because: Introduces significant cross-site scripting security vulnerabilities and bypasses design system component encapsulation.
  When valid: Static pre-sanitized content rendered in an isolated sandbox environment.
- Server-side HTML pre-rendering of markdown content (deferred)
  Rejected because: Requires backend infrastructure changes and additional API serialization contracts not present in the current architecture.
  When valid: High-volume static documentation sites with heavy SEO and initial page-load performance requirements.

## Risks

- Performance degradation when parsing large markdown messages repeatedly on the client thread.
  Mitigation: Memoize unified pipeline execution or offload transformation outside the primary render cycle.
  Owner: Frontend engineering team
- Version mismatch between unified, remark, and rehype plugins causing pipeline execution failures.
  Mitigation: Verify compatible peer dependency ranges and resolved lockfile versions prior to upgrades.
  Owner: Frontend engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Configure rehypeReact with project-standard React createElement, Fragment, and jsx runtime bindings according to the resolved compiler environment.
- Define component mapping objects with appropriate accessibility attributes, focus states, and styling markers for all rendered markdown elements.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting verification script from the repository dependency manifest.
- Discover and run the automated test suite script defined in the project configuration.

Accept when:
- Static analysis checks pass with zero syntax or typing errors across content processing components.
- Automated tests confirm markdown messages parse and render expected component structures.

## Enforcement

- Verified by: Automated continuous integration pipeline checks
- Verified by: Peer code review for pull requests modifying content transformation components
- Violation handling: Block pull requests that introduce raw HTML injection or unapproved markdown parsers.
- Violation handling: Require remediation to adopt the unified pipeline with proper component mappings.
- Exception process: Submit an architectural review request detailing why the unified pipeline cannot satisfy performance or functional requirements.
- Exception process: Obtain explicit approval from the frontend technical lead.