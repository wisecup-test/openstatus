# Enforce Workflow Step Contracts for Distributed Cron Monitoring: Step Contracts Encode

These rules are ALWAYS ACTIVE for all workflow orchestration components that coordinate distributed monitoring tasks across temporal boundaries, including step transition logic, task queue submission interfaces, and rate-limited workflow execution paths.

### Rules

- **R-STEP-001** MUST: Step contracts MUST encode temporal boundaries and state validation requirements to ensure correct progression through multi-day monitoring stages.
- **R-STEP-002** MUST: Define workflow step contracts as exported types that specify input state structure, output state structure, temporal progression rules, and failure recovery semantics.
- **R-STEP-003** MUST: Implement contract validation at workflow initialization and step transition boundaries using schema parsing to catch state inconsistencies before they propagate to cache layers or task queues.
- **R-STEP-004** MUST: Separate workflow contracts into initialization contracts, step transition contracts, and completion contracts to enable independent testing and evolution of each workflow stage.
- **R-STEP-005** SHOULD: Design cache key patterns and expiration policies as part of the public contract surface when cache consistency is critical to workflow reliability.
- **R-STEP-006** SHOULD: Document the relationship between cache TTL values and workflow temporal boundaries to prevent premature state eviction.
- **R-STEP-007** SHOULD: Log contract validation failures with sufficient context for debugging distributed coordination issues.

### Verify

```bash
# Discover the project's workflow contract type definitions
find . -type f -name '*.ts' -o -name '*.js' | xargs grep -l 'LaunchMonitorWorkflow\|Step14Days\|Step3Days\|StepPaused\|workflowStep' | head -20

# Verify that all step transition types export explicit input state, output state, and temporal progression semantics
grep -r 'export.*type.*Step' . --include='*.ts' --include='*.js' | grep -E '(input|output|state|temporal)'

# Locate the project's workflow orchestration test suite
find . -type f \( -name '*.test.ts' -o -name '*.spec.ts' -o -name '*workflow*.test.*' \) | head -20

# Execute tests that verify contract validation occurs at initialization and step transition boundaries
npm test -- --testPathPattern='workflow|step|contract' 2>&1 | grep -E '(PASS|FAIL|validation)'

# Identify the project's cache layer integration
grep -r 'cache.*key\|TTL\|expiration' . --include='*.ts' --include='*.js' | grep -v node_modules | head -20

# Verify that cache key patterns and expiration policies referenced in workflow contracts match actual cache operations
grep -r 'cache.*set\|cache.*get' . --include='*.ts' --include='*.js' | grep -v node_modules | head -20
```

**Accept when:**
- All workflow step contracts define explicit types for state transitions and temporal progression, verified by successful compilation without type errors
- Contract validation logic executes at workflow initialization and step boundaries, verified by test coverage showing validation failures prevent invalid state transitions
- Cache operations use key patterns and expiration policies consistent with workflow contract specifications, verified by integration tests that exercise full workflow lifecycle
- Workflow contracts are co-located with workflow orchestration logic for discoverability
- Initialization contracts, step transition contracts, and completion contracts are separated to enable independent testing

<enforcement>
Claude Code MUST NOT skip or defer verification. Type system verification during compilation, integration tests exercising full workflow lifecycle, and code review checklist verification are mandatory before accepting any workflow step contract implementation.
</enforcement>