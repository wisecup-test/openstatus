---
version: 2
name: 66fcca47-c77d-4410-bdaf-dfd93b05696c-adoption-of-internal-utilities-module-utils-for-shared-operations-domain-feature-modules-not-duplicate-date
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: Domain Feature Modules Not Duplicate Date"
metadata:
  actual:
    adr_id: 66fcca47-c77d-4410-bdaf-dfd93b05696c
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: Domain Feature Modules Not Duplicate Date

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tracker`

Applies to:
- `src/**`

## Context

- Domain processing and event filtering logic within the tracking subsystem require recurring temporal operations, including day-level date comparisons to evaluate blacklist criteria.
- Ad-hoc implementations of date comparison and calendar calculations across disparate domain modules introduce risks of drift, subtle timezone discrepancy, and logic duplication.
- The tracking subsystem standardizes common calculation and helper logic by encapsulating shared routines within an internal utilities module rather than permitting independent utility implementations across leaf modules.
- Temporal evaluation routines within blacklist filtering invoke functional comparison operations such as isSameDay exposed by the internal utilities module.

## Problem Statement

Dispersed domain modules within the tracking subsystem risk fragmenting recurring helper logic and date calculations when utility routines are implemented ad-hoc or duplicated across domain components. Without a centralized internal utilities module, slight variations in date boundary evaluation lead to subtle discrepancies in blacklist filtering and event tracking.

## Decision

1. MUST_NOT: Domain feature modules MUST NOT duplicate date arithmetic or parse raw date representations independently when corresponding helper capabilities exist in the internal utilities module.

## Policy Block

- MUST_NOT Domain feature modules MUST NOT duplicate date arithmetic or parse raw date representations independently when corresponding helper capabilities exist in the internal utilities module.

In scope:
- Code modules within the tracking subsystem implementing event evaluation, filtering, or temporal boundary checks.
- Internal utility definitions and shared helper functions supporting tracking domain modules.

Out of scope:
- Independent workspace services that maintain their own isolated domain-specific utility boundaries.
- External database client configurations and schema definitions managed by dedicated data layer packages.

Exceptions:
- EXC-20-001: A module requires specialized high-performance temporal arithmetic that cannot be accommodated by the standard isSameDay comparison signature

## Rationale

- Encapsulating common date comparison logic such as isSameDay inside a dedicated internal utility module establishes a single point of definition and maintenance for temporal calculations across the tracking subsystem.
- Standardizing helper operations prevents divergence in date evaluation semantics between blacklist verification routines and event tracking pipelines.
- Isolating utility routines in an internal module minimizes direct coupling of domain logic to low-level date manipulation APIs or external library abstractions.

## Consequences

Positive:
- Consistent evaluation of date boundaries across all tracking and blacklist filtering modules.
- Centralized unit testing surface for utility routines such as isSameDay without requiring full domain pipeline execution.
- Reduced code duplication and lower maintenance overhead across tracking subsystem modules.

Negative:
- Developers must maintain awareness of available internal utility exports to prevent accidental re-implementation of existing helpers.
- Modifications to shared utility helper signatures require regression verification across all importing tracking modules.

## Alternatives

- Inline date arithmetic and local utility definitions within each domain feature module (rejected)
  Rejected because: Inlining date comparison logic creates duplicate code paths, increases maintenance overhead, and elevates the risk of inconsistent date matching behavior across blacklist and tracking components
  When valid: Prototyping isolated one-off scripts where sharing code across modules is not required
- Direct third-party date library imports across all individual leaf modules (rejected)
  Rejected because: Direct leaf-level imports increase coupling to external dependency APIs and disperse version upgrade maintenance across many domain modules
  When valid: When a module requires specialized external library capabilities not appropriate for general internal utility encapsulation

## Risks

- Subtle breaking changes in utility functions like isSameDay could cascade into tracking or blacklist filtering regressions
  Mitigation: Enforce comprehensive automated regression unit test suites for all exported utility helpers and dependent tracking workflows
  Owner: Tracking subsystem engineering team
- Uncontrolled accumulation of unrelated helper functions inside the internal utilities module leading to low cohesion
  Mitigation: Conduct periodic module boundary reviews to ensure utility functions remain cohesive, pure, and strictly scoped to shared operations
  Owner: Subsystem maintainers

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Inspect the tracking subsystem module structure to identify existing shared helper functions before introducing new operations.
- Ensure that date comparison helpers such as isSameDay handle edge cases including midnight boundary transitions and object instantiation consistently.

## Continuation Context


Verify commands:
- Discover the project verification script from the repository manifest and execute the test runner to validate utility function behavior and consumer module regression suites.
- Inspect the project dependency manifest to locate the static analysis and linting scripts, then run them to confirm proper internal module import adherence across tracking components.

Accept when:
- All unit tests covering internal utility functions and their consumer modules pass without failures.
- Static analysis checks verify that tracking domain modules import common date helpers from the internal utilities module rather than defining duplicate local functions.

## Enforcement

- Verified by: Continuous integration test validation and automated static analysis suites
- Verified by: Peer code review verification on all pull requests modifying tracking components or utility helpers
- Violation handling: Pull requests introducing duplicate utility logic or bypassing internal utility modules are blocked during code review until refactored
- Violation handling: Continuous integration failures on regression or static analysis checks prevent merging to protected branches
- Exception process: Submit an architectural review request detailing performance or functional requirements that justify bypassing standard utility functions, accompanied by sign-off from component owners