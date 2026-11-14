---
version: 2
name: 93b5fc3c-a2f6-4cb7-9bee-2763b65f439b-internal-date-utility-issameday-for-blacklist-date-access-when-consuming-any-external-versioned-dependency
description: "Internal Date Utility isSameDay for Blacklist Date Access: When Consuming Any External Versioned Dependency"
metadata:
  actual:
    adr_id: 93b5fc3c-a2f6-4cb7-9bee-2763b65f439b
    status: accepted
    candidate_group_id: ad6d1ea8-3894-4c41-895c-e77b7786a974
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Internal Date Utility isSameDay for Blacklist Date Access: When Consuming Any External Versioned Dependency

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/tracker`

Applies to:
- `src/**`

## Context

- The codebase maintains date-indexed collections representing blacklisted dates where membership must be validated against runtime day inputs.
- Stored date keys may vary in string formatting or time components, preventing direct string property access on the underlying data dictionary.
- The module employs an internal utility module providing an isSameDay function to iterate collection keys and evaluate calendar day equality against query parameters.
- Static analysis observed this pattern in a single tracking module, indicating a localized implementation detail rather than an established repository-wide standard.

## Problem Statement

Validating whether an arbitrary calendar day exists within an in-memory collection of date keys requires calendar-aware day comparison rather than strict string or millisecond timestamp equality. The access pattern must balance lookup simplicity against search efficiency when evaluating collection membership.

## Decision

1. MUST: When consuming any external versioned dependency for date manipulation or data access, developers MUST discover the repository lock artifact and verify the exact resolved version prior to implementation.

## Policy Block

- MUST When consuming any external versioned dependency for date manipulation or data access, developers MUST discover the repository lock artifact and verify the exact resolved version prior to implementation.

In scope:
- Date-keyed data structures requiring calendar day matching within data access modules

Out of scope:
- Data structures indexed by non-temporal keys
- High-throughput data access paths where normalized direct key lookup is required

Exceptions:
- EXC-30-001: Performance benchmarks demonstrate key scanning bottleneck and require constant-time normalized date key hashing

## Rationale

- Utilizing the isSameDay comparison utility ensures accurate calendar day matching regardless of string representation or time-of-day discrepancies in stored date keys.
- Evaluating keys through linear iteration avoids the need for aggressive pre-normalization pipelines on small in-memory collections where key counts remain minimal.
- Localizing comparison logic in a dedicated utility function prevents duplication of calendar boundary arithmetic across data access call sites.

## Consequences

Positive:
- Accurate day-level membership resolution without strict string formatting constraints on dictionary keys.
- Centralization of calendar comparison logic within a reusable utility function.
- Minimal implementation complexity for small in-memory date collections.

Negative:
- Linear search complexity scales with collection size rather than achieving constant-time property lookups.
- Repeated date object allocations occur during predicate evaluation over collection keys.

## Alternatives

- Direct string key lookup via normalized ISO date strings (rejected)
  Rejected because: Requires pre-normalizing all date keys and incoming query parameters to identical string formats, which adds formatting overhead and potential timezone parsing mismatches across runtime inputs.
  When valid: When dataset volume is large enough that linear key iteration incurs unacceptable computational latency.
- Linear scan with direct timestamp epoch comparison (rejected)
  Rejected because: Epoch millisecond equality fails when comparing date timestamps that differ in time-of-day components even when they represent the identical calendar day.
  When valid: When matching requires exact point-in-time timestamp equality down to the millisecond.

## Risks

- Linear key iteration scale bottleneck if blacklist date collections grow substantially.
  Mitigation: Profile collection access latency and transition to a bucketed or normalized day index if item counts increase.
  Owner: engineering team
- Timezone drift during date object instantiation causing false negative matches.
  Mitigation: Ensure date parsing and day equality checks use consistent timezone normalization across all comparison calls.
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
- Ensure isSameDay comparisons accurately handle local and coordinated universal time boundaries to avoid unexpected match failures across daylight saving transitions.
- For large collections, benchmark key enumeration overhead against normalized date string indexing before scaling data volume.

## Continuation Context


Verify commands:
- Discover and run the project test harness targeting date access and blacklist verification suites.
- Discover and execute repository linting and static type analysis to verify date utility signatures and access patterns.

Accept when:
- All date access tests pass successfully demonstrating calendar-accurate day matching.
- Static analysis validates type correctness for date objects and comparison parameters.

## Enforcement

- Verified by: Continuous integration automated test suites covering data access routines.
- Verified by: Peer code review for pull requests modifying date-indexed data structures and lookup mechanisms.
- Violation handling: Pull requests implementing direct string matching for date collections without calendar day normalization must be revised to use the standardized comparison utility or an approved normalized indexing scheme.
- Exception process: Submit an architectural proposal detailing key volume, profiling benchmarks, and proposed constant-time indexing structures to the repository maintainers.