---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-7f81
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Consumers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 7f818587-7aed-43c3-8c39-041596277214
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Consumers Inspect Repository Dependency Lock Artifact

Scope: applies to `apps/server/src/routes/mcp/**`.

These rules are ALWAYS ACTIVE for all server endpoints and modules that implement Model Context Protocol interfaces or expose agent-facing tools.

### Rules

- **R-MCP-001** MUST: Consumers MUST inspect the repository dependency lock artifact to determine the exact resolved version of @modelcontextprotocol/sdk/server/mcp.js and verify public API compatibility prior to implementing or updating protocol endpoints.

### Verify

```bash
# Discover and run the project static type inspection script to verify type adherence across protocol modules.
# Discover and run the test execution suite to validate tool registration and input and output schema parsing behavior.
# Discover and run the repository linter to ensure consistency and adherence to structural conventions.
```

**Accept when:**
- All protocol server tool registrations pass schema validation checks with zero type errors.
- Automated test suites verify that invalid tool inputs are rejected before reaching domain services.
- Protocol tool execution returns properly structured and validated output envelopes.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>