# Magi UI Synthwave | Ghostty Theme

This package contains the Ghostty implementation of the Magi UI Synthwave theme.

## Files

- `themes/magi-ui-synthwave-ghostty` — Ghostty theme file (Ghostty config syntax)

## Install (user-wide)

```bash
mkdir -p ~/.config/ghostty/themes
cp platforms/ghostty/themes/magi-ui-synthwave-ghostty ~/.config/ghostty/themes/
```

Then enable it in your Ghostty config (`~/.config/ghostty/config`):

```ini
theme = magi-ui-synthwave-ghostty
```

## Notes

- This theme preserves the same palette used in the JetBrains, Zed, and OpenCode targets in this repository.
- Theme file uses Ghostty-native options (`background`, `foreground`, `cursor-*`, `selection-*`, `search-*`, and ANSI `palette` entries 0-15).
