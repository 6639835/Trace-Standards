# Logging Standard

## Principles
- Logging is for observability, not debugging-by-default
- Never log secrets or raw sensitive payloads
- Logs must be structured, consistent, and searchable
- Avoid high-volume logs in production

## Approved logger
- Use `Logger` (Unified Logging / os.log) for system logging
- Do not use `print` or ad-hoc console logging in production code

## Levels (intent)
- `debug`: developer-only detail, disabled in production by default
- `info`: lifecycle milestones (start/stop, connect/disconnect, success)
- `warning`: recoverable failure, degraded behavior
- `error`: user-visible failure or dropped critical path
- `fault`: invariants, security boundaries, data corruption

## Structure & fields
Each log should include:
- Subsystem and category
- A short, stable event name
- Key fields (ids, durations, counts)

Prefer:
- `event=network.request.start id=... durationMs=...`
- `event=stream.reconnect.scheduled backoffMs=... attempt=...`

Avoid:
- full payload dumps
- unbounded arrays or blobs

## Privacy
- Use private redaction for anything user-related
- Use hashes or counts instead of raw identifiers
- If a field must be logged, document why it is safe

## Rate limiting
- Any per-item logs must be rate-limited or debug-only
- For streaming, log summaries (counts, durations) instead of per-event

## Error logging
- Log the error type and a stable code where possible
- Do not log raw response bodies or auth headers
- Include recovery path context when available (retry/backoff)

## Debug-only logs
- Noisy logs must be wrapped in `#if DEBUG`
- Never change program behavior based on logging side effects
