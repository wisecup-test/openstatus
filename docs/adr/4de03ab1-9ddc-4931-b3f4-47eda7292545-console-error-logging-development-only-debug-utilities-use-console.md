---
version: 2
name: 4de03ab1-9ddc-4931-b3f4-47eda7292545-console-error-logging-development-only-debug-utilities-use-console
description: "console.error Logging: Development Only Debug Utilities Use Console"
metadata:
  actual:
    adr_id: 4de03ab1-9ddc-4931-b3f4-47eda7292545
    status: accepted
    candidate_group_id: 67cb9726-3590-4f64-8f42-95e6f1534ece
    evidence:
      source: deep
  scope:
    root: packages/assertions
    paths:
      - src/**
---

# console.error Logging: Development Only Debug Utilities Use Console

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/assertions`

Applies to:
- `src/**`

## Context

- Runtime execution failures during payload parsing in assertion workflows require diagnostic output to signal parsing errors.
- Diagnostic error output is currently emitted directly using runtime standard error logging via console.error rather than routing through a structured logging subsystem.
- The codebase contains an isolated instance of direct console error logging, reflecting an uncoordinated diagnostic approach without project-wide logging infrastructure.

## Problem Statement

Parsing failures during assertion evaluation must be surfaced for operational diagnostics, but invoking global console logging directly produces unstructured output that bypasses centralized log ingestion, filtering, and telemetry formatting.

## Decision

1. MAY: Development-only debug utilities MAY use console.error during local debugging sessions when gated behind development environment conditional flags.

## Policy Block

- MAY Development-only debug utilities MAY use console.error during local debugging sessions when gated behind development environment conditional flags.

In scope:
- Runtime payload parsing and error handling in assertion modules
- Diagnostic output generated during validation failure paths

Out of scope:
- Local automated test runner scaffolding
- Interactive command-line developer scripts operating outside application runtime

Exceptions:
- EX-46-001: A critical runtime bootstrap error occurs prior to initialization of the primary logging subsystem

## Rationale

- Direct invocations of console.error produce unformatted text streams that cannot be parsed by automated log collectors or associated with distributed request contexts.
- Restricting unstructured runtime console calls ensures that operational errors are observable, searchable, and actionable across environments.
- The detected pattern represents a single ad-hoc diagnostic call rather than an intentional logging architecture, warranting clear boundaries against proliferated console logging.

## Consequences

Positive:
- Diagnostic emissions maintain standardized structured formats suitable for downstream indexing and alerting.
- Application modules decouple error reporting from runtime global console objects, improving modular testability.
- Operational monitoring gains consistent error categorization and severity filtering across services.

Negative:
- Modules handling parse failures must establish or receive structured diagnostic handlers rather than relying on global runtime objects.
- Local diagnostic inspection requires configured logging handlers or explicit environment flags rather than direct console output.

## Alternatives

- Permit direct console.error calls across all assertion modules (rejected)
  Rejected because: Unstructured string logging to standard error prevents centralized log aggregation, context tracking, and automated severity alerts.
  When valid: Valid only in standalone prototype scripts that do not integrate into broader service architectures.
- Adopt an enterprise structured logging client with distributed tracing metadata (deferred)
  Rejected because: Current evidence reflects minimal diagnostic logging requirements across isolated assertion routines, making immediate enterprise logging framework adoption disproportionately complex.
  When valid: Valid when application modules require multi-transport log routing, metric aggregation, and distributed trace propagation.

## Risks

- Developers continue inserting console.error statements for rapid debugging, leading to unindexed error leaks in production.
  Mitigation: Configure automated static analysis rules to flag direct console invocations during continuous integration.
  Owner: Engineering team
- Suppressing direct console.error in parsing routines without structured handlers could cause silent failures.
  Mitigation: Ensure parsing routines either raise catchable structured errors or route to designated fallback diagnostic handlers.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Search the codebase for direct console invocations and replace ad-hoc error printing with structured logger calls or structured domain error throws.
- Ensure all log statements include contextual error identifiers and avoid dumping unstructured strings into process streams.

## Continuation Context


Verify commands:
- Discover the project linting and static analysis scripts from the dependency manifest and execute them to verify that no unauthorized console invocations exist.
- Discover and run the project test suite using the project build orchestration scripts to verify that assertion parsing error paths behave correctly without direct console output.

Accept when:
- The static analysis verification script executes without reporting unauthorized console.error usage in production modules.
- All unit and integration tests for payload parsing error handling pass successfully.

## Enforcement

- Verified by: Continuous integration automated static analysis checks
- Verified by: Pull request peer review inspecting error handling paths
- Violation handling: Pull requests containing unapproved direct console.error calls are blocked from merging until resolved or granted an exception.
- Violation handling: Static analysis warnings must be resolved prior to release build completion.
- Exception process: Submit an architectural exception request detailing the critical bootstrapping constraints that necessitate direct runtime console output.
- Exception process: Review and approval required from the lead architecture reviewer.