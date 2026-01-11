# File Organization

## Naming
- One primary type per file
- Filename matches the primary type: `CaptureViewModel.swift`
- Extensions live in `TypeName+Topic.swift` (e.g., `URLRequest+Trace.swift`)

## File order
1. Imports
2. Primary type
3. Private helpers
4. Extensions

## Folder conventions
- `Views/`
- `ViewModels/`
- `Models/`
- `Clients/` or `Services/`
- `Support/` (helpers that are not domain models)

## Imports
- Prefer explicit imports
- Don’t import a large feature module if a smaller foundational module is enough
