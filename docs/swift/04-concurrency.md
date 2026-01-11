# Concurrency (Swift 6)

## Principles
- Prefer structured concurrency (`async/await`, `TaskGroup`)
- Avoid GCD unless bridging legacy callbacks
- Make cancellation a first-class behavior

## ViewModels are @MainActor
- ViewModels run on the main actor to keep UI state updates safe.
- Heavy work belongs in services/clients or background tasks.

## Cancellation rules
Any long-running async operation must:
- Be cancellable (check `Task.isCancelled` / handle cancellation errors)
- Cancel previous tasks before starting a new one when appropriate
- Clean up streaming tasks on stop/deinit

## Shared mutable state
- If state is shared across tasks, isolate it in an `actor`.
- Avoid locks unless absolutely necessary.

## AsyncSequence for streams
SSE/WebSocket and similar “push” sources should expose:
- `AsyncSequence` / `AsyncThrowingStream`

Document:
- Completion conditions
- Cancellation behavior
- Retry/backoff behavior
