---
version: 2
name: cross-cutting-rate-limiting-checks-evaluated-earliest-1558
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Rate Limiting Checks Evaluated Earliest Boundary"
metadata:
  actual:
    adr_id: 1558fda1-d7f5-450a-a0e4-0b55e186b420
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

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Rate Limiting Checks Evaluated Earliest Boundary

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-RATE-001** SHOULD: Rate limiting checks SHOULD be evaluated at the earliest boundary of incoming request handling prior to executing resource-intensive operations.
- **R-RATE-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the repository.
- **R-RATE-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, re-run per dependency at point of use for version-sensitive behavior.
- **R-RATE-004** MANDATORY: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-RATE-005** MANDATORY: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

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
Claude Code MUST NOT skip or defer verification. Pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated.
</enforcement>