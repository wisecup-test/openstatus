---
version: 2
name: f79f282d-6a1d-440f-a4d2-dbd0b83a8e3b-adopt-openstatus-regions-internal-module-for-region-definitions-consumers-discover-ecosystem-lock-artifact-verify
description: "Adopt @openstatus/regions Internal Module for Region Definitions: Consumers Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: f79f282d-6a1d-440f-a4d2-dbd0b83a8e3b
    status: accepted
    candidate_group_id: c5f66e55-2633-46e9-84c4-e8af97ce4818
    evidence:
      source: deep
  scope:
    root: packages/notifications/base
    paths:
      - src/utils/**
---

# Adopt @openstatus/regions Internal Module for Region Definitions: Consumers Discover Ecosystem Lock Artifact Verify

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/notifications/base`

Applies to:
- `src/utils/**`

## Context

- Notification services require consistent geographical and infrastructure region identifiers when formatting incident status alerts and payload messages.
- The codebase defines a dedicated internal workspace module @openstatus/regions to provide authoritative region identifiers and metadata across packages.
- Message formatting utilities import @openstatus/regions directly alongside notification data models to populate localized incident context.

## Problem Statement

Decentralized or ad-hoc definitions of geographical regions across notification services lead to inconsistent naming, fractured display values, and breaking schema variations when alert messages are delivered to external consumers.

## Decision

1. MUST: Consumers MUST discover the ecosystem lock artifact and verify the exact resolved version and module contract for all shared workspace dependencies prior to implementation.

## Policy Block

- MUST Consumers MUST discover the ecosystem lock artifact and verify the exact resolved version and module contract for all shared workspace dependencies prior to implementation.

In scope:
- Message formatting utilities and notification services that construct payload messages containing region metadata

Out of scope:
- Third-party notification transport adapters that pass raw pre-formatted payload strings without inspecting regional data

## Rationale

- Standardizing on @openstatus/regions ensures that notification messages display uniform region names across all delivery channels.
- Centralizing region metadata in a dedicated internal package isolates geographic configuration changes to a single workspace boundary.
- Direct consumption of shared module types eliminates payload serialization errors caused by mismatched region constants.

## Consequences

Positive:
- Guarantees consistent region labeling across all generated incident notification messages.
- Centralizes maintenance of region catalogs, display names, and metadata in a single internal module.
- Enables compile-time type checking for regional attributes in notification payloads.

Negative:
- Introduces an internal dependency between notification utilities and the workspace region module.
- Requires workspace-level updates whenever regional attributes or metadata schemas evolve.

## Alternatives

- Local hardcoded region string constants within each notification package (rejected)
  Rejected because: Duplicating region names across packages causes naming drift, typos, and inconsistent customer-facing incident alerts.
  When valid: Valid only in standalone prototypes with no shared infrastructure contracts.
- Dynamic retrieval of region metadata via network calls at message formatting time (rejected)
  Rejected because: Introduces latency, network failure modes, and operational complexity into critical alert dispatch paths.
  When valid: Valid when region lists mutate continuously at runtime and cannot be synchronized at build time.

## Risks

- Breaking changes in the exported contract of @openstatus/regions could fail notification message construction.
  Mitigation: Enforce workspace type checking and contract testing across consumer packages prior to publishing updates.
  Owner: Engineering Team
- Unknown region identifiers received from legacy database records causing runtime formatting failures.
  Mitigation: Provide fallback display formatting in notification message utilities when unmapped region codes are encountered.
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
- Import region definitions directly from @openstatus/regions and compose them into notification messages alongside incident timestamps and descriptions.
- Verify that all region lookups adhere to the exported interfaces of the shared module before dispatching formatted messages.

## Continuation Context


Verify commands:
- test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured type checking script"
- test -n "$(find . -maxdepth 2 -type f)" && echo "Discover repository manifest and execute configured test script"

Accept when:
- The discovered project verification suite completes with zero exit code across all notification modules.
- All references to region identifiers and metadata resolve strictly to @openstatus/regions without local duplication.

## Enforcement

- Verified by: Automated static analysis and type verification executed during pull request checks.
- Verified by: Peer code review confirming no hardcoded regional strings are introduced in message formatting logic.
- Violation handling: Pull requests with local duplicate region constants or unapproved region lookup implementations are blocked from merging.
- Violation handling: Violations discovered in active development must be refactored to import from @openstatus/regions.
- Exception process: Exceptions require documented approval from the architecture team detailing why dynamic or third-party region mapping is strictly necessary.