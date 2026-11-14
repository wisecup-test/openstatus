---
version: 2
name: cross-cutting-model-resolution-modules-encapsulate-pro-6886
description: "Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Model Resolution Modules Encapsulate Provider Instantiation"
metadata:
  actual:
    adr_id: 6886609d-293b-4182-b479-081e822e5cd0
    status: accepted
    candidate_group_id: c5f02e5f-768e-49c4-9e06-66844f1be5cb
    evidence:
      source: deep
  scope:
    root: packages/ai
    paths:
      - src/**
---

# Adoption of @ai-sdk/openai-compatible for Model Provider Resolution: Model Resolution Modules Encapsulate Provider Instantiation

Scope: applies to `packages/ai/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MOD-001** MUST: Model resolution modules MUST encapsulate provider instantiation, credential mapping, and endpoint base URL configuration within the dedicated AI subsystem boundary.

### Verify

```bash
# Discover the repository package manifest and execute the configured test suite to verify model resolution behavior across configured provider endpoints.
# Discover and run the project static analysis and type checking scripts to verify contract conformity for all provider factory invocations.
```

**Accept when:**
- The automated test suite passes with all OpenAI-compatible provider resolution tests asserting valid language model instance instantiation.
- Static type analysis confirms zero type errors across module boundaries importing and consuming the model resolution exports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipeline running automated tests and static type checking across the AI module, and mandatory architectural peer review on pull requests modifying model resolution or provider configuration logic.
</enforcement>