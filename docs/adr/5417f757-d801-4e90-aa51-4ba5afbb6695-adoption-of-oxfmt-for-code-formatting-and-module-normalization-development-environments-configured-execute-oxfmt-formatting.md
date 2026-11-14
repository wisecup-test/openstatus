---
version: 2
name: 5417f757-d801-4e90-aa51-4ba5afbb6695-adoption-of-oxfmt-for-code-formatting-and-module-normalization-development-environments-configured-execute-oxfmt-formatting
description: "Adoption of oxfmt for Code Formatting and Module Normalization: Development Environments Configured Execute Oxfmt Formatting"
metadata:
  actual:
    adr_id: 5417f757-d801-4e90-aa51-4ba5afbb6695
    status: accepted
    candidate_group_id: 51a59959-4b9a-4c8b-af08-2ccfb27e6231
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

# Adoption of oxfmt for Code Formatting and Module Normalization: Development Environments Configured Execute Oxfmt Formatting

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

- The project requires deterministic, automated code formatting and syntax normalization across all source modules to preserve uniform presentation.
- A dedicated configuration artifact establishes centralized formatting rules and options using oxfmt.
- Standardizing code presentation through an automated formatting library eliminates stylistic variation and streamlines collaboration across distributed changes.

## Problem Statement

Inconsistent code styling across modules increases review overhead, generates cosmetic diff churn, and obscures meaningful functional changes. A centralized, authoritative formatting configuration is necessary to guarantee predictable, uniform source representation across the codebase.

## Decision

1. SHOULD: Development environments SHOULD be configured to execute oxfmt formatting automatically upon file save to maintain continuous compliance.

## Policy Block

- SHOULD Development environments SHOULD be configured to execute oxfmt formatting automatically upon file save to maintain continuous compliance.

In scope:
- All first-party source code modules maintained within the repository.
- Automated formatting and code style verification routines within local development and pipeline workflows.

Out of scope:
- Third-party vendor libraries and auto-generated source artifacts designated in repository exclusion lists.

Exceptions:
- EXC-20-001: Third-party vendor bundles or auto-generated source files cannot conform to oxfmt formatting rules without syntax disruption.

## Rationale

- Adopting oxfmt provides automated, uniform formatting across the codebase, eliminating manual style debates during reviews.
- Consolidating configuration into a single authoritative module guarantees consistent rule application across both local and automated environments.
- Automated formatting enforcement ensures long-term repository maintainability with minimal manual developer overhead.

## Consequences

Positive:
- Guarantees deterministic and consistent code formatting across all project modules.
- Eliminates cosmetic code style debates and review friction during code integration.
- Reduces non-semantic diff churn in source control revision history.

Negative:
- Couples repository formatting workflows to the specific configuration schema and syntax rules of oxfmt.
- Requires all development environments and automated pipelines to maintain synchronized formatting runtimes matching the locked dependency version.
- May produce repository-wide formatting diffs when updating formatting engine rules.

## Alternatives

- Manual code formatting guided by written style documentation (rejected)
  Rejected because: Relies on developer discipline and manual code review scrutiny, leading to inconsistent styling, review friction, and non-semantic diff churn.
  When valid: Small exploratory prototypes or single-contributor projects without automated build pipelines.
- Adoption of multiple competing formatting engines across repository subtrees (rejected)
  Rejected because: Introduces conflicting formatting rules, increases maintenance overhead, and creates inconsistent developer workflows across modules.
  When valid: Polyglot projects where subtrees require language-specific tooling unsupported by oxfmt.

## Risks

- Discrepancies between local formatting runtime versions and pipeline validation environments causing unexpected build failures.
  Mitigation: Enforce strict dependency resolution against the repository lock artifact across all environments.
  Owner: Engineering Team
- Upstream updates to oxfmt introducing breaking formatting rule changes that produce massive codebase diffs.
  Mitigation: Pin dependency versions in the lock artifact and plan formatting updates deliberately across repository revisions.
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
- Locate the project dependency manifest and configuration module to identify active formatting parameters and excluded path patterns.
- Configure local development environments and pre-commit hooks to invoke the discovered formatting routine automatically prior to commit.

## Continuation Context


Verify commands:
- Discover and execute the repository formatting verification script defined in the project manifest.
- Discover and run the project static analysis and formatting check command in non-modifying check mode.

Accept when:
- The discovered formatting verification command completes with zero syntax or style violations and an exit code of zero.
- All committed source files match the formatting rules declared in the repository configuration artifact.

## Enforcement

- Verified by: Continuous integration pipelines executing automated formatting checks on pull requests.
- Verified by: Pre-merge reviews verifying that automated compliance checks pass and no conflicting local formatting configurations are introduced.
- Violation handling: Automated checks reject unformatted or non-compliant pull requests, blocking integration until formatting violations are resolved.
- Violation handling: Developers must run the discovered repository formatting task to normalize affected files before merging.
- Exception process: Exceptions for third-party or generated assets must be proposed via pull request updating the configuration ignore patterns, requiring explicit review and approval by repository maintainers.