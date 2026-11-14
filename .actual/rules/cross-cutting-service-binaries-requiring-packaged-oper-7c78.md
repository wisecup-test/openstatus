---
version: 2
name: cross-cutting-service-binaries-requiring-packaged-oper-7c78
description: "Adoption of embed Package for In-Memory Asset Integration: Service Binaries Requiring Packaged Operational Data"
metadata:
  actual:
    adr_id: 7c783760-59c2-47a7-9cb9-4d8516af3721
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Service Binaries Requiring Packaged Operational Data

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service entrypoints and compiled executables requiring bundled static assets and standard serialization handling.

### Rules

- **R-EMB-001** MUST: Service binaries requiring packaged operational data MUST utilize the embed package to inline assets directly into compilation units instead of relying on external runtime filesystem paths.

### Verify

```bash
# Discover the project test runner from repository manifests and execute the test suite to verify embedded asset loading.
# Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors.
```

**Accept when:**
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipelines and peer code review enforce that asset encapsulation relies on embed declarations rather than runtime relative paths.
</enforcement>