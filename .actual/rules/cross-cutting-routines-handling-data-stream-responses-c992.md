---
version: 2
name: cross-cutting-routines-handling-data-stream-responses-c992
description: "Adoption of embed Package for In-Memory Asset Integration: Routines Handling Data Stream Responses Close"
metadata:
  actual:
    adr_id: c992a912-e02b-417a-ae9b-ea7e327d7bd2
    status: accepted
    candidate_group_id: b4f53085-bb41-4106-b8f7-d05ee9e097a8
    evidence:
      source: deep
  scope:
    root: apps/ssh-server
---

# Adoption of embed Package for In-Memory Asset Integration: Routines Handling Data Stream Responses Close

Scope: applies to `apps/ssh-server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EMBED-001** MUST: Routines handling data stream responses MUST close the underlying response body and manage stream terminations using standard io utilities.

### Verify

```bash
# Discover the project test runner from repository manifests and execute the test suite to verify embedded asset loading.
# Discover repository static analysis and compilation scripts to ensure embedded directives resolve without errors.
```

**Accept when:**
- The project test suite passes with all embedded assets successfully resolved and parsed into memory.
- Static analysis and compilation routines succeed with zero unresolved resource references.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>