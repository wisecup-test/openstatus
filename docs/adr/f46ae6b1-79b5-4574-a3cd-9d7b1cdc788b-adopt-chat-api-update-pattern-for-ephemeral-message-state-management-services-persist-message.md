# Adopt Chat API Update Pattern for Ephemeral Message State Management: Services Persist Message

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Interactive messaging platforms require in-place message updates to reflect state transitions without creating new messages, preserving conversation context and user experience.
- Ephemeral actions such as confirmations, cancellations, and error states must be communicated by mutating existing messages rather than appending new ones, reducing channel noise.
- Integration workflows coordinate asynchronous operations where initial messages serve as placeholders that are later updated with results, requiring a persistent reference to the original message location.
- The codebase demonstrates repeated use of chat update operations with channel and timestamp identifiers to modify message content and clear interactive blocks across error handling, cancellation, and expiration flows.

## Problem Statement

Services integrating with chat platforms need a consistent pattern for updating ephemeral message states across success, error, cancellation, and expiration scenarios without fragmenting conversation history or losing message context references.

## Decision

1. MUST: Services must persist message coordinates (channel identifier and timestamp) from initial message creation to enable subsequent updates throughout the interaction lifecycle.

## Policy Block

- MUST Services must persist message coordinates (channel identifier and timestamp) from initial message creation to enable subsequent updates throughout the interaction lifecycle.

In scope:
- All chat platform integrations that use interactive message components
- Asynchronous workflows that update users on operation progress or completion
- Error handling flows that communicate failures through existing messages
- Confirmation and cancellation interactions that modify message state

Out of scope:
- One-way notification messages that do not require updates
- Conversational flows where message history accumulation is desired
- Broadcast messages to multiple channels without interaction tracking

## Rationale

- The evidence shows consistent use of chat.update operations with channel and timestamp parameters across multiple state transitions (expiration, cancellation, error handling), indicating an established pattern for in-place message mutation.
- Clearing interactive blocks during terminal state transitions prevents users from interacting with stale actions while preserving the message history and context within the conversation thread.
- Coordinating database updates with message updates (as seen in the screenshot service updating incident records alongside message state) ensures consistency between persisted state and user-visible feedback.
- The pattern supports both synchronous error handling and asynchronous operation completion by maintaining stable message references throughout the interaction lifecycle.

## Consequences

Positive:
- Conversation threads remain clean and contextual, with state changes reflected in place rather than accumulating redundant messages.
- Users receive immediate visual feedback on action outcomes without losing the context of their original interaction.
- Message coordinates serve as stable references for asynchronous operations, enabling updates from background processes or delayed workflows.
- Error handling becomes more user-friendly by updating messages with descriptive error states rather than leaving interactions in ambiguous states.

Negative:
- Message update failures can leave interactions in inconsistent states if the update operation fails after state has been persisted elsewhere.
- Debugging becomes more difficult as message history does not show the progression of states, only the final updated message content.
- Rate limiting on chat API update operations may constrain high-frequency state transitions or bulk update scenarios.
- Requires careful management of message coordinate persistence and retrieval, adding complexity to interaction state management.

## Alternatives

- Post new messages for each state transition instead of updating existing messages (rejected)
  Rejected because: Creates conversation clutter and fragments interaction context, making it difficult for users to track the state of a single operation across multiple messages.
  When valid: Appropriate for audit trails or workflows where preserving the full history of state transitions is a compliance or debugging requirement.
- Use ephemeral messages that are only visible to the interacting user and automatically disappear (rejected)
  Rejected because: Ephemeral messages cannot be updated after initial posting and do not persist for asynchronous operation completion, limiting their use to immediate synchronous responses.
  When valid: Suitable for transient notifications or validation errors that do not require persistence or updates.
- Implement a polling mechanism where users refresh message state by re-invoking an action (rejected)
  Rejected because: Places burden on users to manually check operation status and does not provide proactive feedback on state changes, degrading user experience.
  When valid: May be necessary when chat platform API limitations prevent server-initiated message updates or when operating in highly restricted environments.

## Risks

- Message update API calls may fail due to network issues, rate limiting, or permission changes, leaving messages in stale states while backend state has progressed.
  Mitigation: Implement retry logic with exponential backoff for message updates, log update failures for monitoring, and consider fallback mechanisms such as posting new messages when updates fail after retries.
  Owner: engineering team
- Message coordinates (channel ID and timestamp) may become invalid if messages are deleted by users or administrators, causing update operations to fail silently or throw errors.
  Mitigation: Wrap message update calls in error handling that gracefully degrades when message references are invalid, and implement monitoring to detect patterns of update failures that may indicate deleted messages.
  Owner: engineering team
- Concurrent updates to the same message from multiple workflows or processes may result in race conditions where the final message state does not reflect the intended outcome.
  Mitigation: Design interaction flows to minimize concurrent updates to the same message, use optimistic locking or versioning if the chat platform supports it, and ensure update operations are idempotent where possible.
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
- Store message coordinates immediately after initial message posting in a structure that associates them with the interaction or operation identifier, enabling retrieval when updates are needed from asynchronous contexts.
- Establish a consistent error handling pattern that catches exceptions during action execution and translates them into user-friendly message updates with cleared interactive blocks.
- When coordinating database updates with message updates, consider using a saga pattern or compensating transactions to handle partial failures where one operation succeeds but the other fails.

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute integration tests that verify message update operations across error, cancellation, and expiration scenarios.
- Locate the project's static analysis or linting configuration and run checks that enforce error handling around message update API calls.
- Identify the project's dependency verification tooling and confirm that the chat platform client library version matches the lock artifact and that all used API methods are documented for that version.

Accept when:
- All message state transitions in interactive flows use the chat update API with channel and timestamp parameters, and interactive blocks are cleared in terminal states.
- Error handling wraps action execution and updates messages with descriptive error content rather than leaving messages in stale states.
- Message coordinates are persisted and retrievable for asynchronous update operations, and database state changes are coordinated with corresponding message updates.

## Enforcement

- Verified by: Code review checklist items that verify message update patterns in new integration code
- Verified by: Integration tests that assert message state transitions occur correctly across success and error paths
- Verified by: Static analysis rules that flag chat API usage without corresponding error handling or coordinate persistence
- Violation handling: Pull requests that introduce chat integrations without proper message update patterns are blocked until corrected
- Violation handling: Runtime monitoring alerts on message update failures that exceed threshold rates, triggering investigation
- Violation handling: Periodic architecture reviews audit integration code for compliance with message update patterns and coordinate management
- Exception process: Exceptions may be granted for read-only integrations or one-way notifications where message updates are not applicable
- Exception process: Exception requests must document the specific technical constraint that prevents adoption of the pattern and propose an alternative approach
- Exception process: Approved exceptions are time-bound and require re-evaluation when the underlying constraint changes or new platform capabilities become available