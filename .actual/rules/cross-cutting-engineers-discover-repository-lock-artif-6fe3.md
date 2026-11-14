---
version: 2
name: cross-cutting-engineers-discover-repository-lock-artif-6fe3
description: "@bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Engineers Discover Repository Lock Artifact Resolve"
metadata:
  actual:
    adr_id: 6fe37eb5-f02f-4531-841f-ae8fbdf38db2
    status: accepted
    candidate_group_id: 2198f7d3-4116-4221-bdc7-c0a86d693c7e
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/openstatus/monitor/v1/**
---

# @bufbuild/protobuf/codegenv2 Data Access Contract Definitions: Engineers Discover Repository Lock Artifact Resolve

Scope: applies to `packages/proto/gen/ts/openstatus/monitor/v1/**`.

These rules are ALWAYS ACTIVE for all codebases generating TypeScript data contracts and deletion schema representations using @bufbuild/protobuf/codegenv2 and components modeling data access deletion requests across boundary definitions.

### Rules

- **R-BUF-001** MUST: Engineers MUST discover the repository lock artifact to resolve the exact dependency version for @bufbuild/protobuf/codegenv2 before referencing generated contract definitions.
- **R-BUF-002** MUST: All data access modules MUST consume deletion types exported from the designated generated package rather than ad-hoc local types.
- **R-BUF-003** MUST: Ensure schema generation workflows are integrated into build pipelines so generated types remain synchronized with canonical definitions.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration type validation pipelines and architecture code reviews enforce these requirements, and pull requests introducing manual overrides or ungenerated contract structures will be blocked.
</enforcement>