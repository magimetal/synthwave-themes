# Plan: add Pi theme target

## Objective
Add Pi theme target into workspace using existing multi-platform conventions. Scope limited to new `platforms/pi` target plus required repo documentation and validation guidance.

## Inputs / Evidence
- Source theme exists: `/Users/magimetal/.pi/agent/themes/magi-ui-synthwave.json`
- Pi theme docs: `/Users/magimetal/.nvm/versions/node/v24.14.0/lib/node_modules/@mariozechner/pi-coding-agent/docs/themes.md`
- Recommended repo target path: `platforms/pi/themes/magi-ui-synthwave.json`
- Workspace docs in scope: `README.md`, `CHANGELOG.md`, `AGENTS.md`
- New platform doc expected: `platforms/pi/README.md`
- Repo currently lists active targets: JetBrains, Zed, OpenCode, Ghostty, VS Code, Slack

## Assumptions
- Pi target remains file-based only. No build toolchain, packaging step, or runtime code added.
- Theme content should preserve source JSON semantics while being reformatted to repo-standard JSON style: 2-space indentation and trailing newline.
- Pi install/activation guidance should follow Pi docs exactly for this repo scope: global `~/.pi/agent/themes/`, optional project-local `.pi/themes/`, activation via `/settings` or `settings.json` using theme name `magi-ui-synthwave`.
- No PRD or ADR required for this addition.

## Verification strategy
- Pi target uses file validation only. No JetBrains Gradle command is relevant verification evidence for this work.
- Artifact verification must prove all of: JSON parse success, semantic parity with source theme, exact schema URL, exact theme name, all 51 required color tokens, exact `export` keys, repo-standard formatting.
- Documentation verification must prove exact install paths, optional project-local path, activation method, and theme name wording.

## Dependencies / Sequence
1. Add Pi platform artifact first.
2. Add Pi platform README second so root docs can link to real file.
3. Update workspace docs after platform files exist.
4. Update `AGENTS.md` last so repo guidance matches final file paths and validation rules.
5. Run file-based verification only.

## Task 1 — Add Pi platform artifact
### What
Create new platform folder structure and add theme file at `platforms/pi/themes/magi-ui-synthwave.json` using `/Users/magimetal/.pi/agent/themes/magi-ui-synthwave.json` as source while preserving parsed JSON semantics and reformatting to repo-standard JSON style.

### References
- Source: `/Users/magimetal/.pi/agent/themes/magi-ui-synthwave.json`
- Pi docs: `/Users/magimetal/.nvm/versions/node/v24.14.0/lib/node_modules/@mariozechner/pi-coding-agent/docs/themes.md`
- Target dir: `platforms/pi/themes/`
- Target file: `platforms/pi/themes/magi-ui-synthwave.json`

### Acceptance criteria
- `platforms/pi/` exists.
- `platforms/pi/themes/magi-ui-synthwave.json` exists.
- Target JSON parses successfully.
- Parsed target JSON matches parsed source JSON semantics.
- Target file uses 2-space indentation, contains no tabs, and ends with trailing newline.
- Target `$schema` remains `https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json`.
- Target `name` remains `magi-ui-synthwave`.
- Target `colors` object contains all 51 required Pi color tokens.
- Target `export` object contains exactly `pageBg`, `cardBg`, and `infoBg`.
- No unrelated platform files change in this task.

### Guardrails
- Do not describe result as verbatim byte copy.
- Do not rename theme file, theme name, schema URL, color tokens, `vars` keys, or `export` keys.
- Do not add extra Pi assets, package metadata, screenshots, binaries, or build scripts.
- Do not normalize content in ways that change parsed values.

### Verification
- Command:
  ```bash
  python3 - <<'PY'
  import json
  from pathlib import Path

  source = Path('/Users/magimetal/.pi/agent/themes/magi-ui-synthwave.json')
  target = Path('platforms/pi/themes/magi-ui-synthwave.json')

  src = json.loads(source.read_text())
  tgt_text = target.read_text()
  tgt = json.loads(tgt_text)

  assert tgt == src
  assert '\t' not in tgt_text
  assert tgt_text.endswith('\n')
  assert '  "$schema":' in tgt_text
  assert '  "vars": {\n    "bg":' in tgt_text
  assert '  "colors": {\n    "accent":' in tgt_text
  assert '  "export": {\n    "pageBg":' in tgt_text
  assert tgt['$schema'] == 'https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json'
  assert tgt['name'] == 'magi-ui-synthwave'
  assert len(tgt['colors']) == 51
  assert list(tgt['export'].keys()) == ['pageBg', 'cardBg', 'infoBg']
  print('Pi theme validation passed')
  PY
  ```

## Task 2 — Add Pi platform README
### What
Create `platforms/pi/README.md` matching other file-based platform docs. Document exact file location, exact install destinations, exact activation methods, and note that target is file-based only.

### References
- New doc: `platforms/pi/README.md`
- Similar docs: `platforms/zed/README.md`, `platforms/opencode/README.md`, `platforms/slack/README.md`
- Pi docs: `/Users/magimetal/.nvm/versions/node/v24.14.0/lib/node_modules/@mariozechner/pi-coding-agent/docs/themes.md`
- Theme file: `platforms/pi/themes/magi-ui-synthwave.json`

### Acceptance criteria
- README includes `Files`, `Install`, `Activation`, and `Notes` sections.
- `Files` section references `themes/magi-ui-synthwave.json` exactly.
- `Install` section states global install path `~/.pi/agent/themes/`.
- `Install` section states optional project-local path `.pi/themes/`.
- `Activation` section states `/settings` and `settings.json` options.
- `Activation` section shows theme name `magi-ui-synthwave` exactly.
- README states target is file-based and does not claim build, packaging, or marketplace distribution.

### Guardrails
- Keep doc scoped to Pi target only.
- Do not document package-based theme discovery, CLI flags, or other Pi theme-loading paths unless task scope expands later.
- Do not describe project-local `.pi/themes/` as required; it is optional alternative path.
- Preserve terse style used by other platform READMEs.

### Verification
- Command:
  ```bash
  rg -n "themes/magi-ui-synthwave.json|~/.pi/agent/themes/|\.pi/themes/|/settings|\"theme\": \"magi-ui-synthwave\"|file-based" platforms/pi/README.md
  ```
- Manual check: confirm README wording says `.pi/themes/` is optional and activation uses theme name `magi-ui-synthwave` exactly.

## Task 3 — Update root workspace README
### What
Add Pi target to root `README.md` so workspace inventory, platform links, and structure diagram include `platforms/pi`.

### References
- File: `README.md`
- New linked doc: `platforms/pi/README.md`

### Acceptance criteria
- `Available targets` list includes `platforms/pi` with concise description.
- Platform README links include Pi README.
- Workspace tree includes `pi/` under `platforms/`.
- Existing target descriptions remain intact.

### Guardrails
- Do not rewrite README narrative beyond Pi-target additions.
- Do not reorder existing platforms unless needed for clean insertion.
- Keep root README workspace-focused, not Pi-internals-focused.

### Verification
- Command:
  ```bash
  rg -n "platforms/pi|Pi README|pi/" README.md
  ```
- Manual check: confirm new Pi lines appear in `Available targets`, platform README links, and workspace tree.

## Task 4 — Update changelog
### What
Add unreleased changelog entries for Pi target artifact, Pi platform README, and AGENTS guidance updates.

### References
- File: `CHANGELOG.md`
- Section: `## [Unreleased]`

### Acceptance criteria
- `Added` section mentions new Pi target at `platforms/pi`.
- Entry mentions `platforms/pi/README.md`.
- Entry mentions `AGENTS.md` expansion for Pi paths, validation, and theme rules.
- Entry stays consistent with existing changelog voice and granularity.

### Guardrails
- Do not create release version or date.
- Do not modify historical release sections.
- Keep entries limited to user-visible repo changes from this work.

### Verification
- Command:
  ```bash
  rg -n "Pi target|platforms/pi|platforms/pi/README.md|AGENTS.md" CHANGELOG.md
  ```

## Task 5 — Expand AGENTS.md for Pi target
### What
Update repository agent guidance so Pi becomes first-class target in scope, paths, validation commands, theme rules, and current reality sections.

### References
- File: `AGENTS.md`
- Existing sections to update:
  - `Repository Scope`
  - `Important Paths`
  - `Build / Lint / Test Commands`
  - `Theme-Specific Rules`
  - `Current Reality Check`

### Acceptance criteria
- `Repository Scope` lists `platforms/pi` among implemented targets.
- `Important Paths` includes Pi root, Pi README, and Pi theme JSON.
- `Build / Lint / Test Commands` includes Pi JSON validation command.
- Pi guidance states target is file-based and has no dedicated build/test runner.
- Pi validation guidance checks schema URL, theme name, 51 color tokens, and exact `export` keys.
- `Theme-Specific Rules` gains Pi subsection with file location, JSON formatting expectations, and scope guidance.
- `Current Reality Check` includes Pi target in active inventory.

### Guardrails
- Do not disturb existing platform guidance except list expansions and Pi-specific additions.
- Do not invent Pi packaging, marketplace, or JetBrains Gradle verification steps.
- Keep Pi validation guidance static and file-based.

### Verification
- Command:
  ```bash
  rg -n "platforms/pi|magi-ui-synthwave.json|pageBg|cardBg|infoBg|file-based|no dedicated build/test runner" AGENTS.md
  ```
- Manual check: confirm Pi guidance appears in same structural style as other file-based targets and does not cite JetBrains Gradle commands as Pi verification.

## Minimal verification sequence
1. Run Pi JSON semantic-and-format validation:
   ```bash
   python3 - <<'PY'
   import json
   from pathlib import Path

   source = Path('/Users/magimetal/.pi/agent/themes/magi-ui-synthwave.json')
   target = Path('platforms/pi/themes/magi-ui-synthwave.json')

   src = json.loads(source.read_text())
   tgt_text = target.read_text()
   tgt = json.loads(tgt_text)

   assert tgt == src
   assert '\t' not in tgt_text
   assert tgt_text.endswith('\n')
   assert tgt['$schema'] == 'https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json'
   assert tgt['name'] == 'magi-ui-synthwave'
   assert len(tgt['colors']) == 51
   assert list(tgt['export'].keys()) == ['pageBg', 'cardBg', 'infoBg']
   print('Pi theme validation passed')
   PY
   ```
2. Spot-check exact documentation wording:
   ```bash
   rg -n "~/.pi/agent/themes/|\.pi/themes/|/settings|\"theme\": \"magi-ui-synthwave\"|platforms/pi|magi-ui-synthwave.json" README.md CHANGELOG.md AGENTS.md platforms/pi/README.md
   ```
3. Manual verification:
   - Confirm Pi README describes `.pi/themes/` as optional.
   - Confirm no JetBrains Gradle command is used or cited as Pi verification evidence.
   - Confirm changed files stay limited to `README.md`, `CHANGELOG.md`, `AGENTS.md`, `platforms/pi/README.md`, and `platforms/pi/themes/magi-ui-synthwave.json`.
   - Confirm no unrelated platform folders change.

## Risks / Unknowns
- Source theme file may evolve before execution. Implementation should read current source file at execution time, not cached text.
- Pi docs may add more theme-loading paths later. This plan intentionally limits README wording to global path, optional project-local path, and activation methods required by current review.
- AGENTS Pi validation command must stay synchronized with actual theme schema and export section if upstream Pi format changes.

## Self-review
- Replaced verbatim-copy assumption with semantic-parity plus repo-formatting requirement.
- Pinned Pi README requirements to documented install paths and activation methods from Pi docs.
- Stated explicitly that JetBrains Gradle verification is not relevant evidence for Pi target work.
- Strengthened verification for schema URL, exact export keys, README wording, and file-format checks.
- Scope still limited to plan-only revision. No PRD. No ADR.
