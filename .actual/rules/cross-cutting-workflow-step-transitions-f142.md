# Enforce Workflow Step Contracts for Distributed Cron Monitoring: Workflow Step Transitions

These rules are ALWAYS ACTIVE for all workflow orchestration components that coordinate distributed monitoring tasks across temporal boundaries, including step transition logic, task queue submission interfaces, and rate-limited workflow execution paths.

### Rules

- **R-WF-001** MUST: All workflow step transitions MUST be governed by explicit public contract types that define input state, output state, and temporal progression semantics.
- **R-WF-002** MUST: Define workflow step contracts as exported types that specify input state structure, output state structure, temporal progression rules, and failure recovery semantics, co-located with workflow orchestration logic for discoverability.
- **R-WF-003** MUST: Implement contract validation at workflow initialization and step transition boundaries using schema parsing to catch state inconsistencies before they propagate to cache layers or task queues.
- **R-WF-004** MUST: Log contract validation failures with sufficient context for debugging distributed coordination issues.
- **R-WF-005** MUST: Design cache key patterns and expiration policies as part of the public contract surface when cache consistency is critical to workflow reliability.
- **R-WF-006** MUST: Document the relationship between cache TTL values and workflow temporal boundaries to prevent premature state eviction.
- **R-WF-007** MUST: Separate workflow contracts into initialization contracts (LaunchMonitorWorkflow), step transition contracts (Step3Days, Step14Days, StepPaused), and completion contracts to enable independent testing and evolution of each workflow stage.

### Verify

```bash
# Discover the project's workflow contract type definitions and verify that all step transition types export explicit input state, output state, and temporal progression semantics
find . -type f -name '*.ts' -o -name '*.js' | xargs grep -l 'LaunchMonitorWorkflow\|Step14Days\|Step3Days\|StepPaused\|workflowStep' | head -20

# Locate the project's workflow orchestration test suite and execute tests that verify contract validation occurs at initialization and step transition boundaries
find . -type f \( -name '*.test.ts' -o -name '*.spec.ts' -o -name '*.test.js' -o -name '*.spec.js' \) | xargs grep -l 'workflow\|contract\|transition' | head -20

# Identify the project's cache layer integration and verify that cache key patterns and expiration policies referenced in workflow contracts match actual cache operations
find . -type f -name '*.ts' -o -name '*.js' | xargs grep -l 'cache.*TTL\|expiration.*policy\|cache.*key' | head -20

# Verify contract validation logic executes at workflow initialization and step boundaries
find . -type f -name '*.ts' -o -name '*.js' | xargs grep -l 'selectWorkspaceSchema\.parse\|schema.*parse\|validate.*contract' | head -20
```

**Accept when:**
- All workflow step contracts define explicit types for state transitions and temporal progression, verified by successful compilation without type errors
- Contract validation logic executes at workflow initialization and step boundaries, verified by test coverage showing validation failures prevent invalid state transitions
- Cache operations use key patterns and expiration policies consistent with workflow contract specifications, verified by integration tests that exercise full workflow lifecycle
- Workflow contracts are co-located with orchestration logic and exported as public types
- Cache TTL values and temporal boundaries are documented in contract definitions
- Initialization, step transition, and completion contracts are separated for independent testing

<enforcement>
Claude Code MUST NOT skip or defer verification. Type system verification during compilation, integration tests exercising full workflow lifecycle, and code review checklist verification are mandatory before accepting workflow step contract implementations.
</enforcement>