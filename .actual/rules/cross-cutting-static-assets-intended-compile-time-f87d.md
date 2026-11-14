---
version: 2
name: cross-cutting-static-assets-intended-compile-time-f87d
description: "embed Module Adoption: Static Assets Intended Compile Time Embedding"
metadata:
  actual:
    adr_id: f87d30d5-32ae-4fda-8db1-39b0b2f439de
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Static Assets Intended Compile Time Embedding

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for standalone services that compile static assets directly into binaries and components requiring immutable embedded file systems.

### Rules

- **R-EMBED-001** SHOULD: Static assets intended for compile-time embedding SHOULD be structured in dedicated asset directories isolated from operational service logic.

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