# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Rate Limiting Applied

These rules are ALWAYS ACTIVE for all scheduled monitor execution workflows, cron-triggered workflow initialization, multi-step workflows with time-based transitions, and user-scoped workflow execution with rate limiting.

### Rules

- **R-WFCM-001** MUST: Rate limiting MUST be applied at the workflow execution level with token-based interval constraints.
- **R-WFCM-002** MUST: Define workflow step contracts as exported types or interfaces that declare input parameters, return types, and error conditions for each step in the execution sequence.
- **R-WFCM-003** MUST: Implement cache key naming conventions that include workflow identifiers and user scope to prevent key collisions across concurrent workflow executions.
- **R-WFCM-004** MUST: Configure cache expiration policies with explicit time units that exceed maximum workflow duration.
- **R-WFCM-005** MUST: Implement structured error logging and monitoring for validation failures with alerting thresholds.
- **R-WFCM-006** SHOULD: Monitor rate limiter token exhaustion metrics and adjust interval parameters based on observed workflow execution patterns.
- **R-WFCM-007** SHOULD: Set cache expiration policies to exceed maximum workflow duration and implement state recovery mechanisms.

### Verify

```bash
# Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
grep -r "export.*function\|export.*interface\|export.*type" --include="*.ts" --include="*.js" | grep -i workflow | head -20

# Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
grep -r "expir\|ttl\|cache" --include="*.ts" --include="*.js" --include="*.json" | grep -i "time\|duration\|interval" | head -20

# Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level
grep -r "rate.*limit\|token.*interval\|rateLimit" --include="*.ts" --include="*.js" --include="*.json" | head -20

# Verify workflow state persistence across step transitions
grep -r "persist\|state.*transition\|workflow.*step" --include="*.ts" --include="*.js" | head -20
```

**Accept when:**
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing
- Workflow state persistence is implemented across step transitions with recovery mechanisms
- Structured error logging is in place for validation failures with alerting thresholds

<enforcement>
Clause MUST NOT skip or defer verification. Code review MUST confirm workflow steps follow explicit contract definitions. Integration tests MUST validate workflow state persistence across step transitions. Load testing MUST verify rate limiting prevents resource exhaustion. Workflow implementations without explicit step contracts are rejected in code review. Cache configurations without expiration policies trigger build-time validation failures. Rate limiting violations detected in load testing block deployment to production.
</enforcement>