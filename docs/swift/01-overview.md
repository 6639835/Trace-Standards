# Swift Standards Overview (Trace)

## Scope
These standards apply to all iOS app targets and first-party modules:
- Foundation: `TraceCore`, `TraceCompression`
- Infrastructure: `TraceSecurity`, `TraceNetworking`
- Feature: `TraceProxy`, `TraceFeatures`, `TraceUI`
- App: `Trace`, `TraceVPN`, `TraceWidget`

## Default stack
- Swift language mode: Swift 6
- UI: SwiftUI-first
- Architecture: MVVM
- Dependencies: first-party only unless explicitly approved
- Formatting: swift-format
- Linting: SwiftLint
- Testing: XCTest (Swift Testing allowed when we start migration)

## “Must” rules (summary)
- No force unwrap (`!`), no `try!`, no empty catches
- ViewModels are `@MainActor`
- Async work is cancellable and uses structured concurrency
- Public APIs are small and documented
- Log consistently and never log secrets
- Format + lint in CI
