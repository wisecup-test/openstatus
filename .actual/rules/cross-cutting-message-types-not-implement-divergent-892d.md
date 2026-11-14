---
version: 2
name: cross-cutting-message-types-not-implement-divergent-892d
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Types Not Implement Divergent Custom"
metadata:
  actual:
    adr_id: 892de3b2-3a45-4c83-9670-75a8e2202356
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Types Not Implement Divergent Custom

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST_NOT: Message types MUST_NOT implement divergent custom binary serialization logic that bypasses the adopted protobuf runtime infrastructure.

### Verify

```bash
# Discover and execute project verification test suites from repository configuration
echo "Discover and execute project verification test suites from repository configuration"
# Verify that project dependency lock records contain the resolved protobuf runtime dependency
echo "Verify that project dependency lock records contain the resolved protobuf runtime dependency"
```

**Accept when:**
- All message definitions compile cleanly and satisfy ProtoMessage and ProtoReflect interfaces.
- Descriptor initialization executes successfully without concurrent race conditions in test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>