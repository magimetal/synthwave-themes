# AGENTS.md

Guidance for coding agents operating in `magi-uI-synthwave`.

## 1) Repository Scope

- This is a **multi-platform theme workspace**.
- Currently implemented target: `platforms/jetbrains`.
- JetBrains target is an IntelliJ Platform theme plugin (resources-first project).
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

## 6) Theme-Specific Rules (JetBrains)

- Keep `plugin.xml`, `.theme.json`, and editor scheme XML aligned.
- If `pluginVersion` changes in `gradle.properties`, ensure release docs/changelog are updated.
- Keep compatibility declarations intentional (`since-build`, `until-build` behavior).
- Validate color-token references in `.theme.json` after edits.
- Preserve New UI / Islands compatibility unless explicitly asked to drop it.

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

- Active target: JetBrains only.
- Build tool: Gradle wrapper in `platforms/jetbrains`.
- Gradle wrapper distribution: `8.13`.
- No local Cursor/Copilot rule files in repository.
- No committed automated tests yet.
