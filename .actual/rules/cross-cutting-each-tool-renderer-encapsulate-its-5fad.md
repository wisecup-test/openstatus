---
version: 2
name: cross-cutting-each-tool-renderer-encapsulate-its-5fad
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Each Tool Renderer Encapsulate Its Presentation"
metadata:
  actual:
    adr_id: 5fade048-77d7-4c19-9677-6ec7f3dc40a7
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Each Tool Renderer Encapsulate Its Presentation

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TOOL-001** MUST: Each tool renderer MUST encapsulate its presentation logic in a dedicated component and MUST NOT couple its internal layout to sibling tool renderers.

### Verify

```bash
# Discover and run workspace dependency verification command to ensure package resolution matches repository lock specifications
# Discover and execute project typecheck script to validate tool renderers conform to @openstatus/services/agent-tools contracts
# Discover and run test suite covering conversational tool rendering to confirm fallback handling for unmapped tool identifiers
```

**Accept when:**
- All tool renderer components typecheck without errors against @openstatus/services/agent-tools contracts.
- The tool registry renders supported tools and triggers the expected fallback warning for unmapped tools.
- Workspace lockfile verification confirms all packages resolve to exact locked versions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>