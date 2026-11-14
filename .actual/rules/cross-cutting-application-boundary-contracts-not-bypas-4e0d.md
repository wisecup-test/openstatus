---
version: 2
name: cross-cutting-application-boundary-contracts-not-bypas-4e0d
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Application Boundary Contracts Not Bypass Generated"
metadata:
  actual:
    adr_id: 4e0d5142-1061-442a-b8f5-a904cc3cb547
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Application Boundary Contracts Not Bypass Generated

Scope: applies to `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for service boundary message definitions, inter-service contract structures, distributed telemetry, monitoring definitions, and contract serialization layers.

### Rules

- **R-PROT-001** MUST_NOT: Application boundary contracts MUST_NOT bypass generated protoimpl runtime serialization methods in favor of manual byte packing or non-standard reflection.

### Verify

```bash
# Discover and execute the repository contract validation scripts to verify schema integrity and generated model alignment.
# Execute the repository test suite to validate serialization, deserialization, and reflection behavior across all boundary contracts.
```

**Accept when:**
- All contract validation scripts execute without reporting schema discrepancies or outdated contract structures.
- All boundary serialization and reflection test suites pass with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced by automated contract consistency checks in CI pipelines and peer code reviews.
</enforcement>