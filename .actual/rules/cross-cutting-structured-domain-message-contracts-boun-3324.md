---
version: 2
name: cross-cutting-structured-domain-message-contracts-boun-3324
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Structured Domain Message Contracts Boundary Communication"
metadata:
  actual:
    adr_id: 332409e9-1a1e-4cc8-b527-813e07c3c643
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Structured Domain Message Contracts Boundary Communication

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROTO-001** MUST: All structured domain message contracts and boundary communication schemas MUST be implemented using the google.golang.org/protobuf runtime and reflection modules.

### Verify

```bash
# Discover and execute the repository contract validation test suite to confirm all generated boundary structures satisfy ProtoMessage and ProtoReflect interfaces.
# Execute the workspace static analysis checks to verify that descriptor initialization conforms to synchronized single-execution patterns without unauthorized memory manipulation.
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>