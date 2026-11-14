---
version: 2
name: cross-cutting-before-implementing-referencing-versione-c5ea
description: "Evaluation of after Callback within @trpc/server Middleware: Before Implementing Referencing Versioned Dependencies Consumer"
metadata:
  actual:
    adr_id: c5eaad74-4c0a-4b64-9c20-619f2e577b06
    status: accepted
    candidate_group_id: 488a7432-7d16-4be8-9ce8-5827c5960bf3
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Evaluation of after Callback within @trpc/server Middleware: Before Implementing Referencing Versioned Dependencies Consumer

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for server-side procedure middleware pipelines defined with @trpc/server.

### Rules

- **R-TRPC-001** MUST: Before implementing or referencing versioned dependencies, the consumer MUST inspect the repository lock artifact and verify the exact resolved version.
- **R-TRPC-002** MUST: Locate @trpc/server procedure definitions within the API package boundary and verify that post-response work is decoupled from test assertions.

### Verify

```bash
# Discover and run the project test execution script from the repository manifest to verify procedure logic independently.
TEST_SCRIPT=$(node -p "require('./package.json').scripts?.test || ''")
if [ -n "$TEST_SCRIPT" ]; then
  npm test
else
  echo "No test script defined in package.json"
  exit 1
fi
```

**Accept when:**
- The project test verification scripts complete without relying on procedure middleware lifecycle hooks for test assertions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>