---
version: 2
name: cross-cutting-embedded-asset-declarations-not-referenc-61fa
description: "Adoption of embed Package for In-Memory Asset Integration: Embedded Asset Declarations Not Reference Mutable"
metadata:
  actual:
    adr_id: 61faffa5-6179-4ecf-84e6-628d7128c2f4
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Embedded Asset Declarations Not Reference Mutable

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service entrypoints and compiled executables requiring bundled static assets and standard serialization handling.

### Rules

- **R-EMBED-001** MUST_NOT: Embedded asset declarations MUST_NOT reference mutable runtime paths or expose write interfaces on embedded content.

### Verify

```bash
# Discover the project test runner from repository manifests and execute the test suite to verify embedded asset loading.
# Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors.
```

**Accept when:**
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code changes that introduce runtime filesystem dependencies for static service assets must be rejected during review and refactored.
</enforcement>