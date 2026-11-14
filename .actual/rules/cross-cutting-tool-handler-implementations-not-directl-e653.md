---
version: 2
name: cross-cutting-tool-handler-implementations-not-directl-e653
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Handler Implementations Not Directly Expose"
metadata:
  actual:
    adr_id: e65367ee-e78f-42a1-ba91-1c504f880c9d
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Handler Implementations Not Directly Expose

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MAND-001** MUST_NOT: Tool handler implementations MUST NOT directly expose underlying domain service exceptions or internal database entities across the protocol interface boundary.

### Verify

```bash
# Discover and execute project test, lint, and build scripts from the repository manifest:
# 1. Run test suite for tool route handlers
# 2. Run static analysis and linting across the server codebase
# 3. Run build and type checking across all protocol integration endpoints
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review. Build failures resulting from type mismatches or unverified dependency versions must block merge.
</enforcement>