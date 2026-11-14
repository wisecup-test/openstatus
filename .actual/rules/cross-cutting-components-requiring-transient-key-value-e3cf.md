---
version: 2
name: cross-cutting-components-requiring-transient-key-value-e3cf
description: "Standard Runtime Key-Value Mapping and Ephemeral State Access: Components Requiring Transient Key Value Associations"
metadata:
  actual:
    adr_id: e3cfaedb-4367-462c-9ef0-c81ee6e9f6ce
    status: accepted
    candidate_group_id: 012be8c2-fbee-4f98-a859-b7530f4838b8
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
      - src/trpc/**
---

# Standard Runtime Key-Value Mapping and Ephemeral State Access: Components Requiring Transient Key Value Associations

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for transient in-memory data associations, request query parameter lookups, and related component logic within isolated execution boundaries.

### Rules

- **R-KV-001** MUST: Components requiring transient key-value associations within a single execution cycle MUST use local runtime mapping primitives rather than provisioning external distributed cache resources.
- **R-KV-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-KV-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence (manifest discovery, build tool identification, lock file inspection, version-specific documentation lookup, and API existence confirmation).
- **R-KV-004** MUST: Scope in-memory key-value data structures strictly to local function scopes to prevent unintended state sharing across asynchronous boundaries.
- **R-KV-005** MUST: Ensure keys follow structured namespaces when introducing external caching clients to prevent collision across independent application domains.

### Verify

```bash
# Discover and run static analysis and type verification scripts
# Example discovery / execution pattern (derive from repo manifest/scripts):
if [ -f package.json ]; then
  npm run lint || npx tsc --noEmit
elif [ -f pyproject.toml ]; then
  pytest && ruff check .
fi

# Execute repository test suite
if [ -f package.json ]; then
  npm test
elif [ -f pyproject.toml ]; then
  pytest
fi
```

**Accept when:**
- All static analysis and linting checks complete with zero errors.
- Unit and integration tests verify that transient state structures do not persist across isolated test cases.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>