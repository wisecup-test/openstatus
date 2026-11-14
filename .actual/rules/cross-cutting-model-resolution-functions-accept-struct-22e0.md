---
version: 2
name: cross-cutting-model-resolution-functions-accept-struct-22e0
description: "Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Model Resolution Functions Accept Structured Configuration"
metadata:
  actual:
    adr_id: 22e040d7-dee2-4a92-bcb6-627e71cb6580
    status: accepted
    candidate_group_id: c5f02e5f-768e-49c4-9e06-66844f1be5cb
    evidence:
      source: deep
  scope:
    root: packages/ai
    paths:
      - src/**
---

# Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Model Resolution Functions Accept Structured Configuration

Scope: applies to `packages/ai/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AI-001** SHOULD: Model resolution functions SHOULD accept structured model configuration derived from workspace database schemas to dynamically instantiate provider instances.
- **R-AI-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-AI-003** MANDATORY: Before writing code that uses a versioned library, the developer MUST execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover the repository package manifest and execute the configured test suite
# to verify model resolution behavior across configured provider endpoints.
# (Run the project test command as defined in package.json)
npm test

# Run the project static analysis and type checking scripts
npm run typecheck
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and mandatory architectural peer review on pull requests will enforce these rules. Violations detected in static analysis must be refactored, and direct imports of @ai-sdk/openai-compatible outside the model resolution layer will be blocked from merging.
</enforcement>