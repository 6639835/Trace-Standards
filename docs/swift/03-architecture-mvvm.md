# Architecture: SwiftUI + MVVM

## Responsibilities

### Views (SwiftUI)
Views are responsible for:
- Rendering UI from state
- Forwarding user intent (button taps, gestures) to the ViewModel
- Local ephemeral UI state (`@State`) only

Views must NOT:
- Perform networking
- Own business logic
- Parse responses
- Implement security/crypto logic

### ViewModels
ViewModels are responsible for:
- Owning feature state
- Orchestrating async workflows
- Mapping domain models to view state
- Validation and user-facing error mapping

ViewModels must:
- Be `@MainActor` (default)
- Avoid side effects in `init`
- Support cancellation of long-running tasks

### Services / Clients
Services/clients are responsible for:
- Side effects (network, storage, crypto, streaming connections)
- Being testable via protocols
- Returning typed data/errors

### Models
Models should:
- Prefer `struct`
- Conform to `Sendable` when used across tasks
- Use `Codable` where it improves clarity (not mandatory for everything)

## Suggested feature layout
- `FeatureNameView.swift`
- `FeatureNameViewModel.swift`
- `FeatureNameModels.swift`
- `FeatureNameClient.swift` (if feature owns a small client)
- Tests in `FeatureNameTests/`

## State management pattern
Recommended:
- `struct State`
- `enum ViewEvent` (optional)
- `func onAppear() async` / `func refresh() async` / explicit actions

Rules:
- No side effects in init
- Keep state transitions explicit
- Avoid hidden global state
