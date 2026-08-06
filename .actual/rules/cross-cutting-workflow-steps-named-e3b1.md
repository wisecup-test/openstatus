# Adopt Workflow-Based Concurrency Model for Scheduled Monitor Execution: Workflow Steps Named

These rules are ALWAYS ACTIVE for all scheduled monitor execution workflows, cron-triggered workflow initialization, multi-step workflows with time-based transitions, and user-scoped workflow execution with rate limiting.

### Rules

- **R-WORKFLOW-001** SHOULD: Workflow steps SHOULD be named to reflect their temporal or lifecycle position in the execution sequence.

### Verify

```bash
# Discover the project's workflow step definitions and verify that each step is exported as a distinct, testable function with explicit type contracts
grep -r "export.*function\|export.*const.*=>" --include="*.ts" --include="*.js" | grep -i "step\|workflow" || echo "No workflow step exports found"

# Locate the cache configuration and verify that expiration policies are set with explicit time units and exceed maximum workflow duration
grep -r "expir\|ttl\|maxAge" --include="*.ts" --include="*.js" --include="*.json" | grep -i "cache\|workflow" || echo "No cache expiration config found"

# Identify the rate limiting configuration and verify that token-based interval constraints are applied at the workflow execution level
grep -r "rateLimit\|token.*interval\|throttle" --include="*.ts" --include="*.js" --include="*.json" | grep -i "workflow\|monitor" || echo "No rate limiting config found"
```

**Accept when:**
- All workflow steps are defined as distinct functions with explicit contracts and can be independently invoked in test environments
- Cache layers use key naming conventions that include workflow and user scope, with expiration policies that exceed workflow duration
- Rate limiting is configured with token-based intervals and prevents resource exhaustion under load testing
- Workflow step names clearly reflect their temporal or lifecycle position (e.g., `initializeWorkflow`, `executeMonitor`, `finalizeResults`)

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow steps must be explicitly named and scoped. Cache expiration and rate limiting configurations must be validated before code review approval.
</enforcement>