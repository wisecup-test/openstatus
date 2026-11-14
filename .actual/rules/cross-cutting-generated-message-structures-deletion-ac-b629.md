---
version: 2
name: cross-cutting-generated-message-structures-deletion-ac-b629
description: "@bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Generated Message Structures Deletion Access Patterns"
metadata:
  actual:
    adr_id: b6299aa9-28b7-4394-b54b-e0d7ab58773a
    status: accepted
    candidate_group_id: 2198f7d3-4116-4221-bdc7-c0a86d693c7e
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/openstatus/monitor/v1/**
---

# @bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Generated Message Structures Deletion Access Patterns

Scope: applies to `packages/proto/gen/ts/openstatus/monitor/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BUF-001** MUST_NOT: Generated message structures for deletion access patterns MUST NOT be manually modified or altered outside code generation workflows.

### Verify

```bash
echo "Discover and execute project test suite for schema compliance"
echo "Discover and execute project type validation suite"
echo "Discover and execute project contract linting suite"
```

**Accept when:**
- All deletion operations consuming protocol contracts compile against @bufbuild/protobuf/codegenv2 generated definitions.
- The project dependency lock artifact is inspected and verified to contain the resolved runtime dependency.
- Type validation checks pass across all modules handling deletion access patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>