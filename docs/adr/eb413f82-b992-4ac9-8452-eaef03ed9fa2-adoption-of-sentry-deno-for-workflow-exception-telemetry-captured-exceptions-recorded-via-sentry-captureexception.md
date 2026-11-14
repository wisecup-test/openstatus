---
version: 2
name: eb413f82-b992-4ac9-8452-eaef03ed9fa2-adoption-of-sentry-deno-for-workflow-exception-telemetry-captured-exceptions-recorded-via-sentry-captureexception
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Captured Exceptions Recorded Via Sentry Captureexception"
metadata:
  actual:
    adr_id: eb413f82-b992-4ac9-8452-eaef03ed9fa2
    status: accepted
    candidate_group_id: 0a970db4-4f7d-45c5-a8bb-cf5b2fe6222e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/lib/**
---

# Adoption of @sentry/deno for Workflow Exception Telemetry: Captured Exceptions Recorded Via Sentry Captureexception

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/cron/**`
- `src/lib/**`

## Context

- Scheduled cron jobs and background workflow handlers execute asynchronously where unhandled failures can occur silently without real-time interactive user feedback.
- Background tasks require error tracking that integrates with the underlying Deno runtime environment and exports structured operational metadata for alerting and triage.
- Inconsistent error capture structures across workflow handlers hinder post-incident root cause analysis and metric aggregation.

## Problem Statement

Background workflow execution and scheduled cron tasks require uniform, centralized error reporting capable of operating within the Deno runtime environment. Without a standardized exception tracking client and consistent contextual tagging, operational failures across asynchronous jobs lack actionable diagnostic metadata and alert correlation.

## Decision

1. MUST: Captured exceptions MUST be recorded via Sentry.captureException and MUST supply contextual operational tags defining the job classification, execution phase, and operational entity identifier.

## Policy Block

- MUST Captured exceptions MUST be recorded via Sentry.captureException and MUST supply contextual operational tags defining the job classification, execution phase, and operational entity identifier.

In scope:
- Asynchronous workflow runners, background cron tasks, and scheduled job handlers within workflow services.

Out of scope:
- Client-side web applications and services executing outside the designated workflow runtime environment.

## Rationale

- Standardizing on @sentry/deno ensures native compatibility with the workflow execution runtime while providing dedicated exception capture APIs.
- Enforcing structured tags via Sentry.captureException allows automated filtering, alerting, and triage based on task phase, job slug, and error category.
- Decoupling error telemetry dispatch from console logging ensures that operational failures are preserved and indexed in centralized monitoring infrastructure.

## Consequences

Positive:
- Provides centralized visibility and automated triage capabilities for asynchronous cron workflows.
- Standardizes error context structure with consistent job phase, slug, and operational tags across handlers.
- Isolates runtime-compatible telemetry instrumentation to the designated runtime client.

Negative:
- Couples background workflow execution services to an external telemetry provider and third-party monitoring infrastructure.
- Requires continuous maintenance of metadata tagging conventions across disparate workflow phases and job handlers.

## Alternatives

- Unstructured console output logging without telemetry client (rejected)
  Rejected because: Lacks centralized aggregation, structured operational tagging, and automated alerting for asynchronous background executions.
  When valid: Acceptable only in isolated local development environments where external network telemetry is unavailable.
- Generic Node runtime error tracking SDK (rejected)
  Rejected because: Workflow services run within a Deno execution environment that requires native runtime bindings and compatible platform integrations.
  When valid: Valid only if the entire workflow execution service is migrated to a standard Node runtime container.

## Risks

- Sensitive authorization secrets or private configuration values might leak into exception tags or extra payload metadata.
  Mitigation: Enforce payload sanitization rules that filter sensitive headers, authorization credentials, and environment secrets prior to telemetry transmission.
  Owner: engineering team
- Network disruptions to the telemetry ingestion endpoint could introduce latency or secondary exceptions into workflow execution.
  Mitigation: Implement non-blocking asynchronous exception dispatch and error boundary safeguards so telemetry transmission failures do not interrupt job execution.
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
- Centralize common error capture helpers within a shared workflow utility module to ensure uniform tag formatting across cron and checker tasks.
- Configure telemetry dispatch to operate asynchronously to prevent monitoring network latency from impacting workflow execution timing.

## Continuation Context


Verify commands:
- Discover the project workspace test runner and execute the automated test suite covering workflow error handling and exception capture.
- Discover the project linter and static analysis runner, then execute code validation against workflow telemetry integrations.

Accept when:
- The automated test suite verifies that error handling routines invoke exception capture with required contextual tags.
- Static analysis and dependency verification confirm that error tracking imports resolve exclusively to the adopted runtime SDK without unverified version discrepancies.

## Enforcement

- Verified by: Static analysis and type checking verifying correct SDK imports and payload structures.
- Verified by: Automated test suite coverage validating that error handling paths dispatch telemetry calls with required metadata tags.
- Verified by: Peer code reviews auditing error handling boundaries in workflow and cron service modules.
- Violation handling: Pull requests introducing uninstrumented error catching or alternative telemetry clients without prior architectural approval are blocked by automated continuous integration checks.
- Exception process: Submit an architectural exception request detailing runtime constraints or alternative telemetry requirements to the engineering team for formal review.