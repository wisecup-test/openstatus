---
version: 2
name: cross-cutting-http-client-network-calls-encapsulated-4114
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Http Client Network Calls Encapsulated Retry"
metadata:
  actual:
    adr_id: 411458e6-8008-4068-9c52-d95d8e315c71
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Http Client Network Calls Encapsulated Retry

Scope: applies to `apps/checker/handlers/**`.

These rules are ALWAYS ACTIVE for all network client request handlers executing outbound diagnostic checks, regional ping operations, and network operations subject to transient connection resets, socket timeouts, or temporary remote endpoint unavailability.

### Rules

- **R-HTTP-001** MUST: HTTP client network calls encapsulated in a retry operation MUST enforce an explicit timeout on the client instance.

### Verify

```bash
# Discover and execute the project test runner for handler packages to validate exponential backoff behavior under simulated network faults.
# Discover and run the project static analysis and linting targets to ensure no unmanaged retry loops exist in handler implementations.
```

**Accept when:**
- All outbound HTTP client network calls that execute within request handlers coordinate transient failure recovery through the adopted backoff library.
- Automated verification scripts confirm zero unhandled transient network errors and verify that retry operations enforce configured maximum retry counts.
- Static analysis verifies that lock resolution artifacts match the active environment before building handler packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. All outbound network requests must be validated against the retry and timeout constraints.
</enforcement>