---
version: 2
name: cross-cutting-tool-rendering-registry-provide-explicit-9c41
description: "Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Rendering Registry Provide Explicit Component"
metadata:
  actual:
    adr_id: 9c411260-4100-423a-ae41-b3d5d74e2f36
    status: accepted
    candidate_group_id: 5734c25c-6eab-4e00-80c4-3e1fde6ed343
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/services/agent-tools for Agent Tool Invocation and Rendering Contracts: Tool Rendering Registry Provide Explicit Component

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TOOL-001** MUST: The tool rendering registry MUST provide an explicit component mapping for each supported tool defined in @openstatus/services/agent-tools and MUST log a warning while falling back to raw data output when an unmapped tool name is encountered.

### Verify

```bash
# Discover and run workspace dependency verification command
# Discover and execute the project typecheck script
# Discover and run the test suite covering conversational tool rendering
```

**Accept when:**
- All tool renderer components typecheck without errors against @openstatus/services/agent-tools contracts.
- The tool registry renders supported tools and triggers the expected fallback warning for unmapped tools.
- Workspace lockfile verification confirms all packages resolve to exact locked versions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>