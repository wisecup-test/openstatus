---
version: 2
name: cross-cutting-internal-communication-endpoints-not-use-63be
description: "Internal API Contract Standardization with google.golang.org/protobuf: Internal Communication Endpoints Not Use Unstructured"
metadata:
  actual:
    adr_id: 63beaf3f-a777-4b7b-af52-a96f36f96b08
    status: accepted
    candidate_group_id: 523c2937-cde9-4774-8810-baf86298bb1a
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
      - proto/private_location/v1/**
---

# Internal API Contract Standardization with google.golang.org/protobuf: Internal Communication Endpoints Not Use Unstructured

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROT-001** MUST_NOT: Internal API communication endpoints MUST NOT use unstructured or ad-hoc payload structures that bypass compiled schema verification.

### Verify

```bash
# Discover and execute the project contract linting and verification scripts to confirm internal schema compliance.
# Discover and execute the project test suite covering internal API serialization and message boundary validation.
```

**Accept when:**
- All internal service communication schemas implement the ProtoMessage and ProtoReflect interfaces without manual field mutation.
- Automated contract verification confirms zero schema drift and successful binary serialization across all internal API endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by static analysis checks, schema linters, automated boundary test suites, and architecture peer reviews.
</enforcement>