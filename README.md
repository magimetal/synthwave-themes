# Magi UI Synthwave Themes

Magi UI Synthwave is a multi-platform theme workspace built around one goal:
deliver the same visual identity across editors and terminals without losing
the personality that made the original theme feel special.

---

## Why this theme exists

A while back, I was using VS Code and fell in love with
[Synthwave-x-fluoromachine](https://github.com/webrender/synthwave-x-fluoromachine).
When I moved into JetBrains IDEs, I couldn't find a theme that felt close enough,
so I ported the vibe myself.

At first it was basically a 1:1 translation. Over time it turned into something
more intentional: cleaner palette choices, fewer near-duplicate colors, stronger
contrast in the right places, and language-specific tuning based on real daily use.

This repository is the continuation of that work — now expanded beyond JetBrains.

---

## Design intent

- Keep the synthwave mood without sacrificing readability.
- Preserve visual consistency across platforms.
- Prioritize practical coding ergonomics (search, errors, diffs, selection states).
- Evolve carefully instead of chasing constant redesigns.

---

## Available targets

- `platforms/jetbrains` — IntelliJ Platform theme plugin
- `platforms/zed` — Zed editor theme JSON
- `platforms/opencode` — OpenCode theme JSON
- `platforms/ghostty` — Ghostty terminal theme

Start with platform-specific docs:

- [JetBrains README](platforms/jetbrains/README.md)
- [Zed README](platforms/zed/README.md)
- [OpenCode README](platforms/opencode/README.md)
- [Ghostty README](platforms/ghostty/README.md)

---

## Workspace structure

```text
.
├── LICENSE
├── README.md
└── platforms/
    ├── jetbrains/
    ├── zed/
    ├── opencode/
    └── ghostty/
```

Each platform has its own implementation details, install instructions, and
theme artifacts, while sharing the same core palette direction.
