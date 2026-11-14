---
version: 2
name: cross-cutting-static-assets-incorporated-via-embed-8c5e
description: "embed Module Adoption: Static Assets Incorporated Via Embed Module"
metadata:
  actual:
    adr_id: 8c5e0fb1-326c-4047-9cc0-cd52ad2b9090
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Static Assets Incorporated Via Embed Module

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for standalone services that compile static assets directly into binaries and components requiring immutable embedded file systems.

### Rules

- **R-EMBED-001** MUST: Static assets incorporated via the embed module MUST be accessed through standard in-memory file system interfaces rather than external relative file paths.

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