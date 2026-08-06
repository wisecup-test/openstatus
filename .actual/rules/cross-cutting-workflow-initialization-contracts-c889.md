# Enforce Workflow Step Contracts for Distributed Cron Monitoring: Workflow Initialization Contracts

These rules are ALWAYS ACTIVE for all workflow orchestration components that coordinate distributed monitoring tasks across temporal boundaries, including step transition logic, task queue submission interfaces, and rate-limited workflow execution paths.

### Rules

- **R-WIC-001** MUST: Workflow initialization contracts MUST specify cache key patterns, expiration policies, and rate limiting constraints that apply to all subsequent step executions.
- **R-WIC-002** MUST: Define workflow step contracts as exported types that specify input state structure, output state structure, temporal progression rules, and failure recovery semantics.
- **R-WIC-003** MUST: Implement contract validation at workflow initialization and step transition boundaries using schema parsing to catch state inconsistencies before they propagate to cache layers or task queues.
- **R-WIC-004** MUST: Design cache key patterns and expiration policies as part of the public contract surface when cache consistency is critical to workflow reliability.
- **R-WIC-005** MUST: Separate workflow contracts into initialization contracts, step transition contracts, and completion contracts to enable independent testing and evolution of each workflow stage.
- **R-WIC-006** SHOULD: Co-locate workflow step contracts with workflow orchestration logic for discoverability.
- **R-WIC-007** SHOULD: Document the relationship between cache TTL values and workflow temporal boundaries to prevent premature state eviction.
- **R-WIC-008** SHOULD: Log contract validation failures with sufficient context for debugging distributed coordination issues.

### Verify

```bash
# Discover the project's workflow contract type definitions and verify that all step transition types export explicit input state, output state, and temporal progression semantics
grep -r "LaunchMonitorWorkflow\|Step14Days\|Step3Days\|StepPaused\|workflowStep" --include="*.ts" --include="*.tsx" | head -20

# Locate the project's workflow orchestration test suite and execute tests that verify contract validation occurs at initialization and step transition boundaries
find . -name "*.test.ts" -o -name "*.spec.ts" | xargs grep -l "workflow.*contract\|step.*transition" | head -10

# Identify the project's cache layer integration and verify that cache key patterns and expiration policies referenced in workflow contracts match actual cache operations
grep -r "cache.*key\|TTL\|expiration" --include="*.ts" --include="*.tsx" | grep -i workflow | head -20
```

**Accept when:**
- All workflow step contracts define explicit types for state transitions and temporal progression, verified by successful compilation without type errors
- Contract validation logic executes at workflow initialization and step boundaries, verified by test coverage showing validation failures prevent invalid state transitions
- Cache operations use key patterns and expiration policies consistent with workflow contract specifications, verified by integration tests that exercise full workflow lifecycle
- Workflow contracts are co-located with orchestration logic and exported for discoverability
- Cache TTL values and temporal boundaries are documented in contract definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. Type system verification during compilation ensures workflow contracts are satisfied at all coordination boundaries. Integration tests must exercise full workflow lifecycle including initialization, multi-day step transitions, and paused state handling. Code review must verify that new workflow steps define explicit contracts and implement validation at transition boundaries.
</enforcement>