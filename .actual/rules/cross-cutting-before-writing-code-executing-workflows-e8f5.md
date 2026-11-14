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

These rules are ALWAYS ACTIVE for all web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-UPSTASH-001** MUST: Before writing code or executing workflows using the versioned dependency, the consumer MUST inspect the repository dependency manifest and lock or resolution artifact to determine and adhere to the exact resolved package version.
- **R-UPSTASH-002** MUST: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-UPSTASH-003** MUST: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# Discover and execute project verification and linting scripts
# 1. Discover the project verification script defined in the repository manifest and execute it.
# 2. Discover and run the project static analysis and linting script.
if [ -f package.json ]; then
  npm test || pnpm test || yarn test
  npm run lint || pnpm lint || yarn lint
fi
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. All distributed cache and rate limiting operations must route exclusively through the approved module, and pull requests introducing unapproved external cache client connections or unbounded cache writes are blocked until remediated.
</enforcement>