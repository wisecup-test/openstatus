---
version: 2
name: cross-cutting-outbound-payloads-constructed-external-c-0a73
description: "Adoption of Zod for Runtime Input and Boundary Validation: Outbound Payloads Constructed External Client Boundaries"
metadata:
  actual:
    adr_id: 0a730239-0414-461e-8183-353e28009769
    status: accepted
    candidate_group_id: 25160848-33da-4fdc-b666-faaa817d09fe
    evidence:
      source: deep
  scope:
    root: packages/notifications/pagerduty
    paths:
      - src/**
---

# Adoption of Zod for Runtime Input and Boundary Validation: Outbound Payloads Constructed External Client Boundaries

Scope: applies to `packages/notifications/pagerduty/src/**`.

These rules are ALWAYS ACTIVE for all code constructing outbound payloads for external client boundaries or handling untrusted ingestion and boundary validation.

### Rules

- **R-ZOD-001** SHOULD: Outbound payloads constructed for external client boundaries SHOULD be validated through Zod schema parsing before serialization and network transmission to guarantee contract conformance.
- **R-ZOD-002** MANDATORY: The consumer MUST discover dependency manifests and lock artifacts in the repository, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, and confirm every API or function exists in that version before writing code using a versioned library.
- **R-ZOD-003** MANDATORY: Define domain boundary schemas in dedicated schema modules to facilitate reuse across ingestion handlers, environment loaders, and external client dispatchers, and couple schema parsing directly with deserialization logic.

### Verify

```bash
# Discover the project dependency manifest and lock artifact to identify registered test and typecheck scripts, then execute the suite
MANIFEST=$(find . -maxdepth 2 -name "package.json" -o -name "pom.xml" -o -name "build.gradle" -o -name "go.mod" | head -n 1)
echo "Found manifest: $MANIFEST"

# Run typecheck, lint, and test suites to verify boundary schemas and Zod parsing
if [ -f "package.json" ]; then
  npm test || npx jest || yarn test
  npm run typecheck || npx tsc --noEmit
  npm run lint || npx eslint .
fi
```

**Accept when:**
- All boundary handlers parsing serialized incoming data or configuring external client dispatches pass automated test suites and reject malformed schemas with validation errors.
- Static analysis and typecheck suites complete with zero errors across all modules declaring and parsing Zod boundary schemas.
- The resolved library version matches the documented API usage confirmed from the authoritative dependency lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests omitting Zod validation at untrusted input boundaries will be blocked, and runtime schema validation failures must log structured error details and halt further processing.
</enforcement>