# Magi UI Synthwave Themes

This repository is now organized as a **multi-platform theme workspace**.

## Structure

```text
.
├── LICENSE
├── README.md
└── platforms/
    └── jetbrains/
        ├── README.md
        ├── resources/
        ├── build.gradle.kts
        ├── gradle.properties
        ├── settings.gradle.kts
        ├── gradlew
        ├── gradlew.bat
        └── gradle/
```

## Available Targets

- `platforms/jetbrains` — IntelliJ Platform theme plugin (Magi UI Synthwave)

## Why this layout

Each platform now gets its own isolated folder so we can add future targets (for example: VS Code, Neovim, Ghostty, etc.) without mixing build tools, assets, or docs.

If you want to work on JetBrains specifically, start here:

- [`platforms/jetbrains/README.md`](platforms/jetbrains/README.md)
