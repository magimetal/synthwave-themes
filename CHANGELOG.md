# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

### Added
- Added a new VS Code target at `platforms/vscode` with a local-installable extension manifest (`package.json`) and theme file (`themes/magi-ui-synthwave-vscode-color-theme.json`).
- Added VS Code platform documentation at `platforms/vscode/README.md` and linked it from the workspace README.
- Added VS Code packaging ignore rules at `platforms/vscode/.vscodeignore`.
- Added a bundled local-install VS Code extension artifact at `platforms/vscode/magi-ui-synthwave-vscode-0.1.0.vsix`.
- Expanded `AGENTS.md` with VS Code platform paths, validation command, theme authoring rules, and current target inventory.

### Changed
- Rewrote the root `README.md` with a more personal and professional project narrative, design intent, and clearer multi-platform navigation.

## [2.1.0] - 2026-03-13

### Added
- Added a new Zed target at `platforms/zed` with a local-installable theme file: `themes/magi-ui-synthwave-zed.json`.
- Added Zed platform documentation at `platforms/zed/README.md` and linked it from the workspace README.
- Added a new OpenCode target at `platforms/opencode` with a local-installable theme file: `themes/magi-ui-synthwave-opencode.json`.
- Added OpenCode platform documentation at `platforms/opencode/README.md` and linked it from the workspace README.
- Added a new Ghostty target at `platforms/ghostty` with a local-installable theme file: `themes/magi-ui-synthwave-ghostty`.
- Added Ghostty platform documentation at `platforms/ghostty/README.md` and linked it from the workspace README.
- Expanded `AGENTS.md` with per-platform validation commands and authoring rules for Zed, OpenCode, and Ghostty themes.

## [2.0.0] - 2026-03-13

### Changed
- Modernized the JetBrains theme plugin to current architecture (Islands/New UI support, expanded UI token coverage, bundled color scheme registration, and Gradle-based plugin build scaffold).
- Restructured the repository into a multi-platform layout with JetBrains assets isolated under `platforms/jetbrains/`.
- Replaced the root README with workspace-level documentation and moved JetBrains-specific docs/screenshots into the JetBrains platform folder.

### Removed
- Removed legacy DevKit and generated artifacts from source control (`.iml`, prebuilt `.jar`, old `.icls`, tracked `.idea` project files, and unused theme background image).
