---
version: 2
name: cross-cutting-consumer-discover-project-dependency-loc-61bf
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Consumer Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 61bf9f3d-99d5-4b05-92f6-2fb9fe1b5271
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Consumer Discover Project Dependency Lock Artifact

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: The consumer MUST discover the project dependency lock artifact and verify the exact resolved version of google.golang.org/protobuf before introducing or modifying schema implementations.

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