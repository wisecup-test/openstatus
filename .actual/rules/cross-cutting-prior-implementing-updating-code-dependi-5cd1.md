---
version: 2
name: cross-cutting-prior-implementing-updating-code-dependi-5cd1
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Prior Implementing Updating Code Depending Google"
metadata:
  actual:
    adr_id: 5cd10684-f1b2-4433-a9ca-0d0d395fb080
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Prior Implementing Updating Code Depending Google

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROT-001** MUST: Prior to implementing or updating code depending on google.golang.org/protobuf, developers MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked library version in accordance with the lock-version grounding policy.

### Verify

```bash
# Find the repository build and validation script defined in the project configuration and execute it to verify message schema compilation.
# Locate the contract test execution command from the project manifest and run test suites validating ProtoMessage serialization and reflection behaviors.
# Query the repository dependency auditing mechanism to confirm all references to the serialization runtime resolve to the locked dependency version.
```

**Accept when:**
- All message definitions pass validation and schema generation checks without compilation errors.
- Test suites asserting message serialization, deserialization, and contract interface compliance pass completely.
- The resolved library version matches the exact specification recorded in the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>