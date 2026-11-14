---
version: 2
name: cross-cutting-input-validation-deserialization-logic-e-2e27
description: "Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Input Validation Deserialization Logic Executed Within"
metadata:
  actual:
    adr_id: 2e27a948-9412-4e26-8a21-579d986de7e1
    status: accepted
    candidate_group_id: a89df71c-b426-4d94-a699-ab4046839363
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/preferred-settings/**
      - src/trpc/**
---

# Internal `./shared` Module Adoption for Isomorphic Client-Server Boundaries: Input Validation Deserialization Logic Executed Within

Scope: applies to `apps/web/src/lib/preferred-settings/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHARED-001** SHOULD: Input validation and deserialization logic executed within runtime-specific entrypoints SHOULD rely on schemas declared within the internal `./shared` module.

### Verify

\`\`\`bash
# Discover the project verification script from the repository manifest and execute module boundary and type checking.
# Discover the repository test runner script and execute integration tests verifying client and server entrypoint isolation.
# Discover the build analysis script and execute production bundle evaluation to ensure no server-side imports appear in client artifacts.
\`\`\`

**Accept when:**
- Static dependency analysis reports zero illegal cross-tier imports between client and server module entrypoints.
- All shared contracts and validation schemas compile cleanly without referencing environment-specific globals.
- Production client bundle inspection confirms complete absence of server-exclusive packages and header stores.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>