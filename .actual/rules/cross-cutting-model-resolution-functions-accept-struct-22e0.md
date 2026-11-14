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

- **R-AI-001** SHOULD: Model resolution functions MUST accept structured model configuration derived from workspace database schemas to dynamically instantiate provider instances.

### Verify

```bash
# Discover repository package manifest and execute configured test suite
npm test

# Run project static analysis and type checking
npm run check
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and automated tests/static type checking verify compliance. Direct imports of @ai-sdk/openai-compatible outside the model resolution layer will be blocked from merging.
</enforcement>