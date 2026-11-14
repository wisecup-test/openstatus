---
version: 2
name: cross-cutting-probing-components-not-evaluate-integrat-f8d6
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Probing Components Not Evaluate Integration Assertions"
metadata:
  actual:
    adr_id: f8d6498e-b0ac-4eb3-a0ee-51378e848c11
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

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Probing Components Not Evaluate Integration Assertions

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for components executing synthetic HTTP integration checks, scheduler and worker modules evaluating declarative assertions against live HTTP responses, and handlers processing synthetic monitoring probe requests and regional ping evaluations.

### Rules

- **R-CHECKER-001** MUST_NOT: Probing components MUST_NOT evaluate integration assertions directly inside transport client wrappers or transport listeners without delegating assertion logic to EvaluateHTTPAssertions.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts
# Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines verify assertion evaluation and probe execution contracts, and peer code reviews audit external network call sites for compliance with timeout and backoff policies.
</enforcement>