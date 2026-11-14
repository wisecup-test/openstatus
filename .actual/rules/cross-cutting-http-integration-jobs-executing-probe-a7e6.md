---
version: 2
name: cross-cutting-http-integration-jobs-executing-probe-a7e6
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Http Integration Jobs Executing Probe Routines"
metadata:
  actual:
    adr_id: a7e64a2e-2c1c-4fc6-97b4-32c3b7ba3199
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

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Http Integration Jobs Executing Probe Routines

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CHCK-001** SHOULD: HTTP integration jobs executing probe routines SHOULD record comprehensive timing metrics through Timing structures to enable latency percentile tracking alongside assertion outcomes.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts
# Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>