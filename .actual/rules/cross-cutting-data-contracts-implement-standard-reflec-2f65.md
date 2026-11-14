---
version: 2
name: cross-cutting-data-contracts-implement-standard-reflec-2f65
description: "Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Data Contracts Implement Standard Reflection Interfaces"
metadata:
  actual:
    adr_id: 2f6534c2-48e4-4a45-96f8-c2055f9c49cd
    status: accepted
    candidate_group_id: 009e557d-1a63-4729-89c8-7ee78b28aa15
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf Runtime and Reflection Modules: Data Contracts Implement Standard Reflection Interfaces

Scope: applies to `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for service boundary message definitions, inter-service contract structures, and distributed telemetry, monitoring definitions, and contract serialization layers.

### Rules

- **R-PROTO-001** MUST: Data contracts MUST implement the standard reflection interfaces including ProtoReflect and Descriptor methods to enable runtime message inspection and dynamic traversal.

### Verify

```bash
# Discover and execute the repository contract validation scripts to verify schema integrity and generated model alignment
# Execute the repository test suite to validate serialization, deserialization, and reflection behavior across all boundary contracts
```

**Accept when:**
- All contract validation scripts execute without reporting schema discrepancies or outdated contract structures.
- All boundary serialization and reflection test suites pass with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated contract consistency checks in continuous integration pipelines and peer code reviews verify adherence to ProtoMessage and ProtoReflect interface requirements.
</enforcement>