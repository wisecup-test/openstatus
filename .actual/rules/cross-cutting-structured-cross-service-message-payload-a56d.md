---
version: 2
name: cross-cutting-structured-cross-service-message-payload-a56d
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Structured Cross Service Message Payloads Monitor"
metadata:
  actual:
    adr_id: a56d9472-cb5e-47c1-b281-359e43cff6e8
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Structured Cross Service Message Payloads Monitor

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: All structured cross-service message payloads and monitor specification models MUST adopt google.golang.org/protobuf for serialization contracts, implementing ProtoMessage and ProtoReflect interfaces.

### Verify

```bash
echo "Discover and execute project verification test suites from repository configuration"
echo "Verify that project dependency lock records contain the resolved protobuf runtime dependency"
```

**Accept when:**
- All message definitions compile cleanly and satisfy ProtoMessage and ProtoReflect interfaces.
- Descriptor initialization executes successfully without concurrent race conditions in test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>