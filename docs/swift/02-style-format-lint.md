# Style, Formatting, and Linting

## Naming
We follow Swift’s API design principles:
- Prefer clarity at the call site
- Use meaningful argument labels
- Avoid abbreviations unless universally understood
- Use nouns for types, verbs for methods

## Formatting (SwiftFormat)
- Formatting is not a debate: we format with `SwiftFormat`.
- The repo uses a `.swiftformat` file at the root (see `Trace/.swiftformat`).

Rules of thumb:
- Let the formatter decide whitespace/line breaks
- Keep lines readable; avoid deeply nested closures without extracting helpers

## Linting (SwiftLint)
We lint with SwiftLint for consistency and to prevent common mistakes.
- Add `.swiftlint.yml` to the repo root (see `Trace/.swiftlint.yml`)
- Start with warnings, tighten over time

Required practices:
- Avoid force unwrap/force try
- Avoid IUOs except when bridging legacy APIs and explicitly justified
- Prefer `private`/`fileprivate` where appropriate
- No dead code or unused imports

## “No bikeshedding” rule
If swift-format and SwiftLint agree, we follow them. If they conflict, we update configuration.
