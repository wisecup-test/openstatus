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

These rules are ALWAYS ACTIVE for all synthetic HTTP integration checks, scheduler/worker modules evaluating declarative assertions against live responses, and handlers processing synthetic monitoring probe requests and regional ping evaluations.

### Rules

- **R-CHK-001** MUST: All synthetic HTTP integration probe executions and response validations MUST be coordinated through the checker internal module using the Http probe dispatcher and HTTPJob executor contracts.
- **R-CHK-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-CHK-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find the dependency manifest, identify the build tool, inspect the repository lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that exact version, and re-run as needed.

### Verify

```bash
# Discover and execute the project integration test suite to verify probe dispatch and assertion evaluation contracts
# Discover and run static analysis checks across the codebase to ensure external network invocations comply with timeout and backoff retry constraints
```

**Accept when:**
- All synthetic integration probing modules successfully evaluate HTTP response bodies, status codes, and timing metrics using EvaluateHTTPAssertions without errors.
- Network integration probes handle simulated endpoint failures according to configured exponential backoff limits without unbounded retry loops.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external network call sites must comply with timeout and backoff policies, and non-compliant integration probe logic must be refactored to use the checker module contracts.
</enforcement>