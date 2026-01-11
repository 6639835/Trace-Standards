# Error Handling Standard

## Principles
- Use typed errors in domain layers
- Don’t swallow errors
- Don’t push raw errors directly to UI
- Make recovery paths explicit (retry/backoff, user actions)

## Typed errors
Prefer:
- `enum FeatureError: Error { ... }`
- `enum NetworkingError: Error { ... }`

Avoid:
- throwing `Error` everywhere without structure

## UI error mapping
ViewModels should map errors to user-facing state:
- `UserFacingError { message, recoverySuggestion, debugCode? }`
- Keep debug detail behind a debug-only toggle if needed.

## Logging
- Log errors at `error` if user-visible behavior changes
- Log at `warning` if recoverable with retry/degraded mode
- Use `fault/critical` for invariants and security boundary issues

## “No empty catch” rule
Every catch must:
- handle recovery OR
- log + surface a user-facing state OR
- rethrow
