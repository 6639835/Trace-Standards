# Code Review Standard

## PR checklist (required)
- [ ] swift-format applied / passes formatting check
- [ ] SwiftLint passes (or warnings justified)
- [ ] Tests added/updated for non-trivial logic
- [ ] Public APIs documented (doc comments)
- [ ] Concurrency/cancellation considered (no leaks, safe actor usage)
- [ ] Module boundaries respected (no forbidden imports)
- [ ] Logging follows standard (levels, privacy, format)
- [ ] User-facing strings localized + accessibility labels where needed

## Review focus areas
- Correctness and safety (concurrency, cancellation)
- API clarity and naming
- Separation of responsibilities (View vs ViewModel vs Client)
- Error handling and user-facing messaging
- Logging privacy (no secrets)

## “Blocker” conditions
- Force unwrap/try without strong justification
- Empty catches or swallowed errors
- Side effects in View bodies or View init
- Logging secrets or sensitive payloads
- Introducing dependency cycles
