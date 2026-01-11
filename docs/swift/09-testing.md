# Testing Standard

## Default framework
- XCTest is the default (existing codebase standard).
- Swift Testing can be adopted when we explicitly start migration.

## What we test
- ViewModels: state transitions, error mapping, cancellation
- Clients: decoding/parsing, retry/backoff policies
- Security boundaries: keychain wrappers, crypto boundaries (use test vectors if applicable)

## Test structure guidelines
- Prefer given/when/then naming and arrangement
- Keep tests focused and fast
- Use mocks/fakes via protocol-based DI

## Coverage expectations
- Non-trivial logic requires tests
- Bug fixes should come with regression tests when feasible

## SwiftUI views
- Prefer ViewModel tests over heavy UI tests
- Add UI tests only for critical end-to-end flows when justified
