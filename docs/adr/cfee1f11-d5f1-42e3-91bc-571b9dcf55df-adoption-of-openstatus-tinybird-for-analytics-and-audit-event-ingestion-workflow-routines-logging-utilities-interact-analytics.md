---
version: 2
name: cfee1f11-d5f1-42e3-91bc-571b9dcf55df-adoption-of-openstatus-tinybird-for-analytics-and-audit-event-ingestion-workflow-routines-logging-utilities-interact-analytics
description: "Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Workflow Routines Logging Utilities Interact Analytics"
metadata:
  actual:
    adr_id: cfee1f11-d5f1-42e3-91bc-571b9dcf55df
    status: accepted
    candidate_group_id: c8e7cec8-7a72-4a93-ab64-16c9cff4fc8f
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/utils/**
---

# Adoption of @openstatus/tinybird for Analytics and Audit Event Ingestion: Workflow Routines Logging Utilities Interact Analytics

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/workflows`

Applies to:
- `src/cron/**`
- `src/utils/**`

## Context

- Background workflow routines execute scheduled maintenance jobs and capture audit events that require persistent analytical tracking.
- Multiple modules within the workflow service independently require capabilities to log audit entries and record periodic metric freezes.
- Direct coupling to remote API endpoints across multiple execution routines causes code duplication and inconsistent error recovery behavior.

## Problem Statement

Dispersed background tasks and audit logging utilities need a reliable, unified mechanism to publish analytical telemetry and event logs without duplicating external transport logic, credential management, and failure recovery handling.

## Decision

1. MUST: Workflow routines and logging utilities MUST interact with the analytics infrastructure exclusively through the @openstatus/tinybird package.

## Policy Block

- MUST Workflow routines and logging utilities MUST interact with the analytics infrastructure exclusively through the @openstatus/tinybird package.

In scope:
- All background workflow tasks, cron jobs, and utility logging modules that transmit analytical telemetry or record audit events.

Out of scope:
- Standalone client-side user interfaces and frontend web applications that do not interface directly with internal analytical pipelines.

## Rationale

- Consolidating analytics calls into @openstatus/tinybird guarantees uniform event schema serialization and authentication across all workflow workers.
- Observed usage shows batch processing with structured chunk failure logging, confirming that client-level abstraction is essential for maintaining task resilience during analytical backend degradation.

## Consequences

Positive:
- Centralizes data ingestion contracts, schema compliance, and analytical queries behind a single maintainable interface.
- Ensures consistent error logging, retry structures, and batch chunk execution handling across cron jobs and audit trails.
- Eliminates duplicated transport and authentication code across background tasks.

Negative:
- Workflows are tightly coupled to the contract and release cycle of the internal analytics package.
- Breaking changes to shared event schemas require coordinated updates across workflows and service consumers.

## Alternatives

- Direct HTTP calls to the external analytics REST endpoints from individual workflow handlers (rejected)
  Rejected because: Introduces duplicate transport logic, fragmented authentication header handling, and inconsistent error-handling mechanisms across background tasks.
  When valid: Valid only in isolated low-footprint proof-of-concept services where importing shared packages is explicitly unsupported.
- Third-party analytics SDK directly embedded in every workflow routine (rejected)
  Rejected because: Bypasses project-specific schema validation, shared retry semantics, and centralized environment credential management.
  When valid: Valid if no internal abstraction layer or shared organizational package exists.

## Risks

- Outages or rate limits in the analytical backend could stall workflow processing if errors are unhandled.
  Mitigation: Implement chunk-level error containment and structured logging so transient remote failures do not interrupt surrounding business operations.
  Owner: engineering team
- Schema changes in the shared analytics client may cascade into runtime errors across multiple workflow routines.
  Mitigation: Enforce integration testing against contract mocks during continuous integration runs to identify breaking schema discrepancies early.
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
- Instantiate or consume the client using environment variables provided through the centralized configuration module.
- Ensure chunked batch operations encapsulate failures in structured try-catch blocks and log warning messages with contextual failure metadata rather than terminating the entire job.

## Continuation Context


Verify commands:
- Discover the project task runner from the dependency manifest and run the architectural linting and dependency verification checks.
- Locate the repository test runner and execute the automated test suites governing workflow execution and audit logging to verify client integration.

Accept when:
- All analytical operations, audit event records, and batch ingestions route strictly through the designated internal analytics module.
- The project verification scripts and test suites complete with zero errors and confirm no unauthorized direct network calls to the external analytics service.

## Enforcement

- Verified by: Automated static analysis checks in the continuous integration pipeline that flag unauthorized direct network calls or alternative client usage.
- Verified by: Peer review on all pull requests touching workflow routines and audit logging implementations.
- Violation handling: Pull requests introducing direct external API calls or unapproved client libraries are blocked from merging.
- Violation handling: Violations discovered in main branches trigger remediation tickets prioritized in the subsequent sprint.
- Exception process: Exceptions require submission of an architectural review request detailing why the shared module cannot fulfill the specific analytical transport requirements.
- Exception process: Approved exceptions must be logged with a bounded timeline to migrate back to the standardized client package.