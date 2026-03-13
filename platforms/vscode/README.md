# Magi UI Synthwave | VS Code Theme

This package contains the VS Code implementation of the Magi UI Synthwave theme.

## Files

- `package.json` — VS Code extension manifest
- `themes/magi-ui-synthwave-vscode-color-theme.json` — VS Code color theme
- `glow/magi-ui-synthwave-glow.css` — optional neon glow stylesheet (Custom CSS and JS Loader)
- `.vscodeignore` — packaging ignore rules

## Install (local)

Install the bundled extension package (run from `platforms/vscode`):

```bash
code --install-extension ./magi-ui-synthwave-vscode-0.1.1.vsix
```

Or install from the unpacked folder:

Copy this folder into your VS Code extensions directory:

```bash
mkdir -p ~/.vscode/extensions/magi-ui.magi-ui-synthwave-vscode
cp -R platforms/vscode/* ~/.vscode/extensions/magi-ui.magi-ui-synthwave-vscode/
```

Then restart VS Code and select:

- **Magi UI Synthwave**

## Optional glow effects (Custom CSS and JS Loader)

VS Code themes cannot add real neon glow by themselves. If you want the Synthwave-style glow pass, use a custom CSS injector:

1. Install extension: **Custom CSS and JS Loader** (`be5invis.vscode-custom-css`)
2. Add this setting (use your absolute path):

```json
"vscode_custom_css.imports": [
  "file:///absolute/path/to/magi-ui-synthwave/platforms/vscode/glow/magi-ui-synthwave-glow.css"
]
```

3. Run command: **Enable Custom CSS and JS**
4. Restart VS Code

Notes:

- This glow layer is optional and separate from normal theme install.
- Token class mappings (`.mtk*`) are best-effort and can vary by language/grammar, so you may tune selector intensity locally.

## Notes

- This target is manifest + JSON only (no build toolchain is required).
- The palette stays aligned with the Magi UI Synthwave colors used in JetBrains, Zed, OpenCode, and Ghostty targets.
