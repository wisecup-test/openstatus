---
version: 2
name: cross-cutting-before-implementing-updating-service-han-538d
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Before Implementing Updating Service Handlers Utilizing"
metadata:
  actual:
    adr_id: 538d2123-735b-41af-b0cc-6142b6b28921
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Before Implementing Updating Service Handlers Utilizing

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** MUST: Before implementing or updating service handlers utilizing connectrpc.com/connect, engineers MUST discover the repository dependency lock artifact and verify the exact resolved library version against its official documentation.

### Verify

\`\`\`bash
# Discover the project test execution harness and run the test suite for all ingestion endpoint handlers.
# Discover the project code analysis linter to verify that all RPC handlers conform to ConnectRPC handler signatures.
\`\`\`

**Accept when:**
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>