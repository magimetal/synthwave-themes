# Magi UI Synthwave | Pi Theme

This package contains Pi implementation of Magi UI Synthwave theme.

## Files

- `themes/magi-ui-synthwave.json` — Pi theme JSON (`theme-schema.json`)

## Install

Global install:

```bash
mkdir -p ~/.pi/agent/themes
cp platforms/pi/themes/magi-ui-synthwave.json ~/.pi/agent/themes/
```

Optional project-local install:

```bash
mkdir -p .pi/themes
cp platforms/pi/themes/magi-ui-synthwave.json .pi/themes/
```

## Activation

Select theme in `/settings`, or set this in `settings.json`:

```json
{
  "theme": "magi-ui-synthwave"
}
```

## Notes

- Theme name is `magi-ui-synthwave`.
- Pi also discovers themes from project-local `.pi/themes/`.
- This target is file-based only (no build or packaging step required).
