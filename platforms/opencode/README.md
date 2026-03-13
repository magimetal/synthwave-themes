# Magi UI Synthwave | OpenCode Theme

This package contains the OpenCode implementation of the Magi UI Synthwave theme.

## Files

- `themes/magi-ui-synthwave-opencode.json` — OpenCode theme (`https://opencode.ai/theme.json`)

## Install (user-wide)

```bash
mkdir -p ~/.config/opencode/themes
cp platforms/opencode/themes/magi-ui-synthwave-opencode.json ~/.config/opencode/themes/
```

Then set this in `~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "theme": "magi-ui-synthwave-opencode"
}
```

## Notes

- Theme name is derived from filename (`magi-ui-synthwave-opencode.json` → `magi-ui-synthwave-opencode`).
- Colors are mapped from the JetBrains/Zed Magi Synthwave palette for cross-platform consistency.
