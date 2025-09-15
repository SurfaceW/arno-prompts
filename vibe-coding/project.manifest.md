# [Project Manifest]

> this is the entry file for all context engine.

[project basic guide]

e.g.

* Platforms: iOS (iPhone + iPad) and macOS
* Core Entities:
* Dot (card-like unit):
  * Visuals: background (image, gradient, texture), cover image
  * Content: rich text with Markdown, tags, status (DO | DONE | ARCHIVED), counter (trigger count)
  * Media & Links: external links, local attachments, video, audio
* Collection (Topic): groups Dots; manages ordering, randomization, and AI creation context
* Core Capabilities:
* CRUD: create, read, update, delete Dots and Collections
* Randomization: shuffle one or multiple Dots within a Collection; via shake gesture or button
* AI Creation: paste text or a sentence to auto-create Dots for a Collection
* Daily Widget: select a Collection to surface a random Dot daily on a widget; supports history/backtrack
* Data Portability: export, import, and restore all Dots/Collections
* Sync & Sharing: CloudKit multi-device sync; CoreData storage; CloudKit Share for cross-account shared Collections; collaborative editing on shared content
* Export & Social:
  * Single Dot export as text or image
  * Batch export Dots as text and as images
  * One-tap share of a Dot to social networks (e.g., X/Twitter)
* States & Counters:
* Dot statuses: DO, DONE, ARCHIVED
* Simple per-Dot counter to track trigger/execution count

[documents context rule]

documents with different design level marked with:

- l0: general human-first design docs, usually focus on the blue-print of the product, architecture, marketing strategies, etc.
- l1: semi-human design docs, focus on core-features, essential design details.
- l2: ai-first design docs, focus on the details of the implementation, code-gen, demo, etc.

[feature].l0.prd.md files for production feature specification
[tech].l1.tech.md files for production tech-stack specification
release.logs.md add release logs for iteration specific context

when you perform the iterations, you should update the related files marked with l0, l1, l2 to make the project more complete.

