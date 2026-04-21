# AGENTS.md

Guidance for coding agents operating in `magi-uI-synthwave`.

## 1) Repository Scope

- This is a **multi-platform theme workspace**.
- Currently implemented targets: `platforms/jetbrains`, `platforms/zed`, `platforms/opencode`, `platforms/ghostty`, `platforms/vscode`, `platforms/slack`, and `platforms/pi`.
- JetBrains target is an IntelliJ Platform theme plugin (resources-first project).
- Zed target is a file-based theme JSON package (no build toolchain required).
- OpenCode target is a file-based theme JSON package (no build toolchain required).
- Ghostty target is a file-based theme config package (no build toolchain required).
- VS Code target is a manifest + theme JSON package (no build toolchain required).
- Slack target is a plain-text import-string package (no build toolchain required).
- Pi target is a file-based TUI theme JSON package (no build toolchain required).
- There is currently no application runtime code (no Kotlin/Java source trees).

## 2) Rule Sources Discovered

I checked for external agent-rule files and found none in this repo:

- `.cursor/rules/` → not present
- `.cursorrules` → not present
- `.github/copilot-instructions.md` → not present

If any of these files are added later, treat them as higher-priority local guidance.

## 3) Important Paths

- Root workspace README: `README.md`
- Changelog: `CHANGELOG.md`
- JetBrains platform root: `platforms/jetbrains/`
- JetBrains Gradle build file: `platforms/jetbrains/build.gradle.kts`
- JetBrains Gradle properties: `platforms/jetbrains/gradle.properties`
- JetBrains plugin descriptor: `platforms/jetbrains/resources/META-INF/plugin.xml`
- JetBrains theme JSON: `platforms/jetbrains/resources/theme/magi-ui-synthwave.theme.json`
- JetBrains editor scheme XML: `platforms/jetbrains/resources/magi-ui-synthwave.xml`
- Zed platform root: `platforms/zed/`
- Zed platform README: `platforms/zed/README.md`
- Zed theme JSON: `platforms/zed/themes/magi-ui-synthwave-zed.json`
- OpenCode platform root: `platforms/opencode/`
- OpenCode platform README: `platforms/opencode/README.md`
- OpenCode theme JSON: `platforms/opencode/themes/magi-ui-synthwave-opencode.json`
- Ghostty platform root: `platforms/ghostty/`
- Ghostty platform README: `platforms/ghostty/README.md`
- Ghostty theme file: `platforms/ghostty/themes/magi-ui-synthwave-ghostty`
- VS Code platform root: `platforms/vscode/`
- VS Code platform README: `platforms/vscode/README.md`
- VS Code extension manifest: `platforms/vscode/package.json`
- VS Code theme JSON: `platforms/vscode/themes/magi-ui-synthwave-vscode-color-theme.json`
- Slack platform root: `platforms/slack/`
- Slack platform README: `platforms/slack/README.md`
- Slack theme import strings: `platforms/slack/themes/magi-ui-synthwave-slack.txt`
- Pi platform root: `platforms/pi/`
- Pi platform README: `platforms/pi/README.md`
- Pi theme JSON: `platforms/pi/themes/magi-ui-synthwave.json`

## 4) Build / Lint / Test Commands

Run all commands from repo root unless noted.

### JetBrains: core Gradle commands

```bash
./platforms/jetbrains/gradlew -p platforms/jetbrains help
./platforms/jetbrains/gradlew -p platforms/jetbrains clean
./platforms/jetbrains/gradlew -p platforms/jetbrains buildPlugin
./platforms/jetbrains/gradlew -p platforms/jetbrains verifyPlugin
./platforms/jetbrains/gradlew -p platforms/jetbrains check
```

Notes:

- `buildPlugin` is the key packaging command.
- Build artifact is expected under: `platforms/jetbrains/build/distributions/`.
- `verifyPlugin` is configured in `build.gradle.kts` with recommended IDEs.

### Running tests

There are currently no committed test files in this repo.

When tests are added, use:

```bash
./platforms/jetbrains/gradlew -p platforms/jetbrains test
```

Single test class:

```bash
./platforms/jetbrains/gradlew -p platforms/jetbrains test --tests "com.example.MyThemeTest"
```

Single test method:

```bash
./platforms/jetbrains/gradlew -p platforms/jetbrains test --tests "com.example.MyThemeTest.rendersExpectedPalette"
```

### Lint / validation status

- No dedicated linter task (e.g., ktlint/detekt/eslint) is configured right now.
- Treat `check` plus static file validation as minimum quality gate.

### Zed: validation commands (file-based)

No build/test task is configured for Zed themes. Use JSON parse validation:

```bash
python3 -c "import json; json.load(open('platforms/zed/themes/magi-ui-synthwave-zed.json')); print('Zed JSON validation passed')"
```

### OpenCode: validation commands (file-based)

No build/test task is configured for OpenCode themes. Use JSON parse validation:

```bash
python3 -c "import json; json.load(open('platforms/opencode/themes/magi-ui-synthwave-opencode.json')); print('OpenCode JSON validation passed')"
```

### Ghostty: validation commands (file-based)

No build/test task is configured for Ghostty themes. Use key-presence validation:

```bash
python3 -c "from pathlib import Path; p=Path('platforms/ghostty/themes/magi-ui-synthwave-ghostty'); t=p.read_text(); required=['background =','foreground =','cursor-color =','selection-background =','palette = 0=','palette = 15=']; missing=[k for k in required if k not in t]; print('Ghostty theme validation passed' if not missing else f'Missing keys: {missing}')"
```

### VS Code: validation commands (file-based)

No build/test task is configured for VS Code themes. Use JSON parse validation:

```bash
python3 -c "import json; json.load(open('platforms/vscode/package.json')); json.load(open('platforms/vscode/themes/magi-ui-synthwave-vscode-color-theme.json')); print('VS Code JSON validation passed')"
```

### Slack: validation commands (file-based)

No build/test task is configured for Slack themes. Use key-string presence validation:

```bash
python3 -c "from pathlib import Path; p=Path('platforms/slack/themes/magi-ui-synthwave-slack.txt'); t=p.read_text(); required=['#13051f, #200933, #d21e85, #9963ff']; missing=[k for k in required if k not in t]; print('Slack theme validation passed' if not missing else f'Missing strings: {missing}')"
```

### Pi: validation commands (file-based)

No build/test task is configured for Pi themes. Use JSON semantic validation:

```bash
python3 - <<'PY'
import json
from pathlib import Path
p = Path('platforms/pi/themes/magi-ui-synthwave.json')
d = json.loads(p.read_text())
assert d['name'] == 'magi-ui-synthwave'
assert d['$schema'] == 'https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json'
assert len(d['colors']) == 51
assert set(d.get('export', {}).keys()) == {'pageBg', 'cardBg', 'infoBg'}
text = p.read_text()
assert text.endswith('\n')
assert '\t' not in text
print('Pi theme validation passed')
PY
```

Recommended static validation command for theme assets:

```bash
python3 -c "import json,xml.etree.ElementTree as ET; json.load(open('platforms/jetbrains/resources/theme/magi-ui-synthwave.theme.json')); ET.parse('platforms/jetbrains/resources/META-INF/plugin.xml'); ET.parse('platforms/jetbrains/resources/magi-ui-synthwave.xml'); print('JSON/XML validation passed')"
```

## 5) Coding & Editing Standards

Even though this repo is currently config/resource heavy, apply the following standards.

### 5.1 Imports

- Keep imports minimal and explicit.
- Do not use wildcard imports unless framework tooling requires it.
- Remove unused imports in Gradle Kotlin DSL files.

### 5.2 Formatting

- Preserve existing formatting style in touched files.
- Kotlin DSL (`*.kts`): 4-space indentation.
- JSON (`*.json`): 2-space indentation.
- XML (`*.xml`): 2-space indentation and consistent attribute wrapping.
- Keep one trailing newline at end of text files.

### 5.3 Types and strictness

- Prefer explicit, type-safe configuration in Kotlin DSL.
- Avoid loose or ambiguous values when typed APIs exist.
- Do not introduce suppression-style shortcuts to hide errors.

### 5.4 Naming conventions

- New directories/files: lowercase kebab-case when practical.
- Keep plugin/theme IDs stable unless a migration requires a coordinated rename.
- Color tokens in theme JSON should stay consistent and descriptive.
- Do not introduce near-duplicate color token names without clear reason.

### 5.5 Error handling

- Fail fast in scripts/commands and surface actionable errors.
- Do not silently swallow build or validation failures.
- If verification cannot run (e.g., missing Java), state it explicitly.

### 5.6 Scope control

- Make minimal diffs that directly solve the requested task.
- Avoid drive-by refactors in unrelated platform folders.
- Do not modify binary assets (images/jars) unless required by the task.

## 6) Theme-Specific Rules

### 6.1 JetBrains

- Keep `plugin.xml`, `.theme.json`, and editor scheme XML aligned.
- If `pluginVersion` changes in `gradle.properties`, ensure release docs/changelog are updated.
- Keep compatibility declarations intentional (`since-build`, `until-build` behavior).
- Validate color-token references in `.theme.json` after edits.
- Preserve New UI / Islands compatibility unless explicitly asked to drop it.

### 6.2 Zed

- Keep Zed theme files under `platforms/zed/themes/`.
- Use Zed schema header in theme JSON (`https://zed.dev/schema/themes/v0.2.0.json`).
- Keep hex colors alpha-explicit (`#RRGGBBAA`) for consistency.
- Preserve parity with core Magi palette unless intentionally diverging.
- Validate JSON parse after every change.

### 6.3 OpenCode

- Keep OpenCode theme files under `platforms/opencode/themes/`.
- Use OpenCode schema header in theme JSON (`https://opencode.ai/theme.json`).
- Keep hex colors in 6-digit format (`#RRGGBB`) unless intentionally using ANSI or `none`.
- Do not add undocumented theme keys; OpenCode schema sets `additionalProperties: false`.
- Validate JSON parse after every change.

### 6.4 Ghostty

- Keep Ghostty theme files under `platforms/ghostty/themes/`.
- Use Ghostty config syntax (`key = value`), not JSON.
- Keep hex colors in 6-digit format (`#RRGGBB`) for consistency.
- Provide ANSI palette entries at least for indices `0-15`.
- Validate required keys (`background`, `foreground`, `cursor`, `selection`, palette entries) after every change.

### 6.5 VS Code

- Keep VS Code theme assets under `platforms/vscode/`.
- Keep extension metadata in `platforms/vscode/package.json` and theme JSON under `platforms/vscode/themes/`.
- Use `uiTheme: "vs-dark"` for the dark theme target unless intentionally introducing a separate light variant.
- Keep color values aligned with the shared Magi palette unless divergence is intentional and documented.
- Validate `package.json` and theme JSON parse successfully after every change.

### 6.6 Slack

- Keep Slack theme assets under `platforms/slack/`.
- Keep import strings in plain text for easy copy/paste.
- Preserve the current 4-color import string: `#13051f, #200933, #d21e85, #9963ff`.
- If legacy compatibility guidance is present, keep the 8-color format string documented alongside the current string.
- Validate required import string presence after every change.

### 6.7 Pi

- Keep Pi theme assets under `platforms/pi/themes/`.
- Use Pi theme JSON schema header (`https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json`).
- Keep JSON formatting at 2-space indentation with one trailing newline.
- Preserve theme name `magi-ui-synthwave`, all 51 required `colors` tokens, and exact `export` keys `pageBg`, `cardBg`, and `infoBg`.
- Keep Pi target file-based only; do not add build scripts, packaging metadata, or undocumented discovery paths unless task explicitly requires it.
- Validate JSON parse and Pi-specific schema/name/token/export requirements after every change.

## 7) Documentation Rules

- Update `CHANGELOG.md` for user-visible behavior changes.
- Keep root `README.md` focused on workspace structure.
- Keep platform-specific docs inside each platform folder (e.g., `platforms/jetbrains/README.md`).
- When adding a new platform, add it to root README "Available Targets".

## 8) Git & PR Hygiene

- Never commit secrets or credentials.
- Group related file moves/edits into coherent commits.
- Use clear commit messages describing intent.
- Run at least one verification step before claiming success.
- If commands cannot run locally, report exactly what is blocked and why.

## 9) Agent Checklist Before Final Response

1. Confirm files changed are in the correct platform folder.
2. Run/attempt the relevant Gradle command(s).
3. Validate JSON/XML syntax for theme assets.
4. Update docs/changelog when behavior or structure changes.
5. Report results with: what changed, where, and verification output.

## 10) Current Reality Check (as of this file)

- Active targets: JetBrains + Zed + OpenCode + Ghostty + VS Code + Slack + Pi.
- JetBrains build tool: Gradle wrapper in `platforms/jetbrains`.
- JetBrains Gradle wrapper distribution: `8.13`.
- Zed target is file-only (no build/test runner configured).
- OpenCode target is file-only (no build/test runner configured).
- Ghostty target is file-only (no build/test runner configured).
- VS Code target is file-only (no build/test runner configured).
- Slack target is file-only (no build/test runner configured).
- Pi target is file-only (no build/test runner configured).
- No local Cursor/Copilot rule files in repository.
- No committed automated tests yet.
