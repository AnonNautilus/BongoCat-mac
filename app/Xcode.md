# BongoCat macOS — Xcode AI Context

This is **BongoCat-mac**, a native macOS application written in Swift that displays an animated cat overlay responding to keyboard and mouse input. 

---

## Project Structure

- **Entry points**: `Sources/BongoCat/main.swift`, `Sources/BongoCat/BongoCatApp.swift`
- **Core components**:
  - `Sources/BongoCat/CatView.swift` — Animation logic and SwiftUI rendering
  - `Sources/BongoCat/InputMonitor.swift` — Keyboard/mouse input detection and paw mapping
  - `Sources/BongoCat/OverlayWindow.swift` — Window management and positioning
- **Assets**: Cat sprites in `Sources/BongoCat/Resources/Images/`
- **Tests**: `Tests/BongoCatTests/` — unit and integration tests
- **Build scripts**: `Scripts/` directory (`build.sh`, `bump_version.sh`, `test.sh`, `package_app.sh`)
- **Config**: `Package.swift`, `Info.plist`
- **Private config (gitignored, not in repo)**: `analytics-config.plist`, `config.plist`, `.env`, `.env.local` — contain private API keys and analytics configuration

---

## Build System

- **Xcode project**: `BongoCat Overlay.xcodeproj` — open this directly
- **Swift Package Manager** — dependencies managed within the Xcode project itself
- **Fastlane** — automated distribution and screenshot pipeline (`fastlane/` directory)
---

## Architecture

- **Pattern**: MVVM — view models are kept separate from SwiftUI views
- **UI**: SwiftUI for UI components, AppKit (`NSWindow`, `NSScreen`) for window/overlay management
- **Frameworks in use**: `Cocoa`, `Foundation`, `AVFoundation`
- **Persistence**: `UserDefaults` for settings (position, scale, rotation, flip, stroke counter)
- **Target**: macOS 13.0+ (Ventura)

---

## Key Features

- **Per-app positioning**: Cat position is saved and restored per active application
- **Keyboard layout-based paw mapping**: Intelligent left/right paw assignment based on key position
- **Visual customisation**: Scaling, rotation, and flip options
- **Stroke counter**: Tracks keystrokes and mouse clicks
- **Menu integration**: Status bar menu and right-click context menu
- **Analytics**: Configuration loaded from gitignored `analytics-config.plist` — do not hardcode analytics keys

---

## Swift Coding Standards

- **Indentation**: 4 spaces (never tabs)
- **Access control**: Always specify `private`, `internal`, or `public` explicitly
- **State management**: use `@StateObject` for owned objects, `@ObservedObject` for passed objects
- Prefer **computed properties** over methods where appropriate
- Use **Result types or throwing functions** for error handling
- Keep the **main thread free** for UI updates; debounce rapid input events
- Use **lazy loading** for images and resources
- Respect **reduced motion** system preferences
- Ensure all UI elements have proper **accessibility labels**

---

## Component Responsibilities

| File | Responsibility |
|---|---|
| `InputMonitor.swift` | All input monitoring and paw mapping logic |
| `CatView.swift` | All animation logic and rendering |
| `OverlayWindow.swift` | All window management and positioning |
| `BongoCatApp.swift` | App lifecycle and entry coordination |

Do not mix responsibilities across these files.

---

## Testing

- Tests live in `Tests/BongoCatTests/`
  - `BongoCatTests.swift` — main app tests
  - `CatAnimationControllerTests.swift` — animation logic
  - `StrokeCounterTests.swift` — input tracking
- Run tests via `./Scripts/test.sh`
- Mock `CGEvent`, `UserDefaults`, `NSWindow`, and `NSScreen` for deterministic tests
- Use dependency injection to keep components testable
- Test naming convention: `test_featureName_whenCondition_shouldExpectedResult()`
- Follow **Arrange, Act, Assert** structure

---

## Version Management

- Follows **Semantic Versioning** (`MAJOR.MINOR.PATCH`), build number format `YYYY.MM`
- Version synchronised across `Info.plist`, `Package.swift`, and `CHANGELOG.md`
- Use `./Scripts/bump_version.sh [major|minor|patch]` to update, `./Scripts/check_version.sh` to verify
- Distribution packaged via `./Scripts/package_app.sh` (produces `.dmg`)

---

## Changelog

- Maintained in `CHANGELOG.md` following Keep a Changelog format
- Active changes go in the `[unreleased]` section
- Use emojis and bold keywords to match existing style (e.g. 🎯, 🐛, ⚡)
- Update after implementing features, fixing bugs, or refactoring major components

---

## AI Behaviour Instructions

- Always respect component responsibility boundaries — do not mix logic across files
- Prefer modifying existing files over creating new ones unless clearly warranted
- Always use 4-space indentation and explicit access control in Swift suggestions
- **Never suggest adding new dependencies without checking existing ones first**
- Always reference `BongoCat Overlay.xcodeproj` — there is no `.xcworkspace`
- **Never hardcode API keys or analytics config** — these are loaded from gitignored plist files
- When asked about missing config files (`analytics-config.plist`, `config.plist`, `.env`), explain they are intentionally gitignored and must be created locally from a template
- Suggest tests alongside any new feature or bug fix
- Keep suggestions lightweight and performant — this is a low-CPU overlay app running continuously in the background
