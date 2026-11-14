---
version: 2
name: dfa84b97-3ddc-40db-af53-de8738ab6fd1-adopt-jsr-std-expect-for-test-assertion-matchers-test-utilities-not-introduce-conflicting-assertion
description: "Adopt jsr:@std/expect for Test Assertion Matchers: Test Utilities Not Introduce Conflicting Assertion"
metadata:
  actual:
    adr_id: dfa84b97-3ddc-40db-af53-de8738ab6fd1
    status: accepted
    candidate_group_id: a309ace2-0aca-4547-bae3-648a49462cd1
    evidence:
      source: deep
  scope:
    root: packages/test-utils
    paths:
      - src/**
---

# Adopt jsr:@std/expect for Test Assertion Matchers: Test Utilities Not Introduce Conflicting Assertion

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/test-utils`

Applies to:
- `src/**`

## Context

- Test utility modules require assertion matchers to validate behavior across test suites
- Static analysis detected the adoption of jsr:@std/expect within test utility matcher code
- The pattern is currently isolated to a single test utility module rather than observed across multiple packages

## Problem Statement

Testing infrastructure requires consistent assertion matcher interfaces across test suites, but ad-hoc assertion libraries risk divergence in failure reporting and matcher ergonomics.

## Decision

1. MUST_NOT: Test utilities MUST NOT introduce conflicting assertion expectation libraries across the workspace.

## Policy Block

- MUST_NOT Test utilities MUST NOT introduce conflicting assertion expectation libraries across the workspace.

In scope:
- Test utility modules authoring custom assertion matchers
- Test suites consuming shared assertion expectation helpers

Out of scope:
- Production application runtime code and services
- External modules and standalone verification scripts operating outside the test harness

Exceptions:
- EXC-20-001: A test harness requires assertions incompatible with the expectation library API

## Rationale

- Adopting jsr:@std/expect provides standard expectation matchers and assertion composition for test utilities
- Centralizing assertion matchers within shared test utilities prevents divergence across individual test files
- Evidence shows detected usage in test utility matchers, though limited evidence scope necessitates low confidence in workspace-wide policy intent

## Consequences

Positive:
- Standardizes assertion matcher syntax across test utility modules
- Provides structured failure reporting and expectation composition for test validation

Negative:
- Couples test assertion suites to the API contracts and lifecycle of the selected expectation library
- Single-file adoption evidence indicates incomplete workspace rollout risk if not uniformly governed

## Alternatives

- Rely solely on language runtime basic assertion functions (rejected)
  Rejected because: Basic assertion functions lack fluent matcher semantics and standardized failure output formatting required for comprehensive test suites
  When valid: Minimal standalone utility scripts without shared test harnesses
- Adopt jsr:@std/expect for assertion matchers in test utility modules (accepted)
  Rejected because: None
  When valid: Test utilities providing shared assertion matchers and expectation functions across test suites

## Risks

- Limited single-file adoption may reflect an isolated experiment rather than an intentional project-wide standard
  Mitigation: Review test modules across the repository to verify whether jsr:@std/expect should be standardized globally or restricted to specific packages
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
- Export custom matchers through shared test utility modules to encapsulate assertion implementation details from individual test suites
- Ensure all custom assertion matchers conform to the expectation interface contract established by the resolved library version

## Continuation Context


Verify commands:
- echo "Discover repository test runner and execute assertion test suites"
- command -v test >/dev/null 2>&1 && echo "Verify test execution using repository build configuration" || echo "Discover and execute test runner from project manifest"

Accept when:
- All assertion matchers defined using jsr:@std/expect execute and pass within the repository test runner
- No conflicting assertion expectation libraries are declared within the project dependency manifest or lock artifact

## Enforcement

- Verified by: Automated dependency audit of manifest and lock artifacts during integration pipelines
- Verified by: Peer review verification during code changes in test utility modules
- Violation handling: Flag unapproved assertion libraries during code review and pipeline checks
- Violation handling: Require migration of non-conforming matchers to the approved expectation library before merging
- Exception process: Submit an architectural variance request detailing technical incompatibility with jsr:@std/expect
- Exception process: Obtain review and approval from the engineering team before introducing alternative assertion libraries