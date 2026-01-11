# Networking & Streaming (HTTP, SSE, WebSocket)

## General networking rules
- Keep networking in `TraceNetworking`
- Prefer typed request/response models
- Avoid leaking transport details into ViewModels
- Never log secrets or raw sensitive payloads

## HTTP
Recommended shape:
- Endpoint/request types as value types
- Decode into typed responses
- Centralize request building (headers, auth) inside networking module

Log milestones:
- request start (info/debug)
- request success with duration (info)
- request failure with reason (warning/error)

## Streaming: SSE/WebSocket
### API shape
Expose streams as:
- `AsyncSequence` / `AsyncThrowingStream<Event, Error>`

### Required documentation
Document:
- When it completes
- Cancellation behavior (what cancels, how to close connection)
- Reconnect behavior (yes/no, backoff, max attempts)
- Threading expectations (usually not main; ViewModel publishes on main)

### Logging
Info-level:
- connect/disconnect
- reconnect scheduled
- final exhaustion/failure
Debug-level:
- event counts / summary info
Per-message logs:
- debug-only + rate-limited (off by default in production)

### Backoff
If reconnect exists, standardize:
- exponential backoff with jitter
- max retries or max total time
- emit a state transition visible to UI if it impacts functionality
