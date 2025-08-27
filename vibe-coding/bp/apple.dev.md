This is a project for **Apple Cross-Platform Development** use *Swift* / *SwiftUI* and *SwiftData* and related Apple powered technologies, support Mac, iOS, iPad with different screens

This project is an experimental project for iOS26 beta, MacOSX26 beta, ...
When you use build command, the target device name is iPhone 16 Pro

## Rules

* rule xcode build to test your modifications to pass build

## Dir

* `Model` the data model for the app
* `Views` the Swift UI for the app
* `Tests` the tests for the app
* `Resources` other app related resources

## Tech Guide

* MUST: consider the compatibility of the app for both iOS, iPadOS and macOS, especially the UI and user experience.
* MUST: **strictly follow Apple's Design Guideline and Human Interface Guideline of Apple Platforms**
* use `SF Symbols` to boost its full abilities
* use the latest OS version for the app (swift 6, iOS latest version ...)
* when use `SwiftUI` to build UI, use `@Previewable` to preview the UI in the preview mode for easier development.
* consider write View with `SwiftData` and composite view in a more reusable way.
  * [optional] for sharing editing data, use `CoreData` first, with iCloud kit / share kit and share the data between devices, accounts for co-editing experience.
* control the code complexity, don't write too complex code and try to separate the code into smaller files.
* use system preset dynamic variables for better user experience, e.g. font size and font color use the system preset ones like .caption to avoid fixed size, and color like `.foregroundStyle(.secondary)` to avoid fixed color and auto adapt to light/dark mode.
* follow the i18n rule of this project use Apple's standard 

## Others

The build command is:
xcodebuild -project e-studio-dots.xcodeproj -scheme e-studio-dots -destination 'platform=iOS Simulator,name=iPhone 16' build
