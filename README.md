# Trace Standards

This repository defines the engineering standards for Trace iOS development (Swift 6, SwiftUI, MVVM, multi-module Trace* architecture, zero third-party by default).

## Goals
- Consistent, readable, maintainable Swift across modules
- Safe concurrency defaults (Swift 6 strict concurrency)
- Predictable logging and error handling
- Testable architecture (protocol-first DI)
- Enforced formatting + linting to prevent style drift

## What belongs here
- Standards and conventions (Markdown)
- Templates (SwiftLint, swift-format, PR template, codegen prompt)
- Architecture decisions (ADRs) for big changes

## How to use
1. Read `docs/INDEX.md` for the table of contents.
2. Copy templates from `templates/swift/` into the codebase.
3. Enforce rules in CI and PR review (see `docs/swift/11-code-review.md`).

## Ownership
- Tech Lead owns final decisions.
- Everyone can propose changes via PR + ADR when needed.

## Versioning
We track changes in `CHANGELOG.md`. Standards changes should be reviewed like code changes.
