# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

- No changes yet.

## [2.0.0] - 2026-03-13

### Changed
- Modernized the JetBrains theme plugin to current architecture (Islands/New UI support, expanded UI token coverage, bundled color scheme registration, and Gradle-based plugin build scaffold).
- Restructured the repository into a multi-platform layout with JetBrains assets isolated under `platforms/jetbrains/`.
- Replaced the root README with workspace-level documentation and moved JetBrains-specific docs/screenshots into the JetBrains platform folder.

### Removed
- Removed legacy DevKit and generated artifacts from source control (`.iml`, prebuilt `.jar`, old `.icls`, tracked `.idea` project files, and unused theme background image).
