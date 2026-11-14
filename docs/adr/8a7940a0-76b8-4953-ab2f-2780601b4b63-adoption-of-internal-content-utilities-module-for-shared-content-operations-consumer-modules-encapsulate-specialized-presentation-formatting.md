---
version: 2
name: 8a7940a0-76b8-4953-ab2f-2780601b4b63-adoption-of-internal-content-utilities-module-for-shared-content-operations-consumer-modules-encapsulate-specialized-presentation-formatting
description: "Adoption of Internal Content Utilities Module for Shared Content Operations: Consumer Modules Encapsulate Specialized Presentation Formatting"
metadata:
  actual:
    adr_id: 8a7940a0-76b8-4953-ab2f-2780601b4b63
    status: accepted
    candidate_group_id: 93b03f54-7c9b-4fbb-b40e-541b4d07014e
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/data/**
---

# Adoption of Internal Content Utilities Module for Shared Content Operations: Consumer Modules Encapsulate Specialized Presentation Formatting

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/app/**`
- `src/data/**`

## Context

- Multiple functional areas of the web application, including sitemap generation, content data access, and error presentation views, require common content operations and metadata handling.
- Duplicating content parsing, slug derivation, and metadata extraction across individual presentation and routing modules creates implementation drift and maintenance overhead.
- Establishing a shared internal content utilities module consolidates content-related helper logic, ensuring consistent behavior across runtime and static compilation targets.

## Problem Statement

Without a centralized module for shared content operations, disparate parts of the application independently implement overlapping routines for parsing content, formatting metadata, and extracting structural attributes. This decentralization leads to divergent content transformations between route generation pipelines, data layers, and fallback display views, increasing the risk of inconsistent application state and duplicated maintenance effort.

## Decision

1. MAY: Consumer modules MAY encapsulate specialized presentation formatting locally if the logic is unique to a single presentation component and does not operate on shared content schemas.

## Policy Block

- MAY Consumer modules MAY encapsulate specialized presentation formatting locally if the logic is unique to a single presentation component and does not operate on shared content schemas.

In scope:
- Content processing, normalization, and transformation routines across web application modules.
- Route generation handlers, content data access layers, and fallback rendering views consuming structured content.

Out of scope:
- Standalone presentation components that do not parse, transform, or extract structured content metadata.
- External network boundary handlers that do not interact with the project content domain.

Exceptions:
- EXC-20-001: A consumer component requires an isolated, one-off content transformation that is strictly coupled to a transient view and cannot be generalized.

## Rationale

- Static analysis of the application codebase reveals that the internal content utilities module is systematically imported across routing, data access, and view layers to perform content-specific transformations.
- Centralizing shared content operations in a dedicated internal module enforces uniform content handling semantics across static site generation and dynamic rendering paths.
- Consolidating utility logic reduces code duplication and provides a single location for maintaining content schema compatibility across the web application.

## Consequences

Positive:
- Eliminates redundant content extraction and normalization logic across disparate routing and view modules.
- Ensures uniform content parsing and metadata generation between route sitemap generators and rendering views.
- Simplifies maintenance of content schema compatibility by centralizing transformation logic.

Negative:
- Introduces an internal dependency coupling between consumer modules and the content utilities module.
- Requires deliberate interface curation to prevent the shared module from accumulating unrelated utility routines.

## Alternatives

- Inlining content processing routines directly within consumer modules (rejected)
  Rejected because: Inlining routines causes duplicate logic, inconsistent content normalization across routes, and increased maintenance overhead when content schemas change.
  When valid: An isolated operation is strictly unique to a single presentation component and shares no common structures with other content consumers.
- Adopting an external third-party content management SDK for utility transformations (rejected)
  Rejected because: An external SDK introduces unnecessary external dependency footprint and complexity for operations that are cleanly handled by internal utility functions.
  When valid: Content processing requirements exceed internal capabilities and demand external headless platform runtime integration.

## Risks

- The shared content utilities module could accumulate unrelated helper routines and become an unconstrained catch-all module.
  Mitigation: Restrict exports to domain-specific content processing functions and enforce strict interface boundaries via code review.
  Owner: Frontend engineering team
- Introducing breaking signature changes in content utilities could disrupt multiple dependent modules simultaneously.
  Mitigation: Maintain automated unit test coverage over public utility functions and execute project verification test suites prior to integrating updates.
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
- Structure utility functions as modular, single-responsibility units that operate on standard data structures to facilitate tree shaking and dead-code elimination.
- Ensure all exported functions within the content utilities module provide explicit type definitions for input parameters and return types.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script from the root configuration to verify module import resolution and interface contracts.
- Discover and execute the automated test suite targeting content utility functions to validate operational correctness.
- Discover and execute the static analysis linter script to ensure compliance with import path conventions.

Accept when:
- All content utility functions pass automated unit tests without failures.
- Type checking completes with zero diagnostic errors across all consumer modules.
- No circular dependencies or duplicate content parsing implementations are detected by static analysis.

## Enforcement

- Verified by: Automated continuous integration checks executing type verification and unit tests.
- Verified by: Peer code review validating adherence to centralized utility imports.
- Violation handling: Pull requests containing duplicated content parsing routines or unauthorized bypasses of the content utilities module will be blocked until refactored.
- Exception process: Exceptions must be requested via architectural review, accompanied by technical justification demonstrating why centralization is not viable for the specific use case.