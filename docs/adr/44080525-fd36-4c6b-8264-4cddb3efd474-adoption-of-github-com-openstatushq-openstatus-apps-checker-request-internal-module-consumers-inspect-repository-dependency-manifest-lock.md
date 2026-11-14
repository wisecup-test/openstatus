---
version: 2
name: 44080525-fd36-4c6b-8264-4cddb3efd474-adoption-of-github-com-openstatushq-openstatus-apps-checker-request-internal-module-consumers-inspect-repository-dependency-manifest-lock
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Consumers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 44080525-fd36-4c6b-8264-4cddb3efd474
    status: accepted
    candidate_group_id: 8890ae36-f253-45a2-a72a-0cb76b733ce3
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/assertions/**
      - pkg/otel/**
---

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Consumers Inspect Repository Dependency Manifest Lock

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/checker`

Applies to:
- `pkg/assertions/**`
- `pkg/otel/**`

## Context

- The checker service coordinates network probe execution, telemetry recording, and assertion validation across multiple protocols.
- Both telemetry recording and assertion evaluation require detailed access to request metadata, response status, headers, and body records.
- Multiple packages within the checker application import github.com/openstatushq/openstatus/apps/checker/request to process probe execution transactions.

## Problem Statement

Without a canonical internal request representation, distinct subsystems within the checker application would either implement fragmented data structures for network transactions or pass unstructured payloads, causing structural drift and redundant translation layers between telemetry recording and assertion evaluation.

## Decision

1. MUST: Consumers MUST inspect the repository dependency manifest and lock artifact to resolve and verify exact locked versions before implementing or updating code using versioned dependencies.

## Policy Block

- MUST Consumers MUST inspect the repository dependency manifest and lock artifact to resolve and verify exact locked versions before implementing or updating code using versioned dependencies.

In scope:
- All packages and modules within the checker application that inspect, record, or evaluate network request and response data.

Out of scope:
- External client packages or unrelated workspace services that do not participate in checker probe execution workflows.

## Rationale

- Adopting a dedicated internal module centralizes the domain definitions for requests, responses, and evaluation records in one maintainable boundary.
- Observed consumer contracts across telemetry and assertion packages consistently rely on unified request structures, confirming that request representation is a shared domain concern.
- Static analysis validates high co-occurrence and significance for the internal request library across multiple checker packages.

## Consequences

Positive:
- Provides a unified domain representation for network requests and responses across checker components.
- Eliminates duplicate parsing and data structure mapping between telemetry collection and assertion evaluation pipelines.
- Ensures consistency in how protocol metrics and assertion evaluations interpret request execution results.

Negative:
- Consumer packages are coupled to the interface contract of the shared request module, requiring coordinated updates when data structures change.
- Enhancements to specialized evaluation or telemetry features must be designed to avoid polluting the core request model with package-specific concerns.

## Alternatives

- Define independent request and response structures inside each consuming subsystem package (rejected)
  Rejected because: Introduces duplicate structural definitions that drift over time and require redundant mapping layers between probe execution, telemetry recording, and assertion evaluation.
  When valid: Only valid when consumer packages require decoupled protocol models that intentionally deviate from standard checker request semantics.
- Pass unstructured generic map and byte payload types across subsystem boundaries (rejected)
  Rejected because: Eliminates compile-time type safety, increases runtime deserialization overhead, and complicates assertion evaluation error reporting.
  When valid: Only valid when processing dynamic, schema-less payloads whose structure cannot be defined ahead of time.

## Risks

- Breaking changes to data structures in the shared request module can simultaneously break telemetry recording and assertion evaluation.
  Mitigation: Enforce semantic versioning discipline and comprehensive regression test suites for the shared request package across all consumer modules.
  Owner: engineering team
- Over-generalizing the request model to accommodate every protocol metric may lead to an unwieldy and bloated domain structure.
  Mitigation: Design the request module around lean, protocol-neutral core interfaces and use composable extension types for specialized protocol attributes.
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
- Implementations adding new protocol support must expand the shared request module rather than creating isolated protocol representations.
- Helper routines for assertion evaluation and metric extraction must consume read-only views or standard accessor methods provided by the shared module.

## Continuation Context


Verify commands:
- Discover the workspace test runner scripts from the repository configuration and execute unit and integration test suites covering checker subsystem packages.
- Run the project dependency and static analysis verification scripts to confirm import compliance and verify the absence of circular dependencies.

Accept when:
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

## Enforcement

- Verified by: Automated continuous integration checks that validate module dependency trees and test suites across all checker packages.
- Verified by: Peer code review for all pull requests modifying imports or defining request-handling contracts within the checker application.
- Violation handling: Pull requests introducing localized duplicate request models or unapproved protocol structs will be blocked in code review.
- Violation handling: Violations flagged by dependency linters must be refactored to use the canonical internal module before merge.
- Exception process: Exceptions to defining custom request data models must be submitted via architectural review with documented justification for protocol deviations.
- Exception process: Temporary adapters must be recorded in the issue tracker with a planned migration path back to the canonical module.