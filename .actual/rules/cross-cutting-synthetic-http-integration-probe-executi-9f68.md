---
version: 2
name: cross-cutting-synthetic-http-integration-probe-executi-9f68
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Synthetic Http Integration Probe Executions Response"
metadata:
  actual:
    adr_id: 9f68c2d8-ded7-497c-a2e0-36870942700f
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

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Synthetic Http Integration Probe Executions Response

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all synthetic HTTP integration probe executions, assertion evaluations, and related worker components.

### Rules

- **R-CHK-001** MUST: All synthetic HTTP integration probe executions and response validations MUST be coordinated through the checker internal module using the Http probe dispatcher and HTTPJob executor contracts.
- **R-CHK-002** MUST: The consumer MUST discover tool names, file names, commands, package managers, and version numbers directly from the project repository.
- **R-CHK-003** MUST: Follow lock-version grounding before writing code using a versioned library (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact version, confirm APIs exist).
- **R-CHK-004** MUST: Ensure response payload bodies read during assertion validation are closed and bounded to avoid connection leakage and unbounded memory allocation.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts
# Discover and run static analysis checks to ensure external network invocations comply with timeout and backoff constraints
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external network call sites must comply with timeout and backoff policies, and non-compliant integration probe logic must be refactored to use the checker module contracts.
</enforcement>