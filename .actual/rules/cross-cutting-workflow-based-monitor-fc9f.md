# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Workflow Based Monitor

These rules are ALWAYS ACTIVE for all scheduled monitor execution workflows, cron-triggered workflow initialization, multi-step workflows with time-based transitions, and user-scoped workflow execution with rate limiting.

### Rules

- **R-WBM-001** MUST: Workflow-based monitor execution MUST define explicit step contracts that declare workflow initialization, intermediate states, and terminal states.
- **R-WBM-002** MUST: Define workflow step contracts as exported types or interfaces that declare input parameters, return types, and error conditions for each step in the execution sequence.
- **R-WBM-003** MUST: Implement cache key naming conventions that include workflow identifiers and user scope to prevent key collisions across concurrent workflow executions.
- **R-WBM-004** MUST: Configure rate limiter token intervals based on observed workflow execution frequency and adjust dynamically based on system load metrics.
- **R-WBM-005** MUST: Set cache expiration policies to exceed maximum workflow duration and implement state recovery mechanisms.
- **R-WBM-006** MUST: Implement structured error logging and monitoring for validation failures with alerting thresholds.

### Verify

```bash
# Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
grep -r "export.*function\|export.*interface\|export.*type" --include="*.ts" --include="*.js" | grep -i "step\|workflow" || echo "No workflow step definitions found"

# Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
grep -r "expir\|ttl\|maxAge" --include="*.ts" --include="*.js" --include="*.json" --include="*.yaml" --include="*.yml" | grep -i "cache\|workflow" || echo "No cache configuration found"

# Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level
grep -r "rateLimit\|token.*interval\|throttle" --include="*.ts" --include="*.js" --include="*.json" --include="*.yaml" --include="*.yml" | grep -i "workflow\|monitor" || echo "No rate limiting configuration found"
```

**Accept when:**
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing
- Schema validation at workflow boundaries is implemented and catches data integrity issues before execution begins
- Workflow state persistence across step transitions is verified by integration tests

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review verification that workflow steps follow explicit contract definitions is mandatory. Integration tests that validate workflow state persistence across step transitions are mandatory. Load testing that verifies rate limiting prevents resource exhaustion is mandatory. Workflow implementations without explicit step contracts are rejected in code review. Cache configurations without expiration policies trigger build-time validation failures. Rate limiting violations detected in load testing block deployment to production.
</enforcement>