---
version: 2
name: cross-cutting-integration-probes-interacting-external-6b30
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Integration Probes Interacting External Network Endpoints"
metadata:
  actual:
    adr_id: 6b305cde-d194-4fea-8340-211889b3075e
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

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Integration Probes Interacting External Network Endpoints

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CHECK-001** MUST: Integration probes interacting with external network endpoints MUST configure explicit client timeout thresholds and MUST enforce exponential retry backoff policies using backoff.Retry with backoff.NewExponentialBackOff.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts
# Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory for all synthetic integration probing and assertion evaluation implementations.
</enforcement>