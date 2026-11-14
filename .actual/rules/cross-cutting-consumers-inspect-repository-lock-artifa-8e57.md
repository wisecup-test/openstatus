---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-8e57
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Consumers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 8e577a05-5bb9-4ea9-997c-5cd674972e4e
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Consumers Inspect Repository Lock Artifact Determine

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NSV-001** MUST: Consumers MUST inspect the repository lock artifact to determine the exact resolved version of the framework dependency before implementing or consuming APIs from next/server.

### Verify

```bash
# Discover and run the repository dependency manifest and type checking script to verify next/server import compliance.
# Discover and execute the repository test runner script targeting route handlers and proxy modules.
# Discover and run the project linting script.
```

**Accept when:**
- All server route handlers and proxy modules compile successfully using next/server types without type errors.
- Automated test suites for route handlers, badges, and proxy redirection pass with zero regressions.
- No deprecated or unsupported server-side response methods are invoked across server endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>