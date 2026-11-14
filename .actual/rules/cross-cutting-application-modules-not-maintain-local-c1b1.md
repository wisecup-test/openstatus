---
version: 2
name: cross-cutting-application-modules-not-maintain-local-c1b1
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Application Modules Not Maintain Local Process"
metadata:
  actual:
    adr_id: c1b1764c-dc5a-4675-8e54-e38b6084b5e2
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

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Application Modules Not Maintain Local Process

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-CACHE-001** MUST_NOT: Application modules MUST NOT maintain local, in-process mutable state across request invocations for rate limiting or shared cache verification.
- **R-CACHE-002** MANDATORY: Discover the project verification script defined in the repository manifest and execute it to verify type checking and test suite compliance for modules consuming the shared cache library.
- **R-CACHE-003** MANDATORY: Discover and run the project static analysis and linting script to ensure no unauthorized direct external cache client connections bypass the designated internal module.
- **R-CACHE-004** MANDATORY: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-CACHE-005** MANDATORY: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest
# Discover and run the project static analysis and linting script
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-linked endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test and linting verification scripts, and peer code review verifies that distributed cache and rate limiting operations route exclusively through the approved module.
</enforcement>