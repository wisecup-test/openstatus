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

- **R-UPSTASH-001** SHOULD: Rate limiting checks SHOULD be evaluated at the earliest boundary of incoming request handling prior to executing resource-intensive operations.
- **R-UPSTASH-002** MANDATORY: Discover the project verification script defined in the repository manifest and execute it to verify type checking and test suite compliance for modules consuming the shared cache library.
- **R-UPSTASH-003** MANDATORY: Discover and run the project static analysis and linting script to ensure no unauthorized direct external cache client connections bypass the designated internal module.
- **R-UPSTASH-004** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-UPSTASH-005** MANDATORY: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-UPSTASH-006** MANDATORY: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# Discover and execute project test suite and verification scripts
if [ -f "package.json" ]; then
  npm test || pnpm test || yarn test
  npm run lint || pnpm lint || yarn lint
fi
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code reviews, and static analysis verification scripts enforce that distributed cache and rate limiting operations route exclusively through the approved module.
</enforcement>