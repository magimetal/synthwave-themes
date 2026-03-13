# Magi UI Synthwave Themes

This repository is now organized as a **multi-platform theme workspace**.

## Structure

```text
.
├── LICENSE
├── README.md
└── platforms/
    ├── jetbrains/
    │   ├── README.md
    │   ├── resources/
    │   ├── build.gradle.kts
    │   ├── gradle.properties
    │   ├── settings.gradle.kts
    │   ├── gradlew
    │   ├── gradlew.bat
    │   └── gradle/
    ├── zed/
    │   ├── README.md
    │   └── themes/
    ├── opencode/
    │   ├── README.md
    │   └── themes/
    └── ghostty/
        ├── README.md
        └── themes/
```

## Available Targets

- `platforms/jetbrains` — IntelliJ Platform theme plugin (Magi UI Synthwave)
- `platforms/zed` — Zed editor theme JSON (Magi UI Synthwave Dark)
- `platforms/opencode` — OpenCode theme JSON (Magi UI Synthwave)
- `platforms/ghostty` — Ghostty terminal theme (Magi UI Synthwave)

## Why this layout

Each platform now gets its own isolated folder so we can add future targets (for example: VS Code, Neovim, Ghostty, etc.) without mixing build tools, assets, or docs.

If you want to work on JetBrains specifically, start here:

- [`platforms/jetbrains/README.md`](platforms/jetbrains/README.md)

If you want to work on Zed specifically, start here:

- [`platforms/zed/README.md`](platforms/zed/README.md)

If you want to work on OpenCode specifically, start here:

- [`platforms/opencode/README.md`](platforms/opencode/README.md)

If you want to work on Ghostty specifically, start here:

- [`platforms/ghostty/README.md`](platforms/ghostty/README.md)
