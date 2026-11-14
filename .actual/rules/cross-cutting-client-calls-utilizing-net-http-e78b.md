---
version: 2
name: cross-cutting-client-calls-utilizing-net-http-e78b
description: "Bounded net/http Client Configuration for Internal API Boundaries: Client Calls Utilizing Net Http Instances"
metadata:
  actual:
    adr_id: e78b12e8-0598-4346-aaf2-193ba13d8880
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

# Bounded net/http Client Configuration for Internal API Boundaries: Client Calls Utilizing Net Http Instances

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP client integrations communicating with internal API endpoints, monitoring targets, telemetry ingest services, platform endpoints, and background jobs or request handlers initiating outbound network calls.

### Rules

- **R-NET-001** MUST: All client calls utilizing net/http Client instances MUST propagate a context object with cancellation and deadline controls to the request lifecycle.
- **R-NET-002** MUST: Construct net/http Client instances with explicit Timeout durations calibrated to the specific operational boundary before issuing Do requests.
- **R-NET-003** MUST: Ensure every request object attaches context awareness with deadline propagation and that all response bodies are consistently closed after consumption.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test suite covering client boundaries.
# Discover and run the project static analysis and linting suite to check for unconfigured HTTP client usages.
```

**Accept when:**
- All outbound HTTP client instantiations declare explicit timeout properties and pass automated tests.
- No instances of unconfigured package-level default HTTP client calls exist across integration packages.
- Verification suites confirm bounded execution and context propagation across client boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>