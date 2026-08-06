# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Workflows Maintain Memory

These rules are ALWAYS ACTIVE for all scheduled monitor execution workflows, cron-triggered workflow initialization, multi-step workflows with time-based transitions, and user-scoped workflow execution with rate limiting.

### Rules

- **R-WORKFLOW-001** MAY: Workflows MAY maintain in-memory caches for user-scoped data to reduce database query load during batch operations.

### Verify

```bash
# Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
grep -r "export.*function" --include="*.ts" --include="*.js" | grep -i workflow | head -20

# Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
grep -r "expir" --include="*.ts" --include="*.js" --include="*.json" | grep -i cache | head -20

# Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level
grep -r "rate.*limit\|token.*interval" --include="*.ts" --include="*.js" --include="*.json" | head -20
```

**Accept when:**
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing

<enforcement>
Code review verification that workflow steps follow explicit contract definitions is mandatory. Integration tests that validate workflow state persistence across step transitions are mandatory. Load testing that verifies rate limiting prevents resource exhaustion is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>