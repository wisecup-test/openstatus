---
version: 2
name: cross-cutting-before-implementing-updating-any-code-0a10
description: "Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Before Implementing Updating Any Code Dependent"
metadata:
  actual:
    adr_id: 0a10eb3f-223a-4c15-8fe3-77d4838eb438
    status: accepted
    candidate_group_id: 09c3ec39-9b4e-4ca5-a0fd-52f7a3257726
    evidence:
      source: deep
  scope:
    root: packages/proto
    paths:
      - gen/ts/**
---

# Adoption of @bufbuild/protobuf for Protocol Buffer Runtime and Code Generation: Before Implementing Updating Any Code Dependent

Scope: applies to `packages/proto/gen/ts/**`.

These rules are ALWAYS ACTIVE for all packages and modules containing generated protocol buffer definitions or consuming protocol buffer serialization logic, and all TypeScript message definitions, service descriptors, and schema annotations within the repository.

### Rules

- **R-BUF-001** MUST: Before implementing or updating any code dependent on the protocol buffer runtime, the consumer MUST discover the project dependency manifest and lock artifact to resolve the exact locked version in adherence to the Discovery Policy.

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
Claude Code MUST NOT skip or defer verification. Continuous integration build steps, automated linting rules, and code reviews enforce these requirements.
</enforcement>