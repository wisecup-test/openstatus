---
version: 2
name: cross-cutting-before-introducing-modifying-message-def-a00e
description: "Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Before Introducing Modifying Message Definitions Consumers"
metadata:
  actual:
    adr_id: a00ea2c0-0c4b-4317-9b0d-f5ace35ef2ef
    status: accepted
    candidate_group_id: f13253cc-8873-43d0-88ed-e6192e7e6c56
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Message Contracts and Reflection: Before Introducing Modifying Message Definitions Consumers

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all structured domain models, client communication contracts, and monitoring specifications requiring serialization or schema reflection.

### Rules

- **R-PROT-001** MUST: Before introducing or modifying message definitions, consumers MUST inspect the repository dependency resolution artifact to resolve the exact version of the google.golang.org/protobuf library and verify API compatibility against that exact release.

### Verify

```bash
# Discover and execute the repository contract validation test suite
# Execute workspace static analysis checks for descriptor initialization
```

**Accept when:**
- All boundary message contracts satisfy ProtoMessage, ProtoReflect, and Descriptor interface contracts without runtime reflection faults.
- Test suites across all schema-dependent components pass with zero concurrent descriptor initialization panics.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>