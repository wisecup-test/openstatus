---
version: 2
name: cross-cutting-tool-modules-validate-input-arguments-5996
description: "Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Validate Input Arguments Against"
metadata:
  actual:
    adr_id: 59963acf-1334-441a-8c7a-d89bac4b133b
    status: accepted
    candidate_group_id: 3144f39b-a2d3-4371-82a9-fada4e10611e
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/mcp/tools/**
---

# Adoption of @modelcontextprotocol/sdk for MCP Server Tool Endpoints: Tool Modules Validate Input Arguments Against

Scope: applies to `apps/server/src/routes/mcp/tools/**`.

These rules are ALWAYS ACTIVE for tool handler modules exposing server domain capabilities through the Model Context Protocol and modules integrating internal domain services with external agent execution environments.

### Rules

- **R-MCP-001** SHOULD: Tool modules SHOULD validate input arguments against strict schemas before delegating execution to underlying service operations.
- **R-MCP-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-MCP-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-verify per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute the project test execution script for tool route handlers
# Discover and execute static analysis and linting scripts across the server codebase
# Discover and verify build and type checking scripts across all protocol integration endpoints
```

**Accept when:**
- All tool handler test suites pass without regressions across domain operations.
- Static type analysis confirms full type conformance between domain service adapters and protocol SDK interfaces.
- All agent-facing tool registrations validate against the protocol specification.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing direct domain coupling or bypassing the protocol SDK must be rejected during review. Build failures resulting from type mismatches or unverified dependency versions must block merge.
</enforcement>