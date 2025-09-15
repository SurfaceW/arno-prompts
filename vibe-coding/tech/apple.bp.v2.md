Title: Create a SwiftUI multi-platform app (iOS, iPadOS, macOS) that compiles and passes xcodebuild

Goal
Build a Swift 6, SwiftUI app using SwiftData (or Core Data where noted) that runs natively on iPhone, iPad, and Mac with adaptive layouts and Apple-native UX.

Project constraints

* Must compile with no warnings or errors and pass:
  xcodebuild -project e-studio-dots.xcodeproj -scheme e-studio-dots -destination 'platform=iOS Simulator,name=iPhone 16 Pro' build
* Apple frameworks first, reduce dependency on third-party libraries.
* Follow Apple HIG: platform-appropriate navigation (tab/stack on iPhone, split view on iPad, sidebar + toolbar + commands on macOS), system typography and colors, SF Symbols.
* Dynamic Type, VoiceOver labels, focus/keyboard shortcuts, undo/redo. Prefer .foregroundStyle(.secondary) and system fonts (.body, .caption, etc.).
* Use previews with @Previewable for each view/state configuration.
* Keep files small, reusable, and composable. Avoid over-complex types. less than 1K line for each file.
* Internationalization: use String(localized:) / LocalizedStringKey with Localizable.strings and stringsdict; no hard-coded user-visible strings.
* Brand color: add an asset named “Ascend” (light/dark variants). Use it via .tint(Color("Ascend")) where appropriate.
* Haptics: add subtle feedback for primary actions using UIFeedbackGenerator on iOS/iPadOS; no haptics on macOS (guard with #if os(iOS) || os(tvOS)).
* Data: prefer SwiftData for local persistence. Optional: add Core Data + CloudKit sharing stub (separate module or feature flag) when demonstrating shared editing.

App concept (simple but complete)

[add your app context here]

e.g.

* Entity: Dot (id, title, detail, createdAt, isStarred).
* Features: list/grid of Dots; add/edit/delete; star/unstar; sort/filter/search; multi-selection; drag & drop reorder (where applicable).
* Platform adaptations:
  * iPhone: NavigationStack + toolbar; sheet-based add/edit.
  * iPad: NavigationSplitView with sidebar list; multi-column and multiwindow support.
  * macOS: Sidebar list + detail; menu commands (New, Duplicate, Delete, Toggle Star), toolbar with SF Symbols; keyboard shortcuts; simple menu bar extra showing starred count.
* Share/export: system ShareLink for a Dot’s text; open in new window on iPad/macOS.
* State restoration: restore last selection on supported platforms.

Repository layout (create these top-level directories)

* Model
* Views
* Tests
* Resources

Output formatting

* Start with the final file tree.
* Then, for each file, provide the path line followed by its complete contents.
* Keep code blocks fenced as Swift where appropriate and ensure imports/availability conditions match each platform target.
* End with a short “Build & Run” section repeating the exact xcodebuild command and any scheme/target notes.

Notes

* If you include the optional Core Data + CloudKit sharing demo, keep it isolated and off by default; the main app must still compile cleanly without CloudKit entitlements.
* Prefer smaller view files over large monoliths; extract modifiers into view extensions where helpful.
