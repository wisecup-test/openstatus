---
version: 2
name: cross-cutting-generated-protocol-buffer-bindings-align-d715
description: "Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Generated Protocol Buffer Bindings Align Their"
metadata:
  actual:
    adr_id: d7152945-e97b-4305-a9b8-5ba5a23c935d
    status: accepted
    candidate_group_id: 09c3ec39-9b4e-4ca5-a0fd-52f7a3257726
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/**
---

# Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Generated Protocol Buffer Bindings Align Their

Scope: applies to `packages/proto/gen/ts/**`.

These rules are ALWAYS ACTIVE for all packages and modules containing generated protocol buffer definitions or consuming protocol buffer serialization logic, and all TypeScript message definitions, service descriptors, and schema annotations within the repository.

### Rules

- **R-BUF-001** MUST: Generated protocol buffer bindings MUST align their code generation runtime assumptions with the exact version of the adopted runtime library present in the repository lock artifact.

### Verify

```bash
# Find the project dependency manifest and inspect the declared runtime dependencies to identify the protocol buffer library.
# Inspect the repository resolution lock artifact to verify the exact resolved version of the protocol buffer runtime.
# Run the project verification scripts to ensure all generated protocol buffer modules build cleanly without dependency or type errors.
```

**Accept when:**
- The package dependency manifest and lock artifact define the resolved protocol buffer runtime library without version discrepancies.
- Automated static verification confirms all generated protocol buffer modules resolve runtime primitives exclusively from the adopted library packages.
- Repository test and build suites execute successfully with all protocol buffer serialization and validation tests passing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>