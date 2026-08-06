# Log Message Queue Interactions at Service Boundaries: Message Queue Consumption

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active for all components that interact with message queue boundaries.

## Context

- Message queue interactions represent asynchronous boundaries where failures, timeouts, and data corruption can occur without immediate visibility to calling code.
- The codebase coordinates Slack message updates, action confirmations, and tool execution through ephemeral state stored in cache layers and consumed via message queue patterns.
- Error conditions in message queue workflows (expired actions, invalid payloads, execution failures) require diagnostic context to trace failures across asynchronous boundaries.
- The evidence shows console.error calls at message queue consumption points, action execution handlers, and cache validation failures, indicating a pattern of logging at these architectural boundaries.

## Problem Statement

Asynchronous message queue interactions create observability gaps where failures occur outside the request-response cycle, making it difficult to diagnose issues without explicit logging at consumption, validation, and execution boundaries.

## Decision

1. MUST: All message queue consumption points MUST log the operation outcome, including success confirmations and failure diagnostics with sufficient context to correlate the message to its producer.

## Policy Block

- MUST All message queue consumption points MUST log the operation outcome, including success confirmations and failure diagnostics with sufficient context to correlate the message to its producer.

In scope:
- Message queue consumers that deserialize and process messages
- Action execution handlers triggered by queue events
- Cache-backed confirmation stores that mediate asynchronous workflows
- Tool renderers and result processors invoked via message-driven patterns

Out of scope:
- Synchronous HTTP request handlers that do not interact with message queues
- Database query logging unrelated to message queue state
- Client-side logging in browser or mobile applications

## Rationale

- The evidence shows three distinct files implementing message queue boundary patterns with console.error logging at failure points, indicating an established practice of logging at these boundaries.
- Message queue interactions in the codebase involve ephemeral state (cache-backed pending actions with TTL), making post-mortem debugging impossible without contemporaneous logs.
- The observed logging pattern captures action execution errors, validation failures, and expired action scenarios, all of which represent failure modes unique to asynchronous message-driven architectures.
- Consistent logging at message queue boundaries enables correlation of failures across service boundaries and provides visibility into the health of asynchronous workflows.

## Consequences

Positive:
- Improved observability into asynchronous workflow failures that would otherwise be silent or manifest as user-facing timeouts.
- Faster incident response and debugging by providing diagnostic context at the point of failure rather than requiring reproduction.
- Ability to monitor message queue health and identify patterns of validation failures, expired actions, or execution errors.
- Consistent logging conventions across message queue boundaries reduce cognitive load when troubleshooting cross-service issues.

Negative:
- Increased log volume from message queue operations may require log aggregation and retention policies to manage storage costs.
- Sensitive data in message payloads must be sanitized before logging, adding complexity to logging implementations.
- Console-based logging may not provide sufficient structure for automated alerting or metrics extraction without additional tooling.
- Logging overhead at high message throughput may introduce latency or resource contention if not implemented efficiently.

## Alternatives

- Rely on distributed tracing without explicit logging at message queue boundaries (rejected)
  Rejected because: Distributed tracing requires instrumentation and infrastructure not evident in the codebase, and does not capture validation failures or error details as effectively as explicit logging.
  When valid: In environments with mature distributed tracing infrastructure and automatic context propagation across message queue boundaries.
- Log only at message producers rather than consumers (rejected)
  Rejected because: Producer-side logging cannot capture consumption failures, validation errors, or execution issues that occur after the message is enqueued.
  When valid: When message queue infrastructure provides guaranteed delivery and automatic dead-letter queue logging.
- Use structured logging libraries instead of console methods (deferred)
  When valid: When migrating to a structured logging solution that supports log levels, metadata fields, and integration with log aggregation platforms.

## Risks

- Logging sensitive user data or credentials contained in message payloads violates privacy and security requirements.
  Mitigation: Implement payload sanitization functions that redact sensitive fields before logging, and document which message types require sanitization.
  Owner: engineering team
- High-volume message queues may generate excessive log data, leading to storage costs and difficulty finding relevant logs.
  Mitigation: Implement log sampling for high-frequency operations and use structured logging to enable efficient filtering and aggregation.
  Owner: engineering team
- Inconsistent logging conventions across different message queue consumers make it difficult to build unified monitoring and alerting.
  Mitigation: Establish logging templates or wrapper functions that enforce consistent format, prefixes, and metadata inclusion across all message queue boundaries.
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Identify message queue consumption points by searching for cache retrieval operations followed by deserialization and action execution, then add logging before and after these operations.
- When logging validation failures, include both the validation error structure and a sanitized representation of the payload to enable debugging without exposing sensitive data.
- Consider wrapping message queue operations in a logging decorator or middleware that automatically captures operation metadata, timing, and outcomes to ensure consistent logging across all consumers.

## Continuation Context


Verify commands:
- Discover the project's test runner and execute the test suite covering message queue interaction modules to verify logging is present at consumption boundaries.
- Search the codebase for message queue consumption patterns and verify each has associated error logging with contextual information.
- Review log output from integration tests involving message queue workflows to confirm diagnostic context is sufficient for failure correlation.

Accept when:
- All message queue consumption points include logging statements that capture operation outcomes and error details.
- Test coverage demonstrates that validation failures and execution errors at message queue boundaries produce logs with sufficient diagnostic context.
- Code review confirms that sensitive data in message payloads is sanitized before logging.

## Enforcement

- Verified by: Code review checklist requiring logging at all new message queue consumption points.
- Verified by: Static analysis or linting rules that flag message queue operations without associated error handling and logging.
- Verified by: Integration test assertions that verify expected log entries are produced during message queue workflow execution.
- Violation handling: Pull requests introducing message queue consumers without logging are flagged during code review and require revision.
- Violation handling: Incidents involving message queue failures without diagnostic logs trigger retrospectives to add missing logging.
- Violation handling: Periodic audits of message queue interaction code identify gaps in logging coverage for remediation.
- Exception process: Exceptions may be granted for message queue operations with guaranteed delivery semantics and comprehensive infrastructure-level logging.
- Exception process: Exception requests must document the alternative observability mechanism and demonstrate equivalent diagnostic capability.
- Exception process: Approved exceptions are recorded in code comments with justification and review date.