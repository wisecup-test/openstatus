---
version: 2
name: cross-cutting-keys-written-distributed-cache-layer-0a75
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Keys Written Distributed Cache Layer Define"
metadata:
  actual:
    adr_id: 0a75ca61-15d1-415e-b33b-cb60135fab54
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

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Keys Written Distributed Cache Layer Define

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-UPSTASH-001** MUST: Keys written to the distributed cache layer MUST define explicit time-to-live expiration intervals to prevent unbounded storage growth and stale state leakage.
- **R-UPSTASH-002** MANDATORY (DISCOVERY POLICY): Omit all tool names, file names, commands, package managers, and version numbers. Derive them from the project repository.
- **R-UPSTASH-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, find the dependency manifest, identify the build tool, inspect the repository lock/resolution artifact for the exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.
- **R-UPSTASH-004** MUST: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-UPSTASH-005** MUST: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

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
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated. Non-compliant cache operations or unhandled cache exceptions flagged during review must be refactored prior to merge.
</enforcement>