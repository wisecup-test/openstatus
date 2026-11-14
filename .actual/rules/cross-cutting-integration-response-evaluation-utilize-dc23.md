---
version: 2
name: cross-cutting-integration-response-evaluation-utilize-dc23
description: "checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Integration Response Evaluation Utilize Evaluatehttpassertions Protonumberassertiontocomparator"
metadata:
  actual:
    adr_id: dc235bee-e26c-4679-b42a-2e9f973dfb1a
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

# checker Internal Module Adoption for HTTP Synthetic Integration Probing and Assertion Evaluation: Integration Response Evaluation Utilize Evaluatehttpassertions Protonumberassertiontocomparator

Scope: applies to `apps/checker/checker/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CHECKER-001** MUST: Integration response evaluation MUST utilize EvaluateHTTPAssertions with ProtoNumberAssertionToComparator and ProtoStringAssertionToComparator to evaluate response headers, status codes, and response payload bodies against configured thresholds.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts.
# Run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints.
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test pipelines and peer code reviews verify assertion evaluation and probe execution contracts.
</enforcement>