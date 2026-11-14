---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-c753
description: "embed Module Adoption: Engineers Discover Repository Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: c7539fae-1c48-462c-8d45-7f53e52f1890
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Engineers Discover Repository Dependency Manifest Inspect

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EMB-001** MUST: Engineers MUST discover the repository dependency manifest and inspect the authoritative lock artifact to verify supported language and library versions prior to implementation.

### Verify

```bash
# Discover and run the project's standard build and test suite scripts from the repository configuration to confirm asset compilation.
# Execute repository static analysis and security scanning checks discovered from the workspace configuration.
```

**Accept when:**
- The service compiles successfully with all embedded assets verified by automated tests.
- No relative filesystem path dependencies remain for bundled static assets.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>