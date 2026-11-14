---
version: 2
name: cross-cutting-message-implementations-utilize-google-g-11af
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Implementations Utilize Google Golang Org"
metadata:
  actual:
    adr_id: 11af0f39-5c34-4427-b544-628d8e81e01a
    status: accepted
    candidate_group_id: a0de90ee-f110-4e44-a829-5105eaf11733
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Schema Contracts: Message Implementations Utilize Google Golang Org

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: Message implementations MUST utilize google.golang.org/protobuf/runtime/protoimpl and google.golang.org/protobuf/reflect/protoreflect for raw descriptor compression and dynamic type reflection.

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