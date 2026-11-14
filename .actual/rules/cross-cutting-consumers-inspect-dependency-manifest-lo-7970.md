---
version: 2
name: cross-cutting-consumers-inspect-dependency-manifest-lo-7970
description: "Adoption of embed Package for In-Memory Asset Integration: Consumers Inspect Dependency Manifest Lock Artifact"
metadata:
  actual:
    adr_id: 79707ff4-b0ba-447f-96c1-35c908438382
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Consumers Inspect Dependency Manifest Lock Artifact

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service entrypoints and compiled executables requiring bundled static assets and standard serialization handling.

### Rules

- **R-EMBED-001** MUST: Consumers MUST inspect the dependency manifest and lock artifact in the repository to confirm toolchain support for the embed package before implementation.

### Verify

```bash
# Discover the project test runner from repository manifests and execute the test suite to verify embedded asset loading.
# Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors.
```

**Accept when:**
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>