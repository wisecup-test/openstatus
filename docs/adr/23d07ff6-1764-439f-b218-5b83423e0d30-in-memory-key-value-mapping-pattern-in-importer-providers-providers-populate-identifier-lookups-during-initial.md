---
version: 2
name: 23d07ff6-1764-439f-b218-5b83423e0d30-in-memory-key-value-mapping-pattern-in-importer-providers-providers-populate-identifier-lookups-during-initial
description: "In-Memory Key-Value Mapping Pattern in Importer Providers: Providers Populate Identifier Lookups During Initial"
metadata:
  actual:
    adr_id: 23d07ff6-1764-439f-b218-5b83423e0d30
    status: accepted
    candidate_group_id: b7728fd2-3ad8-4ede-ba50-a11f197b9682
    evidence:
      source: deep
  scope:
    root: packages/importers
    paths:
      - src/providers/**
---

# In-Memory Key-Value Mapping Pattern in Importer Providers: Providers Populate Identifier Lookups During Initial

Status: proposed
Date: 2025-05-15
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/importers`

Applies to:
- `src/providers/**`

## Context

- Data ingestion providers ingest external entity hierarchies requiring resolution between foreign source identifiers and internal entity references.
- Performing network roundtrips or secondary datastore queries for each relational lookup introduces latency and dependency coupling during transformation workflows.
- Provider implementations maintain in-memory key-value mapping structures to facilitate constant-time lookup during mapper execution.

## Problem Statement

External data ingestion routines must resolve foreign entity relationships into coherent internal schemas without incurring repetitive datastore or network overhead during mapping execution.

## Decision

1. SHOULD: Providers SHOULD populate identifier lookups during initial collection before invoking normalization mappers to prevent redundant iterations.

## Policy Block

- SHOULD Providers SHOULD populate identifier lookups during initial collection before invoking normalization mappers to prevent redundant iterations.

In scope:
- Entity mapping routines within data ingestion providers requiring identifier resolution across related data structures.

Out of scope:
- Persistent state storage, cross-process data caching, or session management requiring shared datastores.

## Rationale

- In-memory key-value structures provide constant-time lookup for related identifiers during ingestion processing.
- Isolating identifier mappings within the provider lifecycle avoids infrastructure dependencies on dedicated primary datastores or external cache servers for transient ingestion workloads.

## Consequences

Positive:
- Eliminates external datastore roundtrip latency during entity ingestion and normalization mapping.
- Keeps mapper functions decoupled from persistent storage infrastructure.

Negative:
- Memory consumption scales with the count of unique entities processed in a single ingestion execution batch.
- Transient mappings are not shared across distributed workers or persistent processes.

## Alternatives

- External distributed cache or datastore for identifier mapping (rejected)
  Rejected because: Introduces external infrastructure complexity and network latency for transient lookups that exist strictly for the duration of a single import execution.
  When valid: When entity mappings must be shared across distributed worker processes or retained across multiple independent ingestion sessions.
- Repeated linear scans over raw entity collections (rejected)
  Rejected because: Produces quadratic time complexity during mapping of large entity collections.
  When valid: When processing collections with negligible element counts where lookup map construction overhead exceeds scan cost.

## Risks

- Excessive memory allocation if import batches contain unbounded entity collections.
  Mitigation: Enforce batching and pagination constraints upstream of mapping routines.
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
- Ensure that lookup structures are instantiated per import execution and dereferenced upon mapper completion to enable garbage collection.
- Normalize identifier keys to consistent string representations prior to insertion and retrieval to prevent type-mismatch misses.

## Continuation Context


Verify commands:
- Discover and execute the project unit and integration test suites that validate provider mapping routines and identifier resolution behavior.
- Discover and run the static analysis and linting scripts to verify absence of undeclared datastore dependencies within provider modules.

Accept when:
- All provider mapping test suites pass with complete resolution of related entity identifiers.
- Static analysis confirms no persistent datastore connections or unauthorized external cache dependencies are imported in provider mapping modules.

## Enforcement

- Verified by: Automated test suites in continuous integration validating mapper transformations.
- Verified by: Code reviews ensuring in-memory lookup scopes remain confined to ingestion lifecycles.
- Violation handling: Pull requests introducing persistent datastore calls or global mutable cache state into mapper routines will be blocked.
- Exception process: Architectural review and proposal submission required if cross-run persistence or distributed caching is proven necessary for large-scale imports.