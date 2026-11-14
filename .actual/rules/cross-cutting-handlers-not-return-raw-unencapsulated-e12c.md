---
version: 2
name: cross-cutting-handlers-not-return-raw-unencapsulated-e12c
description: "Adoption of Connect RPC for Service API Implementation: Handlers Not Return Raw Unencapsulated Errors"
metadata:
  actual:
    adr_id: e12c1c29-e822-437d-b788-74ffedbfc71e
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Handlers Not Return Raw Unencapsulated Errors

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server-side RPC handler functions processing monitoring queries and telemetry ingestion across service boundaries, and internal communication protocols between private location runners and core ingestion infrastructure.

### Rules

- **R-CONN-001** MUST_NOT: Handlers MUST_NOT return raw unencapsulated errors or unmapped domain exceptions across the RPC boundary.

### Verify

```bash
# Discover and run the project verification script from the repository configuration and execute the static analysis suite across all service handler packages.
# Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints.
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration pipelines and architectural review.
</enforcement>