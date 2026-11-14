---
version: 2
name: 54c84448-38f4-4893-993f-7dccb681eca6-evaluation-of-after-callback-within-trpc-server-middleware-procedure-definitions-utilizing-trpc-server-middleware
description: "Evaluation of after Callback within @trpc/server Middleware: Procedure Definitions Utilizing Trpc Server Middleware"
metadata:
  actual:
    adr_id: 54c84448-38f4-4893-993f-7dccb681eca6
    status: accepted
    candidate_group_id: 488a7432-7d16-4be8-9ce8-5827c5960bf3
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Evaluation of after Callback within @trpc/server Middleware: Procedure Definitions Utilizing Trpc Server Middleware

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- A single server API module configures procedure middleware with timing logging and an asynchronous after callback executing background analytics.
- Static pattern detection flagged the after callback under unit testing strategy based on identifier heuristics.
- The codebase lacks multi-file evidence demonstrating an adopted unit testing framework or convention rooted in this pattern.

## Problem Statement

Static analysis detected a single-file post-response execution hook as an architectural unit testing convention, risking misclassification of server procedure middleware as a quality assurance strategy.

## Decision

1. MUST_NOT: Procedure definitions utilizing @trpc/server middleware MUST NOT treat asynchronous post-execution after callbacks as unit test lifecycle constructs.

## Policy Block

- MUST_NOT Procedure definitions utilizing @trpc/server middleware MUST NOT treat asynchronous post-execution after callbacks as unit test lifecycle constructs.

In scope:
- Server-side procedure middleware pipelines defined with @trpc/server.

Out of scope:
- Dedicated automated testing suites operating outside procedure execution middleware.

## Rationale

- Static analysis identified an after callback within procedure middleware, misinterpreting post-response execution as a unit testing lifecycle hook.
- The pattern is isolated to a single file implementation and does not establish a project-wide unit testing convention or framework adoption.
- Treating middleware telemetry and post-processing callbacks as testing strategy constructs creates architectural ambiguity between request lifecycle management and automated test assertions.

## Consequences

Positive:
- Prevents misattribution of server request lifecycle execution hooks to unit testing frameworks.
- Clarifies the architectural boundary between @trpc/server middleware execution and quality assurance strategies.

Negative:
- Requires developers to establish separate, explicit test harnesses rather than relying on inline execution hooks.
- Maintains an unverified status for unit testing strategy until cross-cutting testing conventions are observed.

## Alternatives

- Adopt after callback hooks within procedure definitions as unit test verification points (rejected)
  Rejected because: Request-handling middleware callbacks execute in server runtime paths and cannot substitute for independent, isolated unit test assertions.
  When valid: Never valid for automated unit testing suites.
- Establish dedicated unit testing framework harnesses across all package modules (deferred)
  Rejected because: The available evidence does not demonstrate an adopted unit testing framework across multiple files.
  When valid: When a multi-file automated testing pattern is introduced and evidenced across the codebase.

## Risks

- Telemetry and analytics tracked within after callbacks may fail silently if background tasks are terminated prematurely.
  Mitigation: Ensure post-response lifecycle execution handlers are monitored via server runtime error tracking.
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
- Locate @trpc/server procedure definitions within the API package boundary and verify that post-response work is decoupled from test assertions.

## Continuation Context


Verify commands:
- Discover and run the project test execution script from the repository manifest to verify procedure logic independently.

Accept when:
- The project test verification scripts complete without relying on procedure middleware lifecycle hooks for test assertions.

## Enforcement

- Verified by: Code review during pull requests affecting server procedure definitions.
- Verified by: Automated continuous integration pipeline checks enforcing clean separation of runtime middleware and test suites.
- Violation handling: Pull requests attempting to embed test lifecycle hooks into production procedure middleware are rejected.
- Exception process: Exceptions require approval from the technical architecture lead with documented justification for custom lifecycle callbacks.