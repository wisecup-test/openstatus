---
version: 2
name: 98c7ae13-0d11-4bd7-9320-1b4c1eb3e986-direct-process-env-access-for-webhook-secret-configuration-before-introducing-modifying-versioned-dependency-packages
description: "Direct process.env Access for Webhook Secret Configuration: Before Introducing Modifying Versioned Dependency Packages"
metadata:
  actual:
    adr_id: 98c7ae13-0d11-4bd7-9320-1b4c1eb3e986
    status: accepted
    candidate_group_id: b3b62330-898a-4005-92ab-1844d1a6e727
    evidence:
      source: deep
  scope:
    root: packages/notifications/ms-teams
    paths:
      - src/**
---

# Direct process.env Access for Webhook Secret Configuration: Before Introducing Modifying Versioned Dependency Packages

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/ms-teams`

Applies to:
- `src/**`

## Context

- Notification integration modules require external destination credentials such as webhook URLs to deliver outbound messages.
- Runtime configuration sources currently supply secrets through direct property lookups on the process environment object.
- Static detection identified ad-hoc access to secret environment variables without an intermediary secret management abstraction or schema validation boundary.

## Problem Statement

Directly accessing sensitive integration credentials from the global runtime environment couples notification components to ambient execution context, prevents centralized secret validation, and increases the risk of credential leakage across system boundaries.

## Decision

1. MUST: Before introducing or modifying versioned dependency packages for secret handling or schema validation, developers MUST discover the workspace lock artifact and ensure exact resolved version parity.

## Policy Block

- MUST Before introducing or modifying versioned dependency packages for secret handling or schema validation, developers MUST discover the workspace lock artifact and ensure exact resolved version parity.

In scope:
- Outbound notification integrations and credential-dependent delivery modules requiring destination webhook secrets.

Out of scope:
- Internal in-memory mock fixtures that do not communicate with external services or handle production credentials.

## Rationale

- Direct runtime environment access bypasses structural schema enforcement and leaks ambient execution state into domain and integration components.
- Centralizing secret retrieval behind a validated configuration boundary prevents silent runtime failures caused by missing or malformed credentials.
- Preventing raw credential exposure in diagnostic logging protects sensitive webhook endpoints from inadvertent logging pipeline ingestion.

## Consequences

Positive:
- Eliminates scattered ambient environment variable queries across integration packages.
- Ensures fail-fast startup validation for missing or invalid webhook credentials.
- Reduces the risk of credential exposure in diagnostic logs and error traces.

Negative:
- Requires defining explicit configuration contracts and schemas before using external credentials.
- Introduces minor boilerplate when setting up isolated testing or development environments.

## Alternatives

- Direct ambient process environment access across all integration packages (rejected)
  Rejected because: Bypasses startup schema validation and scatters credential lookups across codebases without centralized governance.
  When valid: Single-file prototypes where configuration abstraction introduces unnecessary overhead.
- Centralized configuration service with schema validation and secret masking (accepted)
  When valid: All production and integration packages requiring structured, validated secrets.

## Risks

- Unvalidated environment variables can cause runtime dispatch failures when secrets are absent.
  Mitigation: Enforce startup schema validation that aborts initialization if required secrets are missing.
  Owner: engineering team
- Diagnostic logging on dispatch failures could inadvertently expose sensitive webhook targets.
  Mitigation: Sanitize and redact credential paths from error messages before emitting diagnostic logs.
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
- Inspect the repository configuration structures to discover existing schema definitions and configuration retrieval patterns for environment secrets.
- Ensure test suites verify that missing credentials trigger controlled validation errors rather than unexpected runtime exceptions.

## Continuation Context


Verify commands:
- Discover and run the project repository's linting script to check for unauthorized direct environment variable access.
- Discover and execute the project's test suite to verify that configuration schema validation and credential masking pass.

Accept when:
- Direct queries to process environment variables in integration modules are replaced by validated configuration access.
- Static verification and automated test suites complete with zero errors or warnings.

## Enforcement

- Verified by: Automated static analysis checks in continuous integration pipelines.
- Verified by: Peer code review for pull requests modifying secret handling or notification integrations.
- Violation handling: Continuous integration blocks pull requests that introduce direct ambient environment variable access.
- Violation handling: Code review requests revisions to route credential retrieval through validated configuration interfaces.
- Exception process: Submit an architectural review request documenting why an isolated integration or mock component requires direct ambient access, approved by the security team.