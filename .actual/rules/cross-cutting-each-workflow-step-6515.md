# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Each Workflow Step

These rules are ALWAYS ACTIVE for all scheduled monitor execution workflows, cron-triggered workflow initialization, multi-step workflows with time-based transitions, and user-scoped workflow execution with rate limiting.

### Rules

- **R-WORKFLOW-001** MUST: Each workflow step MUST be implemented as a distinct, named function that can be independently invoked and tested.
- **R-WORKFLOW-002** MUST: Define workflow step contracts as exported types or interfaces that declare input parameters, return types, and error conditions for each step in the execution sequence.
- **R-WORKFLOW-003** MUST: Implement cache key naming conventions that include workflow identifiers and user scope to prevent key collisions across concurrent workflow executions.
- **R-WORKFLOW-004** MUST: Configure rate limiter token intervals based on observed workflow execution frequency and adjust dynamically based on system load metrics.
- **R-WORKFLOW-005** MUST: Set cache expiration policies to exceed maximum workflow duration and implement state recovery mechanisms.
- **R-WORKFLOW-006** MUST: Implement structured error logging and monitoring for validation failures with alerting thresholds.

### Verify

```bash
# Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
grep -r "export.*function" --include="*.ts" --include="*.js" | grep -i workflow

# Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
grep -r "expir" --include="*.ts" --include="*.js" --include="*.json" | grep -i cache

# Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level
grep -r "rate.*limit\|token.*interval" --include="*.ts" --include="*.js" --include="*.json"

# Verify workflow state persistence across step transitions
grep -r "state.*persist\|persist.*state" --include="*.ts" --include="*.js"
```

**Accept when:**
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing
- Workflow state persistence is implemented with recovery mechanisms across step transitions
- Structured error logging and monitoring for validation failures is in place with alerting

<enforcement>
Clause Code MUST NOT skip or defer verification. Workflow implementations without explicit step contracts are rejected in code review. Cache configurations without expiration policies trigger build-time validation failures. Rate limiting violations detected in load testing block deployment to production.
</enforcement>