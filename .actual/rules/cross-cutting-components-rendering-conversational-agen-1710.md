---
version: 2
name: cross-cutting-components-rendering-conversational-agen-1710
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Components Rendering Conversational Agent Tool Outputs"
metadata:
  actual:
    adr_id: 1710153c-01f2-4acc-a3fd-4c35918eabef
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Components Rendering Conversational Agent Tool Outputs

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AGT-001** MUST: Components rendering conversational agent tool outputs MUST consume tool definitions and payload schemas directly from the @openstatus/services/agent-tools module.

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