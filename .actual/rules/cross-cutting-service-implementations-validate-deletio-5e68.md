---
version: 2
name: cross-cutting-service-implementations-validate-deletio-5e68
description: "@bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Service Implementations Validate Deletion Payloads Against"
metadata:
  actual:
    adr_id: 5e68c85b-0a26-4049-9a3c-27e4fa96c68d
    status: accepted
    candidate_group_id: 2198f7d3-4116-4221-bdc7-c0a86d693c7e
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/openstatus/monitor/v1/**
---

# @bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Service Implementations Validate Deletion Payloads Against

Scope: applies to `packages/proto/gen/ts/openstatus/monitor/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DA-001** MAY: Service implementations MAY validate deletion payloads against companion validation schemas prior to executing deletion queries.

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