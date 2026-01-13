# Swift Standards Overview (Trace)

## Scope
These standards apply to all iOS app/extension targets and SwiftPM modules in Trace:
- App targets: `Trace` (app), `TraceVPN` (packet tunnel extension), `TraceWidgetExtension` (WidgetKit + Live Activity)
- Foundation: `TraceCore`, `TraceCompression`
- Infrastructure: `TraceSecurity`, `TraceNetworking`
- Feature: `TraceProxy`, `TraceFeatures`, `TraceUI`
- Widget module: `TraceWidget` (SwiftPM target used by the extension)
- Tests: `TraceTests`, `TraceUITests` (when writing test code)

## Default stack
- Swift language mode: Swift 6 (swift-tools-version 6.0, language mode v6)
- Platform: iOS 26.2
- UI: SwiftUI-first
- Architecture: MVVM
- Dependencies: first-party only unless explicitly approved
- Formatting: SwiftFormat
- Linting: SwiftLint
- Testing: XCTest

## “Must” rules (summary)
- No force unwrap (`!`), no `try!`, no empty catches
- ViewModels are `@MainActor`
- Async work is cancellable and uses structured concurrency
- Public APIs are small and documented
- Log consistently and never log secrets
- Format + lint in CI
