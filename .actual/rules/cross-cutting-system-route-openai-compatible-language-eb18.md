---
version: 2
name: cross-cutting-system-route-openai-compatible-language-eb18
description: "Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: System Route Openai Compatible Language Model"
metadata:
  actual:
    adr_id: eb18331f-6eef-47ab-b0b9-f72a405f5b5d
    status: accepted
    candidate_group_id: c5f02e5f-768e-49c4-9e06-66844f1be5cb
    evidence:
      source: deep
  scope:
    root: packages/ai
    paths:
      - src/**
---

# Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: System Route Openai Compatible Language Model

Scope: applies to `packages/ai/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SYS-001** MUST: The system MUST route all OpenAI-compatible language model resolution through @ai-sdk/openai-compatible provider adapters rather than direct HTTP client calls or proprietary vendor client libraries.

### Verify

```bash
# Discover the repository package manifest and execute the configured test suite to verify model resolution behavior across configured provider endpoints.
# Discover and run the project static analysis and type checking scripts to verify contract conformity for all provider factory invocations.
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>