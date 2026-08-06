# Adopt Ephemeral In-Memory Carrier Pattern for Asynchronous Action Coordination: Carrier Implementation Support

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all asynchronous action coordination patterns within the system.

## Context

- The system coordinates asynchronous user interactions across distributed service boundaries where immediate request-response cycles cannot complete the full workflow
- Action payloads must be temporarily stored with time-bounded validity to support multi-step confirmation and cancellation flows
- The architecture separates action initiation from action execution, requiring a coordination mechanism to bridge the temporal gap between user intent and system response
- Thread-based conversation contexts require bidirectional lookup between action identifiers and conversation thread identifiers to maintain user experience continuity

## Problem Statement

When coordinating asynchronous actions that span multiple request cycles and require user confirmation or cancellation, the system needs a mechanism to temporarily store action state, support bidirectional lookups between action and thread identifiers, enforce time-to-live constraints, and provide atomic consume operations that prevent duplicate execution while maintaining clear boundaries between action storage and business logic.

## Decision

1. SHOULD: The carrier implementation SHOULD support both persistent and in-memory backends to enable testing and development without external dependencies

## Policy Block

- SHOULD The carrier implementation SHOULD support both persistent and in-memory backends to enable testing and development without external dependencies

In scope:
- All asynchronous user interaction flows requiring confirmation or cancellation
- Multi-step workflows where action initiation and execution occur in separate request cycles
- Thread-based conversation contexts requiring action state correlation
- Temporary state storage with automatic expiration requirements

Out of scope:
- Long-term persistent storage of completed actions or audit logs
- Synchronous request-response workflows that complete within a single cycle
- Real-time streaming or pub-sub messaging patterns
- Durable task queues requiring guaranteed delivery and retry semantics

Exceptions:
- EX-001: Testing environments require deterministic action expiration behavior
- EX-002: Migration scenarios require temporary dual-write to both old and new carrier implementations

## Rationale

- The evidence shows three files implementing a consistent carrier pattern with put/get/consume operations, indicating a deliberate architectural choice to separate action coordination from business logic
- The presence of both memory-based and persistent implementations demonstrates a design for testability and deployment flexibility while maintaining a stable interface contract
- Atomic consume operations combined with TTL-based expiration prevent both duplicate execution and indefinite resource accumulation, addressing two critical failure modes in asynchronous coordination
- Bidirectional lookup between action and thread identifiers enables the system to support both user-initiated cancellations and system-initiated action expirations within the same conversation context

## Consequences

Positive:
- Clear separation of concerns between action storage and business logic enables independent testing and evolution of each component
- Automatic expiration of stale actions prevents resource leaks and eliminates the need for manual cleanup processes
- Atomic consume operations eliminate race conditions in distributed environments where multiple workers might process the same action
- Support for multiple backend implementations enables testing without external dependencies and production deployment with appropriate durability guarantees

Negative:
- Ephemeral storage with TTL constraints means actions may expire before users complete them, requiring user-facing error handling and retry mechanisms
- The carrier abstraction introduces an additional layer of indirection that must be understood and maintained by developers working on action workflows
- Bidirectional lookup requirements double the storage footprint for each action and increase the complexity of consistency maintenance during updates and deletions
- Schema validation at storage and retrieval boundaries adds runtime overhead and requires careful schema evolution to prevent breaking changes

## Alternatives

- Store action state directly in the conversation thread metadata provided by the external messaging platform (rejected)
  Rejected because: Platform-specific storage limits, lack of TTL support, and tight coupling to external service APIs would reduce portability and increase failure modes
  When valid: When the messaging platform provides robust metadata storage with TTL, atomic operations, and sufficient capacity guarantees
- Use a durable task queue with delayed execution for all asynchronous actions (rejected)
  Rejected because: Task queues optimize for guaranteed delivery and retry semantics, not for user-cancellable ephemeral state with bidirectional lookup requirements
  When valid: When actions must be guaranteed to execute even after system restarts and user cancellation is not a requirement
- Implement action coordination as stateless workflows using signed tokens containing all necessary state (rejected)
  Rejected because: Signed tokens cannot be revoked or updated after issuance, preventing cancellation and replace operations required by the evidence
  When valid: When actions are truly stateless, do not require cancellation, and token size limits are acceptable

## Risks

- Backend storage failures could cause action state loss, leading to orphaned conversations where users cannot complete or cancel pending actions
  Mitigation: Implement health checks for the storage backend, provide user-facing error messages when actions cannot be retrieved, and design conversation flows to gracefully handle missing action state
  Owner: Engineering team
- Clock skew or TTL misconfiguration could cause actions to expire too quickly or persist too long, degrading user experience or consuming excessive resources
  Mitigation: Validate TTL configuration in deployment pipelines, monitor action expiration rates, and alert on anomalies that indicate misconfiguration
  Owner: Engineering team
- Schema evolution of action payloads could break retrieval of actions stored before the schema change, causing validation failures for in-flight actions
  Mitigation: Implement schema versioning in stored payloads, maintain backward compatibility for at least one TTL period during schema changes, and provide migration paths for breaking changes
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- The carrier interface should define a minimal contract with put, get, consume, findByThread, and replace operations to enable multiple backend implementations without changing consumer code
- Action payload schemas should be defined using a validation library and co-located with the carrier implementation to ensure consistency between storage and retrieval validation
- Error handling for expired or missing actions should provide user-facing messages that explain the expiration and offer clear next steps rather than exposing internal error details

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the validation library used for schema definition, then verify all action payload schemas are defined and exported
- Locate the carrier interface definition and confirm it declares put, get, consume, findByThread, and replace operations with appropriate type signatures
- Find the test suite for the carrier implementations and verify both memory and persistent backends pass the same interface contract tests

Accept when:
- All action payload schemas validate successfully against stored and retrieved action state without runtime type errors
- Carrier interface tests pass for both memory and persistent backend implementations with identical behavior
- Action expiration monitoring shows TTL configuration prevents both premature expiration and excessive resource accumulation

## Enforcement

- Verified by: Code review verifying all asynchronous action coordination uses the carrier abstraction rather than direct storage access
- Verified by: Static analysis confirming action payload schemas are validated at storage and retrieval boundaries
- Verified by: Integration tests verifying atomic consume operations prevent duplicate execution under concurrent access
- Violation handling: Direct storage access bypassing the carrier abstraction is flagged in code review and must be refactored before merge
- Violation handling: Missing schema validation at storage or retrieval boundaries triggers test failures and blocks deployment
- Violation handling: Race conditions in action processing detected by integration tests require implementation of atomic consume operations
- Exception process: Request exception through architecture review with justification for why the carrier pattern cannot be applied
- Exception process: Provide alternative mechanism that addresses action expiration, duplicate prevention, and bidirectional lookup requirements
- Exception process: Document the exception in the codebase with rationale and obtain approval from engineering team lead