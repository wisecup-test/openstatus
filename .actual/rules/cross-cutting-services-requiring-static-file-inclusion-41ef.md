---
version: 2
name: cross-cutting-services-requiring-static-file-inclusion-41ef
description: "embed Module Adoption: Services Requiring Static File Inclusion Compile"
metadata:
  actual:
    adr_id: 41ef8c06-2eb1-41ed-a01d-dd08cecc91ab
    status: accepted
    candidate_group_id: 2530dec0-f9bd-4f1b-857d-16466d46cffc
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# embed Module Adoption: Services Requiring Static File Inclusion Compile

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for standalone services that compile static assets directly into binaries and components requiring immutable embedded file systems.

### Rules

- **R-EMBED-001** MUST: Services requiring static file inclusion at compile time MUST adopt the embed module to package static assets directly into distributed binaries.

### Verify

```bash
# Discover and run the project's standard build and test suite scripts from the repository configuration to confirm asset compilation.
# Execute repository static analysis and security scanning checks discovered from the workspace configuration.
```

**Accept when:**
- The service compiles successfully with all embedded assets verified by automated tests.
- No relative filesystem path dependencies remain for bundled static assets.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>