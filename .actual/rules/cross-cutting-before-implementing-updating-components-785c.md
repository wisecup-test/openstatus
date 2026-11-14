---
version: 2
name: cross-cutting-before-implementing-updating-components-785c
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Before Implementing Updating Components That Consume"
metadata:
  actual:
    adr_id: 785c99d6-2f09-4590-a86f-6124f15f78e5
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Before Implementing Updating Components That Consume

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AST-001** MUST: Before implementing or updating components that consume @openstatus/services/agent-tools, developers MUST inspect the workspace dependency manifest and repository lock artifact to determine the exact resolved package version.
- **R-AST-002** MUST: Register each new tool renderer component in the central tool registry mapping corresponding to its contract in @openstatus/services/agent-tools.
- **R-AST-003** MUST: Ensure fallback logging captures unmapped tool names in development and diagnostic environments to accelerate renderer implementation.

### Verify

```bash
# Discover and run the workspace dependency verification command to ensure package resolution matches repository lock specifications.
# Discover and execute the project typecheck script to validate that all tool renderers conform to @openstatus/services/agent-tools contracts.
# Discover and run the test suite covering conversational tool rendering to confirm fallback handling for unmapped tool identifiers.
```

**Accept when:**
- All tool renderer components typecheck without errors against @openstatus/services/agent-tools contracts.
- The tool registry renders supported tools and triggers the expected fallback warning for unmapped tools.
- Workspace lockfile verification confirms all packages resolve to exact locked versions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>