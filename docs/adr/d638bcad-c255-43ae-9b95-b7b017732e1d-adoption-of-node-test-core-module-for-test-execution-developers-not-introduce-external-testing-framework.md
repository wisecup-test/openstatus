---
version: 2
name: d638bcad-c255-43ae-9b95-b7b017732e1d-adoption-of-node-test-core-module-for-test-execution-developers-not-introduce-external-testing-framework
description: "Adoption of node:test Core Module for Test Execution: Developers Not Introduce External Testing Framework"
metadata:
  actual:
    adr_id: d638bcad-c255-43ae-9b95-b7b017732e1d
    status: accepted
    candidate_group_id: 9ce22998-f17d-41f3-8299-a5e86dfaad71
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adoption of node:test Core Module for Test Execution: Developers Not Introduce External Testing Framework

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/test-utils`

Applies to:
- `src/**`

## Context

- The codebase requires test execution capabilities for shared test utilities.
- The native runtime module node:test is utilized within a shared test runner utility to provide test lifecycle execution.
- The pattern is currently isolated to a single test utility module rather than adopted uniformly across the entire project.

## Problem Statement

Standardizing test runner utilities requires deciding between external testing frameworks and native runtime testing modules. While node:test is present in utility code, lack of broader multi-file adoption leaves architectural test runner conventions fragmented.

## Decision

1. SHOULD_NOT: Developers SHOULD NOT introduce external testing framework runners into internal test execution utilities when native capabilities provided by node:test satisfy requirements.

## Policy Block

- SHOULD_NOT Developers SHOULD NOT introduce external testing framework runners into internal test execution utilities when native capabilities provided by node:test satisfy requirements.

In scope:
- Internal test runner utilities and shared test execution infrastructure.

Out of scope:
- End-to-end integration test suites governed by separate runner conventions.

## Rationale

- The node:test module provides built-in test assertion and lifecycle primitives without requiring external test framework runtime overhead.
- Evidence indicates adoption within test execution utility code, demonstrating feasibility for shared testing harnesses.
- Restricting third-party dependencies in test runner utilities reduces dependency tree complexity and version drift.

## Consequences

Positive:
- Eliminates third-party test framework dependencies for targeted test utilities.
- Reduces installation overhead and maintenance cost for internal testing tools.
- Aligns test execution directly with native runtime execution capabilities.

Negative:
- Limits test suites to features supported natively by the node:test API surface.
- Requires migration effort if existing suites rely on third-party test framework plugins or custom reporters.

## Alternatives

- Third-party external testing framework (rejected)
  Rejected because: Introduces external dependency bloat and ongoing maintenance overhead for utility execution.
  When valid: When complex browser-based rendering or extensive custom plugin ecosystems are strictly required.
- Custom bespoke test assertion and execution harness (rejected)
  Rejected because: Reinvents basic test lifecycle, reporting, and execution primitives already provided natively by node:test.
  When valid: When operating in restricted environments where runtime standard modules are unavailable.

## Risks

- Feature disparity between native node:test APIs and specialized external test framework capabilities.
  Mitigation: Assess test harness requirements prior to authoring suites and isolate runner utilities behind shared interfaces.
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
- Discover the test execution scripts defined in repository configuration files to execute test suites backed by node:test.
- Structure test runner harnesses to consume test functions from the native module while isolating runner invocations from consumer suites.

## Continuation Context


Verify commands:
- Find the repository test execution script in the project manifest and execute the test suite.
- Verify that test runner utilities successfully invoke test execution through the native testing module without errors.

Accept when:
- The discovered test verification command executes and reports all test suites passing.
- Test execution utilities invoke test lifecycle hooks exclusively through the node:test module.

## Enforcement

- Verified by: Continuous integration automated test suite execution.
- Verified by: Code review of test utility changes and pull requests.
- Violation handling: Pull requests introducing unauthorized external test runner dependencies into test utilities are rejected.
- Exception process: Exceptions must be reviewed and approved by the engineering architecture team with documented justification.