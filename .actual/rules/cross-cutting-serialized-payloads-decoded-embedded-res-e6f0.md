---
version: 2
name: cross-cutting-serialized-payloads-decoded-embedded-res-e6f0
description: "Adoption of embed Package for In-Memory Asset Integration: Serialized Payloads Decoded Embedded Resources External"
metadata:
  actual:
    adr_id: e6f00ffa-00c0-4a6a-ae76-273b4b201029
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Serialized Payloads Decoded Embedded Resources External

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for service entrypoints and compiled executables requiring bundled static assets and standard serialization handling.

### Rules

- **R-EMBED-001** SHOULD: Serialized payloads decoded from embedded resources or external data clients SHOULD be unmarshaled into strongly typed data models using encoding/json routines.

### Verify

```bash
# Discover and execute project test suite to verify embedded asset loading
# Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors
```

**Accept when:**
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code changes that introduce runtime filesystem dependencies for static service assets must be rejected during review and refactored.
</enforcement>