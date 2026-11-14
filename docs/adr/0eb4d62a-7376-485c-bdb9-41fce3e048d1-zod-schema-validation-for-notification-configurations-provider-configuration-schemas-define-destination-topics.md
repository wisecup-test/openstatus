---
version: 2
name: 0eb4d62a-7376-485c-bdb9-41fce3e048d1-zod-schema-validation-for-notification-configurations-provider-configuration-schemas-define-destination-topics
description: "Zod Schema Validation for Notification Configurations: Provider Configuration Schemas Define Destination Topics"
metadata:
  actual:
    adr_id: 0eb4d62a-7376-485c-bdb9-41fce3e048d1
    status: accepted
    candidate_group_id: 53081f80-63f1-4a28-a21c-a5dc14dbb968
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Zod Schema Validation for Notification Configurations: Provider Configuration Schemas Define Destination Topics

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/ntfy`

Applies to:
- `src/**`

## Context

- Notification integrations require external configuration values that must be validated prior to establishing provider connections.
- External configurations often contain optional authentication credentials alongside standard default service endpoints.
- Declarative schema validation ensures structural integrity and runtime type safety at integration boundaries.

## Problem Statement

External configuration inputs for notification integrations can be incomplete, malformed, or missing default connection parameters, leading to runtime failures during notification dispatch. An architectural mechanism is required to guarantee structural validity and default parameter assignment at integration boundaries.

## Decision

1. MUST: Provider configuration schemas MUST define destination topics and server connection URLs as string schemas with standard default fallbacks where provider conventions exist.

## Policy Block

- MUST Provider configuration schemas MUST define destination topics and server connection URLs as string schemas with standard default fallbacks where provider conventions exist.

In scope:
- All notification provider configuration modules accepting external configuration or environment inputs.
- Boundary validation layers connecting notification services to external network endpoints.

Out of scope:
- Internal notification message dispatch pipelines that operate on already validated internal structures.
- Utility modules that do not ingest external configuration parameters.

Exceptions:
- EXC-20-001: The integration provider operates purely on in-memory static fixtures without external configuration input.

## Rationale

- The codebase explicitly defines notification configuration structures using Zod object schemas to validate integration parameters prior to runtime dispatch.
- Zod declarative schemas combine structural validation, default endpoint assignment, and optional credential handling into a single verifiable definition.
- Using declarative schemas prevents unvalidated inputs from causing silent failures across integration boundaries.

## Consequences

Positive:
- Provides declarative runtime input validation with automatic type inference.
- Guarantees predictable defaults for service endpoints while supporting optional authentication credentials.
- Prevents invalid or malformed configuration payloads from reaching notification dispatch channels.

Negative:
- Introduces an external dependency and runtime parsing overhead on service startup.
- Requires maintaining schema definitions in alignment with external notification service API specifications.

## Alternatives

- Manual runtime type guard assertions without schema validation libraries (rejected)
  Rejected because: Manual conditional type guards introduce boilerplate, drift from compile-time type definitions, and lack declarative default fallback handling.
  When valid: Valid only in zero-dependency utility packages where external libraries are prohibited.
- Compile-time type interfaces without runtime schema parsing (rejected)
  Rejected because: Pure TypeScript interfaces are erased during compilation and cannot validate external or dynamically loaded configuration inputs at runtime.
  When valid: Valid for internal memory structures that never accept external or deserialized input.

## Risks

- Breaking API changes between library versions could alter schema builder method signatures.
  Mitigation: Apply the lock-version grounding process to ensure imported schema construction functions match the resolved library version documentation.
  Owner: Engineering Team
- Misconfigured default fallback values could unintentionally target public endpoints.
  Mitigation: Write unit tests validating that missing optional fields and fallback defaults resolve correctly during schema parsing.
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
- Define notification provider configuration schemas in dedicated schema modules within each provider package.
- Use inferred schema output types to type provider client initialization parameters rather than manually duplicated interface definitions.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking script across integration modules to verify schema typings.
- Discover and run the project test suite covering notification configuration parsing and validation.

Accept when:
- All notification configuration schemas pass static type verification and schema validation tests without errors.
- Integration configuration parsing succeeds with default endpoint fallbacks when optional credentials are omitted.

## Enforcement

- Verified by: Automated CI build pipelines executing repository test suites and type verification scripts.
- Verified by: Peer code review inspecting new notification module configuration schemas.
- Violation handling: Pull requests with unvalidated configuration schemas or raw type casts will be blocked during automated verification.
- Violation handling: Violations identified during code review must be refactored to use declarative schema definitions before merge.
- Exception process: Submit an architectural exception request detailing why the integration module cannot adopt the declarative validation library.
- Exception process: Obtain sign-off from the integration architecture team with documented mitigation for runtime validation.