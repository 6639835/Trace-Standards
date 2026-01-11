# Documentation & Comments

## Doc comments (DocC / Quick Help)
Public API must include doc comments.

### Required for:
- `public` / `open` types and members
- Cross-module APIs that other teams rely on
- Complex or subtle behavior even if internal

### Doc comment style
- Start with a one-line summary ending with a period.
- Add detail only when it adds value: invariants, edge cases, performance, concurrency expectations.
- Use sections when relevant:
  - `- Parameters:`
  - `- Returns:`
  - `- Throws:`
  - Cancellation behavior for async methods

### Example
```swift
/// Connects to the stream and yields parsed events.
/// - Parameter url: The endpoint to connect to.
/// - Returns: An async sequence of events.
/// - Throws: If the connection fails or event decoding fails.
/// - Important: Cancelling the consuming task closes the connection.
func events(url: URL) -> AsyncThrowingStream<Event, Error>
````

## Inline comments

Inline comments should explain **why**, not **what**.

* Good: tradeoffs, non-obvious constraints, workaround explanations
* Bad: narrating obvious code

## File-level comments

Optional. Use them when:

* The file contains multiple related types
* The file’s purpose isn’t obvious from the name/types

## Tags

* `// TODO(owner|ticket): ...`
* `// FIXME(ticket): ...`

No commented-out code in main branches.

````

---

# File: `docs/swift/06-logging.md`

```markdown
# Logging Standard (Trace)

## Logging goals
- Reconstruct user-visible behavior and system decisions
- Debug issues without leaking secrets
- Make logs filterable by module + category
- Control noise: avoid log spam

## Logging API
Use unified logging via `OSLog.Logger` (not `print()`).

## Ownership and placement
### Where logger definitions live
- `TraceCore` provides a small wrapper/factory to standardize subsystem/category.
- Each module defines categories for its domain.

### Where log statements go
Log:
- Feature lifecycle boundaries (start/stop/connect/disconnect)
- Significant state transitions (loading → loaded → error, retry state changes)
- Network milestones (request start/finish, stream open/close)
- Errors that affect user-visible behavior or functional correctness

Do not log:
- Secrets (tokens, credentials, raw key material)
- Tight loops / per-frame UI updates
- High-volume per-message stream logs at info/warning/error levels (debug only + rate-limited)

## Subsystem and category
### Subsystem
Use the module bundle identifier when possible.
Fallback: `Bundle.main.bundleIdentifier + "." + ModuleName`

### Category naming (recommended)
Stable and filter-friendly:
- `Networking.HTTP`
- `Networking.SSE`
- `Networking.WebSocket`
- `Security.Keychain`
- `Security.Crypto`
- `UI.Navigation`
- `Feature.<Name>`
- `ViewModel.<Name>`

## Severity policy
Use levels consistently:

- `debug`: dev diagnostics; may be noisy but avoid stream spam unless rate-limited
- `info`: expected high-level milestones useful in production diagnosis
- `warning`: unexpected but recoverable; degraded behavior possible
- `error`: operation failed; user-visible behavior likely affected
- `fault` / `critical`: invariant broken, bug, corruption risk, security boundary issue; investigate immediately

Rule of thumb:
- Expected behavior: debug/info
- Ticket-worthy issue: warning/error
- “This should never happen”: fault/critical

## Privacy & sensitive data (mandatory)
- Treat user data as private by default.
- Mark interpolated values as `.private` unless explicitly safe.
- For identifiers you need to correlate without exposing, use hashed masking.

Examples:
- URL query parameters: private (hash mask or redact)
- User ID: private(hash)
- Error codes / enum cases: often public (if not sensitive)

## Message format (standard)
Console already shows timestamp/process/subsystem/category, so message content should be structured.

Required template:
`event=<domain.action> outcome=<success|failure> [duration_ms=] [id=] [reason=] [error=]`

Rules:
- `event` is mandatory and stable
- keys are `lower_snake_case`
- values are short; prefer codes/enums over long prose
- avoid embedding full JSON payloads

Examples:
- `event=networking.sse_connect outcome=success duration_ms=142`
- `event=feature.capture_start outcome=failure reason=permission_denied`

## Noise control
- Rate-limit repeated warnings/errors in reconnect loops
- For streams: per-message logs are debug-only and should be guarded by a debug flag

## Recommended implementation pattern
In `TraceCore`:
- `TraceLog.make(subsystem:category:) -> Logger`
In each module:
- define an enum of module loggers, one per category