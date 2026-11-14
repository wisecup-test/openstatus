---
version: 2
name: cross-cutting-before-implementing-updating-any-version-397d
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Before Implementing Updating Any Versioned Dependency"
metadata:
  actual:
    adr_id: 397d9eb9-28fc-4310-ba99-f3784f103f40
    status: accepted
    candidate_group_id: ca342d26-cc8f-4205-9a21-32db317001d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - handlers/**
      - pkg/job/**
      - pkg/scheduler/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Before Implementing Updating Any Versioned Dependency

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all components executing synthetic HTTP integration checks against external network endpoints, scheduler and worker modules evaluating declarative assertions against live HTTP responses, and handlers processing synthetic monitoring probe requests and regional ping evaluations.

### Rules

- **R-CHK-001** MUST: Before implementing or updating any versioned dependency involved in synthetic integration testing, developers MUST discover the dependency manifest and authoritative resolution lock artifact to verify the exact resolved version and validate API availability in official documentation.
- **R-CHK-002** MUST: Execute dependency lock-version grounding in order: find manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-CHK-003** MUST: Ensure response payload bodies read during assertion validation are closed and bounded to avoid connection leakage and unbounded memory allocation.

### Verify

```bash
# Discover and execute the project integration test suite to verify that probe dispatch and assertion evaluation contracts pass under simulated network conditions.
# Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints.
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Merge requests introducing direct unmanaged HTTP calls without timeout or assertion coordination will be blocked, and non-compliant integration probe logic must be refactored to use the checker module contracts.
</enforcement>