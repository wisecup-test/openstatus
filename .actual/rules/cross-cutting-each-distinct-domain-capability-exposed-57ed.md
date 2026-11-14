---
version: 2
name: cross-cutting-each-distinct-domain-capability-exposed-57ed
description: "@modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Each Distinct Domain Capability Exposed Over"
metadata:
  actual:
    adr_id: 57edb25c-4a61-41c0-bdec-dbfd2d7330d7
    status: accepted
    candidate_group_id: 8a575d33-76df-41a9-b37f-156ab1a4407a
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/**
---

# @modelcontextprotocol/sdk/server/mcp.js Adoption for Model Context Protocol Server: Each Distinct Domain Capability Exposed Over

Scope: applies to `apps/server/src/routes/mcp/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MCP-001** MUST: Each distinct domain capability exposed over the protocol MUST define its tool definitions in modular tool units that register through the centralized registry cache using unique tool identifiers.

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