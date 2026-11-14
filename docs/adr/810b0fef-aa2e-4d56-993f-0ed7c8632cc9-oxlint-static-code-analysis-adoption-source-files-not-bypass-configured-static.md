---
version: 2
name: 810b0fef-aa2e-4d56-993f-0ed7c8632cc9-oxlint-static-code-analysis-adoption-source-files-not-bypass-configured-static
description: "Oxlint Static Code Analysis Adoption: Source Files Not Bypass Configured Static"
metadata:
  actual:
    adr_id: 810b0fef-aa2e-4d56-993f-0ed7c8632cc9
    status: accepted
    candidate_group_id: 03d03f36-df4f-4d82-b61c-15651f701d85
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - "**"
      - "!apps/checker/**"
      - "!apps/dashboard/**"
      - "!apps/private-location/**"
      - "!apps/railway-proxy/**"
      - "!apps/screenshot-service/**"
      - "!apps/server/**"
      - "!apps/ssh-server/**"
      - "!apps/status-page/**"
      - "!apps/web/**"
      - "!apps/workflows/**"
      - "!packages/ai/**"
      - "!packages/analytics/**"
      - "!packages/api/**"
      - "!packages/assertions/**"
      - "!packages/db/**"
      - "!packages/emails/**"
      - "!packages/error/**"
      - "!packages/header-analysis/**"
      - "!packages/icons/**"
      - "!packages/importers/**"
      - "!packages/locales/**"
      - "!packages/notifications/base/**"
      - "!packages/notifications/bird-whatsapp/**"
      - "!packages/notifications/discord/**"
      - "!packages/notifications/email/**"
      - "!packages/notifications/google-chat/**"
      - "!packages/notifications/grafana-oncall/**"
      - "!packages/notifications/ms-teams/**"
      - "!packages/notifications/ntfy/**"
      - "!packages/notifications/opsgenie/**"
      - "!packages/notifications/pagerduty/**"
      - "!packages/notifications/slack/**"
      - "!packages/notifications/telegram/**"
      - "!packages/notifications/twillio-sms/**"
      - "!packages/notifications/webhook/**"
      - "!packages/proto/**"
      - "!packages/react/**"
      - "!packages/regions/**"
      - "!packages/services/**"
      - "!packages/status-fetcher/**"
      - "!packages/subscriptions/**"
      - "!packages/test-utils/**"
      - "!packages/theme-store/**"
      - "!packages/tinybird/**"
      - "!packages/tracker/**"
      - "!packages/tsconfig/**"
      - "!packages/ui/**"
      - "!packages/upstash/**"
      - "!packages/utils/**"
---

# Oxlint Static Code Analysis Adoption: Source Files Not Bypass Configured Static

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `.` (whole repository)

Applies to:
- `**` (whole repository)

Excludes:
- `apps/checker/**`
- `apps/dashboard/**`
- `apps/private-location/**`
- `apps/railway-proxy/**`
- `apps/screenshot-service/**`
- `apps/server/**`
- `apps/ssh-server/**`
- `apps/status-page/**`
- `apps/web/**`
- `apps/workflows/**`
- `packages/ai/**`
- `packages/analytics/**`
- `packages/api/**`
- `packages/assertions/**`
- `packages/db/**`
- `packages/emails/**`
- `packages/error/**`
- `packages/header-analysis/**`
- `packages/icons/**`
- `packages/importers/**`
- `packages/locales/**`
- `packages/notifications/base/**`
- `packages/notifications/bird-whatsapp/**`
- `packages/notifications/discord/**`
- `packages/notifications/email/**`
- `packages/notifications/google-chat/**`
- `packages/notifications/grafana-oncall/**`
- `packages/notifications/ms-teams/**`
- `packages/notifications/ntfy/**`
- `packages/notifications/opsgenie/**`
- `packages/notifications/pagerduty/**`
- `packages/notifications/slack/**`
- `packages/notifications/telegram/**`
- `packages/notifications/twillio-sms/**`
- `packages/notifications/webhook/**`
- `packages/proto/**`
- `packages/react/**`
- `packages/regions/**`
- `packages/services/**`
- `packages/status-fetcher/**`
- `packages/subscriptions/**`
- `packages/test-utils/**`
- `packages/theme-store/**`
- `packages/tinybird/**`
- `packages/tracker/**`
- `packages/tsconfig/**`
- `packages/ui/**`
- `packages/upstash/**`
- `packages/utils/**`

## Context

- A dedicated static analysis configuration module was detected in the repository, establishing automated source code verification.
- Static analysis is required to identify syntax defects, maintain structural consistency, and enforce language-level constraints before runtime compilation.
- Centralized configuration ensures uniform rule evaluation across all contributors and automated verification environments.

## Problem Statement

Source code quality and idiom enforcement risk divergence without a centralized, automated static analysis engine to validate source files across the codebase.

## Decision

1. MUST_NOT: Source files MUST NOT bypass configured static analysis rules without documenting the rationale and obtaining required architectural approval.

## Policy Block

- MUST_NOT Source files MUST NOT bypass configured static analysis rules without documenting the rationale and obtaining required architectural approval.

In scope:
- All source files and modules managed within the repository subject to static analysis.
- Continuous integration and automated verification pipelines executing code validation.

Out of scope:
- Generated build artifacts, distribution bundles, and third-party vendor directories.

Exceptions:
- EXC-20-001: Legacy or vendor-integrated files require temporary diagnostic suppression.

## Rationale

- The IR evidence confirms adoption of Oxlint through a dedicated configuration module governing repository-wide code health.
- Adopting a dedicated static analysis configuration establishes consistent code standards and catches common errors early in the development lifecycle.
- A unified configuration prevents divergent local developer settings and guarantees deterministic evaluation in continuous integration.

## Consequences

Positive:
- Consistent automated rule enforcement across all source files in the repository.
- Reduced review overhead for mechanical syntax and stylistic standards.
- Early detection of syntax anomalies and potential runtime bugs prior to compilation.

Negative:
- Maintenance overhead required to adjust rules as language standards and library APIs evolve.
- Potential friction when introducing stricter rule sets across pre-existing code modules.

## Alternatives

- Compiler-only type checking without a dedicated static analysis engine (rejected)
  Rejected because: Compiler type checking does not enforce stylistic consistency, code health heuristics, or idiomatic design patterns.
  When valid: Prototyping environments where rapid iteration precedes formal static quality gates.
- Traditional ecosystem static analysis runtimes (rejected)
  Rejected because: Higher execution latency and complex configuration dependency graphs compared to modern unified analysis engines.
  When valid: Environments requiring legacy custom plugin ecosystems not supported by modern static analysis engines.

## Risks

- Overly aggressive rule sets may impede development velocity and introduce false-positive diagnostic failures.
  Mitigation: Establish a phased rule rollout with initial warning levels prior to enforcing hard build failures.
  Owner: Engineering Team
- Version discrepancies between local developer environments and automated verification pipelines.
  Mitigation: Strictly enforce lock-file resolution and verify lock integrity during automated builds.
  Owner: Platform Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the repository static analysis configuration module and ensure rules align with project domain standards.
- Integrate static analysis execution into local pre-commit hooks and continuous integration workflows.

## Continuation Context


Verify commands:
- Discover the static analysis verification script from the repository manifest and execute it against all source files.
- Inspect the continuous integration pipeline configuration to locate the linting check target and validate execution.

Accept when:
- Static analysis completes across all in-scope source files with zero diagnostic errors.
- The static analysis configuration module is validated by the execution engine without syntax or schema warnings.

## Enforcement

- Verified by: Automated continuous integration pipeline checks on every pull request.
- Verified by: Peer code review verification of any static analysis rule modifications.
- Violation handling: Pull requests containing static analysis violations fail automated checks and cannot be merged.
- Violation handling: Unapproved inline rule suppression comments are flagged and rejected during review.
- Exception process: Submit an architectural review request detailing the rule conflict, affected files, and proposed timeline for remediation.