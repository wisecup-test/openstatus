---
version: 2
name: cross-cutting-distributed-key-value-caching-rate-b5bf
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Distributed Key Value Caching Rate Limiting"
metadata:
  actual:
    adr_id: b5bfd9bc-c2fa-4ac0-ae11-b1bed954b7d1
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

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Distributed Key Value Caching Rate Limiting

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-UPSTASH-001** MUST: All distributed key-value caching, rate limiting, and ephemeral persistence operations across web application endpoints MUST utilize the @openstatus/upstash module rather than ad-hoc in-memory stores or unmanaged external database connections.
- **R-UPSTASH-002** MUST: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-UPSTASH-003** MUST: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# 1. Discover the project verification script defined in the repository manifest and execute it
# to verify type checking and test suite compliance for modules consuming the shared cache library.
# 2. Discover and run the project static analysis and linting script to ensure no unauthorized
# direct external cache client connections bypass the designated internal module.
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated.
</enforcement>