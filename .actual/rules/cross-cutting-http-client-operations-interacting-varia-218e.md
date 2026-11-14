---
version: 2
name: cross-cutting-http-client-operations-interacting-varia-218e
description: "Bounded net/http Client Configuration for Internal API Boundaries: Http Client Operations Interacting Variable Latency"
metadata:
  actual:
    adr_id: 218ee986-c45f-4c0e-a532-85e369048ba0
    status: accepted
    candidate_group_id: 38c879f3-bd47-4707-8de8-53d6496737aa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - handlers/**
      - pkg/job/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Bounded net/http Client Configuration for Internal API Boundaries: Http Client Operations Interacting Variable Latency

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP client integrations communicating with internal API endpoints, monitoring targets, telemetry ingest services, and platform endpoints, including background jobs and request handlers initiating outbound network calls.

### Rules

- **R-HTTP-001** SHOULD: HTTP client operations interacting with variable-latency endpoints SHOULD wrap client execution calls in exponential backoff retry mechanisms with maximum retry bounds.
- **R-HTTP-002** MUST: Construct net/http Client instances with explicit Timeout durations calibrated to the specific operational boundary before issuing Do requests.
- **R-HTTP-003** MUST: Ensure every request object attaches context awareness with deadline propagation and that all response bodies are consistently closed after consumption.

### Verify

```bash
# Discover and run project test suite covering client boundaries
go test ./...
# Discover and run static analysis and linting suite
# (Project-specific lint tool, e.g., golangci-lint run)
```

**Accept when:**
- All outbound HTTP client instantiations declare explicit timeout properties and pass automated tests.
- No instances of unconfigured package-level default HTTP client calls exist across integration packages.
- Verification suites confirm bounded execution and context propagation across client boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>