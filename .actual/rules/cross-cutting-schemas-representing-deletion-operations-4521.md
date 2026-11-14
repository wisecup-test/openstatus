---
version: 2
name: cross-cutting-schemas-representing-deletion-operations-4521
description: "@bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Schemas Representing Deletion Operations Use Bufbuild"
metadata:
  actual:
    adr_id: 4521cce6-767a-4986-b4ad-78f2363f0208
    status: accepted
    candidate_group_id: 2198f7d3-4116-4221-bdc7-c0a86d693c7e
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/openstatus/monitor/v1/**
---

# @bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Schemas Representing Deletion Operations Use Bufbuild

Scope: applies to `packages/proto/gen/ts/openstatus/monitor/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BUF-001** MUST: Schemas representing deletion operations MUST use @bufbuild/protobuf/codegenv2 to generate typed contract structures.

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