---
version: 2
name: 9391790a-9a14-4f92-8a96-cce12001d4b8-adoption-of-status-vocab-for-status-domain-vocabulary-and-evaluation-component-impact-severity-calculations-across-aggregated
description: "Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Component Impact Severity Calculations Across Aggregated"
metadata:
  actual:
    adr_id: 9391790a-9a14-4f92-8a96-cce12001d4b8
    status: accepted
    candidate_group_id: 232b680f-7bef-42aa-9625-54d5ac910e0b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/content/markdown/**
---

# Adoption of status-vocab for Status Domain Vocabulary and Evaluation: Component Impact Severity Calculations Across Aggregated

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/content/markdown/**`

## Context

- Status page rendering requires consistent vocabulary and semantic definitions across content generators and presentation helpers.
- Dispersing status definitions and severity precedence algorithms across formatting utilities leads to divergent terminology and conflicting component status displays.
- An internal domain module, status-vocab, centralizes status taxonomy and impact evaluation logic across status generation workflows.

## Problem Statement

Generating status reports and markdown outputs from component status data requires uniform vocabulary and deterministic impact calculations. Without a centralized domain module, content generators and helper routines risk re-implementing disparate status string constants and conflicting severity comparison rules, leading to inconsistent user-facing status page reports.

## Decision

1. MUST: Component impact severity calculations across aggregated status items MUST utilize the worstImpact evaluation function provided by the status-vocab module rather than reimplementing precedence logic.

## Policy Block

- MUST Component impact severity calculations across aggregated status items MUST utilize the worstImpact evaluation function provided by the status-vocab module rather than reimplementing precedence logic.

In scope:
- Content generation and helper modules responsible for producing status page representations.
- Routines evaluating component impact severity and aggregating status vocabulary.

Out of scope:
- Third-party network transport layers decoupled from status page domain representation.
- Generic styling and layout components that do not interpret status domain semantics.

## Rationale

- Centralizing domain vocabulary in the status-vocab module guarantees semantic consistency across all rendered status outputs.
- Consolidating impact severity resolution into worstImpact prevents divergence in how incident severity is determined across different components.
- Reusing an internal vocabulary module eliminates redundant status definitions and eases maintenance when status taxonomy evolves.

## Consequences

Positive:
- Consistent status vocabulary and incident impact terminology across all generated markdown artifacts.
- Centralized maintenance of impact evaluation algorithms including worstImpact resolution.
- Reduced code duplication across status generators and helper utilities.

Negative:
- Direct dependency coupling between presentation helpers and the internal domain vocabulary module.
- Changes to status-vocab contracts require synchronized updates across all consuming content generation routines.

## Alternatives

- Decentralized status string constants and local severity comparison functions in each generator (rejected)
  Rejected because: Causes semantic drift and divergent severity calculations between different status page output generators.
  When valid: When content generators have entirely isolated, mutually incompatible domain lifecycles.
- Direct reliance on external service API schemas without an intermediate domain vocabulary module (rejected)
  Rejected because: Couples presentation formatting directly to external service schemas and lacks custom impact evaluation utilities.
  When valid: When the external API schema directly provides all necessary presentation vocabulary and helper functions.

## Risks

- Breaking changes in the status-vocab module signature could disrupt multiple content generation pathways simultaneously.
  Mitigation: Enforce regression test suites and automated type checking across all consuming generator modules.
  Owner: engineering team
- Consumers might bypass worstImpact and introduce ad-hoc severity comparisons.
  Mitigation: Establish static analysis lint rules and mandatory peer code review to reject localized impact logic.
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
- Import domain status definitions and the worstImpact function directly from the shared vocabulary module when aggregating component metrics.
- Maintain state tracking data structures during content generation passes to record cumulative worst-impact evaluations across related components.

## Continuation Context


Verify commands:
- Discover and execute the project verification script to validate module import resolution and type consistency across status content generators.
- Discover and run the automated test suite covering markdown generator outputs and status evaluation functions.

Accept when:
- All status content generators import domain vocabulary and severity functions from status-vocab with zero compilation errors.
- Impact calculations correctly resolve cumulative component status through worstImpact in all automated test scenarios.
- No redundant status string definitions exist within presentation helpers.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and type checks.
- Verified by: Peer code review verifying that status vocabulary and impact evaluation use the centralized module.
- Violation handling: Pull requests containing localized status definitions or duplicate severity logic must be blocked until refactored.
- Violation handling: Static analysis failures halt the build pipeline.
- Exception process: Exceptions require approval from the technical lead with documented justification demonstrating unique requirements outside the domain vocabulary.