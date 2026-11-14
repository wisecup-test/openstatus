---
version: 2
name: a35a7585-4146-440b-9938-dec5cafd5d33-adoption-of-effect-library-for-workflow-services-workflow-handlers-not-bypass-structured-effect
description: "Adoption of Effect Library for Workflow Services: Workflow Handlers Not Bypass Structured Effect"
metadata:
  actual:
    adr_id: a35a7585-4146-440b-9938-dec5cafd5d33
    status: accepted
    candidate_group_id: 8f23a55a-0b96-4d0e-88b7-ec6fbdab8372
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
---

# Adoption of Effect Library for Workflow Services: Workflow Handlers Not Bypass Structured Effect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/checker/**`
- `src/cron/**`

## Context

- Asynchronous background services execute complex multi-step workflows including alerting dispatch and cron maintenance.
- Uncaught asynchronous exceptions in scheduled jobs lead to silent failures or uncoordinated retry states across workflow boundaries.
- Multiple workflow modules require a unified approach to managing side effects, external database transactions, and error propagation.

## Problem Statement

Workflow and scheduled cron tasks require resilient, composable execution pipelines that cleanly distinguish recoverable domain errors from fatal defects, which unmanaged native promises and ad hoc try-catch blocks fail to guarantee across asynchronous execution boundaries.

## Decision

1. MUST_NOT: Workflow handlers MUST NOT bypass structured effect error-handling channels in favor of untyped unhandled exceptions or unmanaged promise rejections.

## Policy Block

- MUST_NOT Workflow handlers MUST NOT bypass structured effect error-handling channels in favor of untyped unhandled exceptions or unmanaged promise rejections.

In scope:
- Asynchronous workflow task handlers
- Scheduled cron job processors
- Background incident and alerting dispatchers

Out of scope:
- Stateless utility functions lacking asynchronous side effects
- Declarative database schema definitions

Exceptions:
- EXP-20-001: Legacy handlers undergo incremental migration before full conversion to effect pipelines

## Rationale

- Static analysis identifies consistent importation and usage of effect across workflow checker and cron task implementations.
- Adopting a unified functional library provides explicit error tracking, composability, and resource management across distributed background tasks.
- Centralizing workflow side effects into functional pipelines mitigates untyped promise rejections across scheduled execution boundaries.

## Consequences

Positive:
- Consistent composable asynchronous pipeline orchestration across scheduled workflows.
- Stronger static type safety for recoverable failures and dedicated error channels.
- Standardized patterns for resource management and cancellation across background workers.

Negative:
- Steeper learning curve for contributors unfamiliar with functional programming primitives.
- Potential overhead in wrapping imperative third-party library calls into effect pipelines.

## Alternatives

- Native asynchronous promises with standard try-catch blocks (rejected)
  Rejected because: Native promise chains lack static error-channel tracking and yield unpredictable error propagation in complex background workflows.
  When valid: Simple single-operation scripts with minimal branching and no dependency on composable failure channels.
- Custom domain error classes and procedural orchestration wrappers (rejected)
  Rejected because: Ad hoc procedural wrappers duplicate error-handling infrastructure across services without standard runtime guarantees.
  When valid: Minimal isolated utility packages that cannot introduce external runtime dependencies.

## Risks

- Incomplete migration leading to mixed paradigms of native promise throwing alongside functional effect pipelines.
  Mitigation: Establish linting and code review checks to require effect error handling in all workflow service entry points.
  Owner: Engineering Team
- Breaking API changes across library releases impacting workflow execution.
  Mitigation: Strict lock-version grounding and automated regression test suites executed prior to dependency upgrades.
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
- Wrap external database queries and network notification calls into managed effect operations to ensure errors are captured in the type system.
- Organize scheduled job controllers so that routing or trigger adapters delegate directly to functional effect pipelines.

## Continuation Context


Verify commands:
- echo "Discover and execute the project verification script defined in the dependency manifest"
- echo "Discover and run static type checking to verify compliance of effect pipeline signatures"
- echo "Inspect lock resolution artifacts to verify the resolved version of effect matches documented APIs"

Accept when:
- The project type verification and static analysis checks pass with zero diagnostic errors across workflow modules.
- All asynchronous workflow handlers return managed effect computations without unhandled promise rejections.
- The resolved version of effect in the repository lock artifact matches the API surface used in workflow code.

## Enforcement

- Verified by: Static analysis and automated type check suites executed during continuous integration.
- Verified by: Mandatory peer code review enforcing architectural guidelines for background workflow tasks.
- Violation handling: Pull requests introducing unmanaged asynchronous exceptions or bypassing effect in workflow modules are blocked until remediated.
- Exception process: Submit an architectural review request documenting technical constraints and a scoped timeline for full migration to effect pipelines.