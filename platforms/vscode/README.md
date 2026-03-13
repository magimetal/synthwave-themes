# Magi UI Synthwave | VS Code Theme

This package contains the VS Code implementation of the Magi UI Synthwave theme.

## Files

- `package.json` — VS Code extension manifest
- `themes/magi-ui-synthwave-vscode-color-theme.json` — VS Code color theme
- `.vscodeignore` — packaging ignore rules

## Install (local)

Install the bundled extension package (run from `platforms/vscode`):

```bash
code --install-extension ./magi-ui-synthwave-vscode-0.1.0.vsix
```

Or install from the unpacked folder:

Copy this folder into your VS Code extensions directory:

```bash
mkdir -p ~/.vscode/extensions/magi-ui.magi-ui-synthwave-vscode
cp -R platforms/vscode/* ~/.vscode/extensions/magi-ui.magi-ui-synthwave-vscode/
```

Then restart VS Code and select:

- **Magi UI Synthwave**

## Notes

- This target is manifest + JSON only (no build toolchain is required).
- The palette stays aligned with the Magi UI Synthwave colors used in JetBrains, Zed, OpenCode, and Ghostty targets.
