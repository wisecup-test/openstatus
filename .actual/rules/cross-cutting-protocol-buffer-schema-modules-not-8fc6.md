---
version: 2
name: cross-cutting-protocol-buffer-schema-modules-not-8fc6
description: "Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Protocol Buffer Schema Modules Not Import"
metadata:
  actual:
    adr_id: 8fc68f2a-20c6-4614-ae26-a67f26992a01
    status: accepted
    candidate_group_id: 09c3ec39-9b4e-4ca5-a0fd-52f7a3257726
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/**
---

# Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Protocol Buffer Schema Modules Not Import

Scope: applies to `packages/proto/gen/ts/**`.

These rules are ALWAYS ACTIVE for all packages and modules containing generated protocol buffer definitions or consuming protocol buffer serialization logic, including all TypeScript message definitions, service descriptors, and schema annotations within the repository.

### Rules

- **R-BUF-001** MUST_NOT: Protocol buffer schema modules MUST NOT import alternative or conflicting protocol buffer runtime libraries within workspace packages.

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
Claude Code MUST NOT skip or defer verification. All protocol buffer schema modules must compile against the resolved library dependencies without unauthorized imports from alternative runtimes.
</enforcement>