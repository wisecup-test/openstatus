---
version: 2
name: cross-cutting-public-message-structures-implement-stan-2ea6
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Public Message Structures Implement Standard Contract"
metadata:
  actual:
    adr_id: 2ea67280-ef8c-4f62-8efc-e55752aead59
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Public Message Structures Implement Standard Contract

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: Public message structures MUST implement standard contract methods Reset, String, ProtoMessage, ProtoReflect, and Descriptor to satisfy serialization interfaces.

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