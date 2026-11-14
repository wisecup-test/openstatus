---
version: 2
name: cross-cutting-schemas-not-bypass-google-golang-2166
description: "Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Schemas Not Bypass Google Golang Org"
metadata:
  actual:
    adr_id: 21664dc8-0ac5-4528-886b-13da727b9cfc
    status: accepted
    candidate_group_id: 0c36f9ee-c7b4-4b8d-b135-88814b53f1d7
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - proto/private_location/v1/**
---

# Adoption of google.golang.org/protobuf for Structured Serialization and Message Reflection: Schemas Not Bypass Google Golang Org

Scope: applies to `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PB-001** MUST_NOT: Schemas MUST NOT bypass the google.golang.org/protobuf runtime reflection abstractions when inspecting message metadata across bounded contexts.
- **R-PB-002** MANDATORY: Follow the mandatory lock-version grounding policy to verify runtime library versions against the repository lock artifact before writing code that uses a versioned library.
- **R-PB-003** MANDATORY: Enforce thread-safe single-execution synchronization blocks for all raw descriptor decompression logic and coordinate descriptor registration to guarantee concurrent safety across worker threads.

### Verify

```bash
# Find the repository build and validation script defined in the project configuration and execute it to verify message schema compilation.
# Locate the contract test execution command from the project manifest and run test suites validating ProtoMessage serialization and reflection behaviors.
# Query the repository dependency auditing mechanism to confirm all references to the serialization runtime resolve to the locked dependency version.
```

**Accept when:**
- All message definitions pass validation and schema generation checks without compilation errors.
- Test suites asserting message serialization, deserialization, and contract interface compliance pass completely.
- The resolved library version matches the exact specification recorded in the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and peer review enforce these rules; violations block merges and schema compilation failures halt builds.
</enforcement>