---
version: 2
name: cross-cutting-before-writing-code-executing-workflows-e8f5
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Before Writing Code Executing Workflows Versioned"
metadata:
  actual:
    adr_id: e8f5b91c-4f25-4e9e-8161-bf4670bb21f6
    status: accepted
    candidate_group_id: c89369f1-016e-48d6-a1e7-23a0bcf0bd22
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/feedback/docs/**
      - src/lib/**
---

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Before Writing Code Executing Workflows Versioned

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UPSTASH-001** MUST: Before writing code or executing workflows using the versioned dependency, the consumer MUST inspect the repository dependency manifest and lock or resolution artifact to determine and adhere to the exact resolved package version.
- **R-UPSTASH-002** MUST: Execute the lock-version grounding sequence (discover manifest, identify build tool, inspect lock/resolution artifact, fetch version-specific documentation, confirm APIs exist, and re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-UPSTASH-003** MUST: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-UPSTASH-004** MUST: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# Discover and execute the project verification script defined in the repository manifest for type checking and test suite compliance
# Discover and run the project static analysis and linting script to ensure no unauthorized direct external cache client connections bypass the designated internal module
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code review enforce that distributed cache and rate limiting operations route exclusively through the approved module, and pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated.
</enforcement>