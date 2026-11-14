---
version: 2
name: cross-cutting-error-handling-around-cache-write-4958
description: "Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Error Handling Around Cache Write Retrieval"
metadata:
  actual:
    adr_id: 4958f4df-a9e5-48fe-bf2c-b0c04e0dfc5e
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

# Adoption of @openstatus/upstash for Distributed Caching and Rate Limiting: Error Handling Around Cache Write Retrieval

Scope: applies to `apps/web/src/app/api/feedback/docs/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for web application API routes, background verification services, and shared utility modules requiring distributed caching, request throttling, or ephemeral key-value persistence.

### Rules

- **R-CACHE-001** SHOULD: Error handling around cache write and retrieval operations SHOULD ensure non-critical failures in telemetry or feedback recording do not degrade primary user request workflows.
- **R-CACHE-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-CACHE-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, and confirm every API/class/function exists in that version's documentation.
- **R-CACHE-004** MANDATORY: Construct cache keys using domain-scoped prefixes and unique identifiers to prevent namespace collisions across distinct feature domains.
- **R-CACHE-005** MANDATORY: Specify explicit expiration durations when invoking write operations to ensure automatic cleanup of expired records.

### Verify

```bash
# Discover and run the project verification script defined in the repository manifest
# Discover and run the project static analysis and linting script
```

**Accept when:**
- Verification suites pass without errors across all modules importing the shared key-value cache and rate limiting library.
- All distributed cache write operations specify an explicit expiration interval.
- All rate-limited endpoints validate request throttle limits through the designated module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>