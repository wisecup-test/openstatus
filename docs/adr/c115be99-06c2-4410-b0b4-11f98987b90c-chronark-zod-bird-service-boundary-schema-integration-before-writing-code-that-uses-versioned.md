---
version: 2
name: c115be99-06c2-4410-b0b4-11f98987b90c-chronark-zod-bird-service-boundary-schema-integration-before-writing-code-that-uses-versioned
description: "@chronark/zod-bird Service Boundary Schema Integration: Before Writing Code That Uses Versioned"
metadata:
  actual:
    adr_id: c115be99-06c2-4410-b0b4-11f98987b90c
    status: accepted
    candidate_group_id: 345a5dc7-c8cf-42dc-aa20-3d73478b08a6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @chronark/zod-bird Service Boundary Schema Integration: Before Writing Code That Uses Versioned

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tinybird`

Applies to:
- `scripts/**`

## Context

- External analytics ingestion pipelines require consistent schema validation across service boundary version increments.
- Direct database records must be resolved and mapped to stable public identifiers prior to transmission to decoupled analytics endpoints.
- Unmapped external identifiers or duplicate payloads risk data corruption during datasource transitions without boundary-level validation.

## Problem Statement

Data pipeline boundaries between relational storage and external analytics ingestion endpoints lack unified schema enforcement, risking unvalidated data ingestion, breaking changes across datasource versions, and duplicate transmission during migration workflows.

## Decision

1. MUST: Before writing code that uses a versioned library, the consumer MUST inspect the repository lock or resolution artifact to determine and verify the exact resolved version.

## Policy Block

- MUST Before writing code that uses a versioned library, the consumer MUST inspect the repository lock or resolution artifact to determine and verify the exact resolved version.

In scope:
- Ingestion pipelines and scripts communicating across service boundaries between internal databases and external analytics datasources.
- Schema migration workflows involving versioned external service endpoints.

Out of scope:
- Internal service-to-service communication governed by shared database schemas within the same runtime process.

Exceptions:
- EXC-23-001: Emergency migration operations require bypassing duplicate count guards via force override flags.

## Rationale

- Validating egress data through @chronark/zod-bird guarantees that third-party analytics endpoints receive type-safe payloads conforming to datasource schema versions.
- Explicit identifier resolution decouples internal persistence schema details from public analytics boundary models.
- Guard checks on existing records mitigate duplicate transmission risks inherent in manual backfills.

## Consequences

Positive:
- Strongly typed service boundaries prevent malformed payloads from polluting external analytics datasources.
- Explicit identifier mapping decouples internal relational schemas from external analytics event contracts.
- Pre-flight duplicate record verification prevents duplicate data publishing during datasource migration operations.

Negative:
- In-memory identifier resolution incurs runtime memory overhead during high-volume backfill operations.
- Maintaining dual models for internal relational schemas and external analytics contracts increases schema synchronization overhead.

## Alternatives

- Direct unvalidated HTTP publishing of raw database rows to external analytics endpoints (rejected)
  Rejected because: Transmitting unvalidated internal rows introduces tight schema coupling and fails to detect missing field mappings or version drift across service boundaries.
  When valid: Temporary prototyping environments without data durability or analytics reporting requirements.
- Database-level trigger synchronization without application-layer service boundaries (rejected)
  Rejected because: Database triggers couple database engine persistence to external network endpoints and lack flexible payload transformation or error handling.
  When valid: Heterogeneous databases operating within a single unified database cluster supporting native replication.

## Risks

- Large migration batches could exceed memory capacity during in-memory identifier mapping.
  Mitigation: Stream records in bounded page sizes and process mapping in discrete chunks.
  Owner: Data Engineering
- External datasource version discrepancies could cause unhandled HTTP rejection during ingestion.
  Mitigation: Enforce strict schema validation and verify endpoint availability prior to executing ingestion runs.
  Owner: Data Engineering

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Derive environment variable configurations for database and analytics endpoints from repository configuration templates.
- Implement paging parameters to restrict external client fetch requests to predictable batch boundaries.

## Continuation Context


Verify commands:
- Discover and run the project type-checking and static analysis verification scripts across service boundary modules.
- Discover and execute test suites validating schema transformations and service boundary client serialization.

Accept when:
- Type checking passes without errors across all service boundary schema definitions.
- Payload validation tests verify that malformed egress records fail prior to network dispatch.
- Endpoint migration logic correctly handles existing record thresholds and halts execution when unexpected records are detected.

## Enforcement

- Verified by: Automated continuous integration checks executing repository test suites and type verification.
- Verified by: Peer code review for all modifications to service boundary schemas and analytics client integrations.
- Violation handling: Pull requests introducing unvalidated external client calls or bypassing schema validation will be blocked until compliant data contracts are implemented.
- Exception process: Exceptions require documented approval from the engineering lead with an issue tracking the migration remediation.