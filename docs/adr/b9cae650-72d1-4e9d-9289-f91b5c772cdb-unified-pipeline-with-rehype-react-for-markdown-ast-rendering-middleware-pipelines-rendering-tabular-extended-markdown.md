---
version: 2
name: b9cae650-72d1-4e9d-9289-f91b5c772cdb-unified-pipeline-with-rehype-react-for-markdown-ast-rendering-middleware-pipelines-rendering-tabular-extended-markdown
description: "Unified Pipeline with rehype-react for Markdown AST Rendering: Middleware Pipelines Rendering Tabular Extended Markdown"
metadata:
  actual:
    adr_id: b9cae650-72d1-4e9d-9289-f91b5c772cdb
    status: accepted
    candidate_group_id: 0564f026-4f47-45da-a4b3-64cc804e3bc3
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Unified Pipeline with rehype-react for Markdown AST Rendering: Middleware Pipelines Rendering Tabular Extended Markdown

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/components/**`

## Context

- User interface components receive dynamic markdown strings from conversational agents and user-generated content that require structured visual rendering.
- Direct injection of parsed HTML strings presents cross-site scripting security risks and circumvents client-side application link routing.
- The codebase establishes a unified processor chain that transforms markdown abstract syntax trees directly into React virtual DOM elements with custom tag overrides.

## Problem Statement

Rendering markdown content directly as HTML strings creates security vulnerabilities and breaks client-side navigation conventions. The application requires a structured, extensible pipeline that safely transforms markdown text into styled React components while enabling fine-grained element replacement.

## Decision

1. MAY: Middleware pipelines rendering tabular or extended markdown content MAY include remark-gfm in the processing chain prior to the HTML AST bridge.

## Policy Block

- MAY Middleware pipelines rendering tabular or extended markdown content MAY include remark-gfm in the processing chain prior to the HTML AST bridge.

In scope:
- Client-side user interface components rendering markdown strings from chat or message streams
- Dashboard content displays requiring custom component mapping for links and formatted code

Out of scope:
- Server-rendered static content generated at build time outside interactive UI components
- Plain text processing pipelines that do not output virtual DOM elements

## Rationale

- Chaining unified with remark-parse, remark-rehype, and rehype-react guarantees that parsed content remains within an abstract syntax tree representation until compiled into React elements, preventing raw HTML injection.
- Providing element overrides through rehype-react allows internal links to use client-side routing components and applies design system styling to code blocks and anchors.
- The plugin architecture of unified permits modular extensions such as GitHub Flavored Markdown where needed without modifying base transformation pipelines.

## Consequences

Positive:
- Eliminates cross-site scripting attack vectors by avoiding direct HTML string insertion.
- Enables standard design system styling and client-side link routing to replace standard HTML tags seamlessly.
- Maintains a composable middleware chain where features like GitHub Flavored Markdown can be selectively enabled.

Negative:
- Increases client bundle weight due to unified parser, compiler, and syntax tree transformation dependencies.
- Introduces performance overhead during repeated pipeline construction if processor chains are not memoized.

## Alternatives

- Direct HTML string injection with client-side sanitization libraries (rejected)
  Rejected because: Bypasses virtual DOM reconciliation and prevents seamless integration with client-side routing components for internal links.
  When valid: When rendering static documentation that does not require interactive components or custom link handling.
- Monolithic all-in-one markdown-to-react wrapper packages (rejected)
  Rejected because: Provides less composability and prevents modular inclusion of syntax plugins across differing message formats.
  When valid: When minimal bundle size is required and customized component replacement is unnecessary.

## Risks

- Unmemoized unified pipeline invocations causing UI lag during rapid message streaming.
  Mitigation: Enforce processor memoization or shared pipeline instances within rendering hooks.
  Owner: Frontend engineering team
- Incompatible plugin versions between remark and rehype AST specifications.
  Mitigation: Strict lock-file version grounding before updating AST middleware plugins.
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
- Memoize unified pipeline instances or their compiled processor output within rendering hooks to prevent re-instantiating the parser and compiler chain across component re-renders.
- Provide explicit type annotations for component replacement dictionaries passed to rehype-react to ensure safe property forwarding to replacement elements.

## Continuation Context


Verify commands:
- Discover and run the static analysis and linter scripts defined in the project dependency manifest to enforce forbidden raw HTML properties.
- Discover and execute the component test suite defined in the project manifest to validate markdown rendering and link handling.
- Discover and run the project type checker to verify unified processor and component mapping type signatures.

Accept when:
- All message rendering components parse markdown through the unified pipeline without invoking raw HTML injection properties.
- Static type analysis and component tests pass without errors across all markdown display modules.

## Enforcement

- Verified by: Automated code review and lint checks preventing direct raw HTML injection in UI components.
- Verified by: Unit and integration tests validating that markdown output maps to expected React component hierarchies.
- Violation handling: Pull requests attempting direct HTML injection or bypass of the unified pipeline are blocked during code review.
- Exception process: Exceptions require documented architecture review and explicit security sign-off from the technical lead.