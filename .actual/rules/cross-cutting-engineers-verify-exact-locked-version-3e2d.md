---
version: 2
name: cross-cutting-engineers-verify-exact-locked-version-3e2d
description: "Gin Framework for Service Endpoint Definitions: Engineers Verify Exact Locked Version Http"
metadata:
  actual:
    adr_id: 3e2dcf42-76c0-424a-86ec-596564f6d04a
    status: accepted
    candidate_group_id: f9f74977-16fb-4da2-9ee2-4a479df0625e
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Gin Framework for Service Endpoint Definitions: Engineers Verify Exact Locked Version Http

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Engineers MUST verify the exact locked version of the HTTP framework dependency in the repository lock artifact prior to invoking framework APIs.

### Verify

```bash
# Discover and execute the project test runner to validate HTTP route handler responses
# Discover and execute the project static analysis suite to verify framework router compliance
```

**Accept when:**
- All discovered endpoint test suites pass with zero assertions failing on route responses.
- Static analysis validates that route handlers adhere to framework context usage conventions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>