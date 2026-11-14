---
version: 2
name: 251e6d54-182e-4895-ac2f-9eec0b991680-resend-email-client-error-logging-via-console-consumers-discover-repository-lock-artifact-inspect
description: "Resend Email Client Error Logging via Console: Consumers Discover Repository Lock Artifact Inspect"
metadata:
  actual:
    adr_id: 251e6d54-182e-4895-ac2f-9eec0b991680
    status: accepted
    candidate_group_id: ba818162-80dd-4d64-adf5-c2c7986fed07
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - src/**
---

# Resend Email Client Error Logging via Console: Consumers Discover Repository Lock Artifact Inspect

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/emails`

Applies to:
- `src/**`

## Context

- The email integration package dispatches transactional messages and invitations through an external email delivery API.
- When email transmission or batch operations encounter rejections or runtime exceptions, failures require immediate visibility for debugging.
- Current implementations directly invoke runtime console error methods with recipient context and error payloads to log failures to standard error.

## Problem Statement

Directly invoking unstructured console logging for email dispatch rejections provides immediate runtime failure visibility but lacks structured log metadata, level control, and consistent tracing integration across application boundaries.

## Decision

1. MUST: Consumers MUST discover the repository lock artifact and inspect the exact resolved version of Resend and related email libraries before implementing or modifying email dispatch routines.

## Policy Block

- MUST Consumers MUST discover the repository lock artifact and inspect the exact resolved version of Resend and related email libraries before implementing or modifying email dispatch routines.

In scope:
- Email dispatch client modules handling transactional and notification email delivery.

Out of scope:
- General application business logic and external network clients outside the email subsystem.

## Rationale

- Emitting transmission errors directly to standard error ensures operational failures are captured by runtime container log collectors without external logging dependencies.
- Associating the recipient identifier with error outputs allows operators to identify failed delivery attempts during troubleshooting.
- Adhering to lock-file version discovery prevents API drift and breaking signature changes in third-party client integrations.

## Consequences

Positive:
- Delivery failures are immediately visible in runtime process output without introducing auxiliary logging infrastructure.
- Debugging failed email dispatches is streamlined by associating recipient metadata directly with logged error strings.

Negative:
- Unstructured log strings impede automated parsing, log querying, and alerting in downstream log aggregation systems.
- Uncontrolled console logging risks emitting sensitive recipient or metadata information into process logs.

## Alternatives

- Structured logging framework adoption across workspace modules (deferred)
  Rejected because: Workspace-wide structured logging requires centralized configuration and shared schema conventions that are not yet established across all packages.
  When valid: Valid when a centralized observability pipeline and structured log collector are provisioned across all services.
- Silent failure suppression within the email client (rejected)
  Rejected because: Suppressing transmission failures leaves delivery dropouts undetected and obstructs incident diagnosis.
  When valid: Never valid for outbound notification and invitation delivery workflows.

## Risks

- Recipient email addresses logged to standard error could be retained in unsecured process log sinks.
  Mitigation: Ensure log ingestion pipelines apply automated masking or data retention policies to process outputs.
  Owner: Engineering Team
- Changes in third-party email SDK error schemas could lead to undefined error output.
  Mitigation: Validate error object properties and fallback to generic error string serialization before logging.
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
- Ensure error handlers in email dispatch functions inspect both rejected promises and returned error structures from the email client prior to logging.
- Format logged strings with clear operational prefixes to simplify text filtering in log viewers.

## Continuation Context


Verify commands:
- Discover and run the repository static analysis script from the root workspace manifest to verify logging compliance.
- Discover and execute the package test suite using the project build runner to confirm error handling assertions pass.

Accept when:
- All email transmission error paths are verified by automated tests to emit captured errors to the configured diagnostic sink.
- Static analysis checks pass across the workspace with zero diagnostic warnings or unhandled rejection errors.

## Enforcement

- Verified by: Pull request code reviews verifying error handling and logging conformance in email client modules.
- Verified by: Automated workspace static analysis and continuous integration test suites.
- Violation handling: Code review rejection for pull requests that suppress dispatch errors or introduce unhandled rejection pathways.
- Exception process: Exceptions must be documented in code comments and approved by the engineering lead during architectural review.