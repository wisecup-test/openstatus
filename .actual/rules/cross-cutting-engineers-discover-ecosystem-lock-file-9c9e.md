---
version: 2
name: cross-cutting-engineers-discover-ecosystem-lock-file-9c9e
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: Engineers Discover Ecosystem Lock File Repository"
metadata:
  actual:
    adr_id: 9c9ed80a-6598-46b1-8ea2-3b44bb8bd2d2
    status: accepted
    candidate_group_id: 1a9fc28f-a52f-4b72-9081-afceafbc050c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
      - pkg/job/**
---

# Resilience Retry Orchestration via github.com/cenkalti/backoff: Engineers Discover Ecosystem Lock File Repository

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all outbound network operations, probe handlers, and automated diagnostic jobs dispatching requests across external network boundaries.

### Rules

- **R-RES-001** MUST: Engineers MUST discover the ecosystem lock file in the repository to resolve the authoritative version of github.com/cenkalti/backoff and verify API compatibility before invoking retry facilities.

### Verify

```bash
# Discover and execute project dependency audit script for third-party resilience libraries
# Discover and run project test suite for exponential backoff bounds and context cancellations
# Execute static analysis rules for unhandled direct network calls
```

**Accept when:**
- All outbound network probe handlers and background job runners execute external calls through bounded github.com/cenkalti/backoff retry structures.
- Project verification test suites confirm that transient network faults trigger exponential backoff retries and terminate upon reaching maximum retry limits or context deadlines.
- Static analysis checks pass across all modules without flagging unhandled or unbounded external network calls.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, code linters, and peer code reviews.
</enforcement>