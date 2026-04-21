# Plan: commit current Pi theme work to main

## Objective
Create one clean commit for current Pi target work, add `.pi-lens/` to `.gitignore`, include existing Pi plan artifact in commit, refresh `CHANGELOG.md` only as needed, then push `main` to `origin/main` without force only if branch/upstream and remote-drift gates pass.

## Evidence collected
- Working tree currently on `main` with upstream `origin/main`.
- Already staged: `AGENTS.md`, `CHANGELOG.md`, `README.md`, `platforms/pi/README.md`, `platforms/pi/themes/magi-ui-synthwave.json`.
- Untracked noise/items: `.pi-lens/` and `docs/`.
- Existing plan artifact user wants included: `docs/plans/add-pi-theme-target.md`.
- Recent commit subjects use lowercase imperative style with no prefix, e.g.:
  - `add Slack platform target and align repo docs metadata`
  - `prepare 2.2.0 changelog for VS Code release`
  - `refresh root readme narrative`
- `CHANGELOG.md` already has staged unreleased Pi entries for target, README, and `AGENTS.md` updates.

## Assumptions
- Push target remains current branch `main`; no branch switch needed.
- `origin` auth already works for non-force push.
- User means include existing Pi plan artifact `docs/plans/add-pi-theme-target.md`, not every file under `docs/plans/`.
- `.pi-lens/` is local tooling noise and should stay untracked.

## Task 1 — Confirm hard-stop branch/upstream gates and message style
### What
Re-check branch, upstream, staged/untracked state, and recent commit subjects immediately before edits/staging. If branch or upstream is not exact expected target, stop plan execution before any staging, commit, or push.

### References
- Repo root: `.`
- Files expected in scope:
  - `.gitignore`
  - `CHANGELOG.md`
  - `README.md`
  - `AGENTS.md`
  - `platforms/pi/README.md`
  - `platforms/pi/themes/magi-ui-synthwave.json`
  - `docs/plans/add-pi-theme-target.md`

### Acceptance criteria
- Current branch confirmed as `main`; any other value is hard-stop.
- Upstream confirmed as `origin/main`; any other value is hard-stop.
- Only intended Pi-theme/repo-doc files plus `.pi-lens/` noise appear in status.
- Commit subject style confirmed as lowercase imperative, no conventional-commit prefix.

### Guardrails
- Hard-stop if `git rev-parse --abbrev-ref HEAD` is not `main`.
- Hard-stop if `git rev-parse --abbrev-ref --symbolic-full-name @{u}` is not `origin/main`.
- Do not stage with `git add .` or `git add docs/`.
- Do not switch branches.
- Do not commit yet.

### Verification
- Commands:
  ```bash
  git status --short
  git rev-parse --abbrev-ref HEAD
  git rev-parse --abbrev-ref --symbolic-full-name @{u}
  git log -5 --oneline
  ```
- Manual check: any branch/upstream mismatch stops remaining tasks. No direct commit/push from detached HEAD, feature branch, or non-`origin/main` upstream.

## Task 2 — Add `.pi-lens/` to `.gitignore`
### What
Update root `.gitignore` to ignore local `.pi-lens/` cache directory.

### References
- File: `.gitignore`
- Local noise path: `.pi-lens/`

### Acceptance criteria
- `.gitignore` contains exactly one ignore entry for `.pi-lens/`.
- `.pi-lens/` no longer appears as untracked content in normal `git status` output.
- `.gitignore` is staged for commit.

### Guardrails
- Do not add broad patterns such as `.pi*`.
- Do not alter unrelated ignore rules.
- Keep diff minimal.

### Verification
- Commands:
  ```bash
  rg -n '^\.pi-lens/$' .gitignore
  git add .gitignore
  git status --short --ignored .pi-lens .gitignore
  ```
- Manual check: status should show `.pi-lens/` as ignored (`!!`) rather than untracked (`??`).

## Task 3 — Refresh changelog only if staged Pi summary is incomplete
### What
Review existing staged `CHANGELOG.md` Pi entries. Keep them if accurate. Edit only if unreleased section is missing one of current shipped artifacts or materially misstates scope.

### References
- File: `CHANGELOG.md`
- Section: `## [Unreleased]`
- Pi files in scope:
  - `platforms/pi/themes/magi-ui-synthwave.json`
  - `platforms/pi/README.md`
  - `AGENTS.md`
  - `README.md`

### Acceptance criteria
- `CHANGELOG.md` unreleased section mentions Pi target artifact and Pi README.
- `CHANGELOG.md` mentions `AGENTS.md` Pi guidance update.
- No unnecessary changelog note added for `.pi-lens/` ignore unless repo owner explicitly wants repo-maintenance entry.
- Historical release sections remain unchanged.

### Guardrails
- Do not invent release version/date.
- Do not rewrite unrelated Slack/VS Code history.
- Keep wording aligned with existing changelog voice.

### Verification
- Commands:
  ```bash
  git diff --cached -- CHANGELOG.md
  rg -n 'Pi target|platforms/pi|platforms/pi/README.md|AGENTS.md' CHANGELOG.md
  ```
- Manual check: unreleased section still reads like user-visible repo changes, not internal tool noise.

## Task 4 — Stage exact commit set, including existing plan artifact only
### What
Stage only requested Pi work, repo-doc updates, `.gitignore`, and existing Pi plan artifact. Exclude `.pi-lens/` and exclude this dispatch plan unless user later explicitly asks to commit it.

### References
- Exact files to stage:
  - `.gitignore`
  - `CHANGELOG.md`
  - `README.md`
  - `AGENTS.md`
  - `platforms/pi/README.md`
  - `platforms/pi/themes/magi-ui-synthwave.json`
  - `docs/plans/add-pi-theme-target.md`
- Paths to exclude:
  - `.pi-lens/`
  - `docs/plans/commit-pi-theme-work-to-main.md`

### Acceptance criteria
- `git diff --cached --name-only` matches exact intended file set.
- `docs/plans/add-pi-theme-target.md` is staged.
- `docs/plans/commit-pi-theme-work-to-main.md` is not staged.
- `.pi-lens/` is not staged and remains ignored.
- No unrelated files are staged.

### Guardrails
- Do not use wildcard staging for `docs/`.
- Do not stage editor junk, caches, or new unrequested docs.
- If any unexpected file appears staged, unstage before continuing.

### Verification
- Commands:
  ```bash
  git add .gitignore CHANGELOG.md README.md AGENTS.md \
    platforms/pi/README.md platforms/pi/themes/magi-ui-synthwave.json \
    docs/plans/add-pi-theme-target.md
  git diff --cached --name-only
  git diff --cached --name-only -- docs/plans/add-pi-theme-target.md docs/plans/commit-pi-theme-work-to-main.md
  git status --short --ignored .pi-lens docs/plans/add-pi-theme-target.md docs/plans/commit-pi-theme-work-to-main.md
  ```
- Manual check: cached names should include `docs/plans/add-pi-theme-target.md`, should not include `docs/plans/commit-pi-theme-work-to-main.md`, and `.pi-lens/` should appear only as ignored (`!!`).

## Task 5 — Run remote-drift gate and minimal pre-commit verification
### What
Fetch remote `main`, confirm local `HEAD` and `origin/main` are exactly synchronized before any direct commit/push flow continues, then validate staged content. Since repo is file-based for Pi target, use syntax/semantic checks plus staged-diff hygiene, not broad build work.

### References
- Remote branch: `origin/main`
- Pi theme file: `platforms/pi/themes/magi-ui-synthwave.json`
- Staged diff: index state for exact file set above

### Acceptance criteria
- `git fetch origin main` completes successfully.
- `git rev-list --left-right --count origin/main...HEAD` returns `0	0`; any other result is hard-stop before commit/push.
- Pi theme validation passes.
- Cached diff has no whitespace/pathology errors.
- Staged diff still matches intended files and content after `.gitignore` update.

### Guardrails
- Hard-stop if fetch fails or ahead/behind count is not exact `0	0`.
- Do not run unrelated broad refactors or force formatting.
- Do not skip validation and claim success.
- If verification fails, fix before commit.

### Verification
- Commands:
  ```bash
  git fetch origin main
  git rev-list --left-right --count origin/main...HEAD
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
  git diff --cached --check
  git diff --cached --name-only
  ```
- Manual check: only proceed if ahead/behind output is exactly `0	0` and cached names still match intended file set.

## Task 6 — Commit with repo-aligned subject, then push `main`
### What
Create one commit on `main` using repo-aligned lowercase imperative subject, then push to `origin/main` without force.

### References
- Current branch: `main`
- Upstream: `origin/main`
- Commit style examples from recent history:
  - `add Slack platform target and align repo docs metadata`
  - `prepare 2.2.0 changelog for VS Code release`
- Recommended subject:
  - `add Pi platform target and align repo docs metadata`

### Acceptance criteria
- One non-empty commit created on `main`.
- Commit subject matches repository style.
- Push succeeds to `origin/main` with no force flags.

### Guardrails
- Do not squash with unrelated previous work.
- Do not amend historical commits.
- Do not use `--force` or `--force-with-lease`.
- Do not bypass Task 1 branch/upstream hard-stop or Task 5 remote-drift gate.
- If push still rejects due to post-fetch remote drift, stop and report instead of forcing.

### Verification
- Commands:
  ```bash
  git commit -m "add Pi platform target and align repo docs metadata"
  git push
  ```
- If upstream unexpectedly missing:
  ```bash
  git push -u origin main
  ```

## Task 7 — Verify post-push state
### What
Confirm local branch clean enough, commit landed at HEAD, and branch is synchronized with upstream after push.

### References
- Repo root: `.`
- Upstream branch: `origin/main`

### Acceptance criteria
- `git log -1 --oneline` shows new commit.
- `git status -sb` shows `main...origin/main` with no ahead/behind delta after push.
- Remaining untracked content, if any, is only intentionally excluded items.

### Guardrails
- Do not claim push success without command evidence.
- Do not hide remaining untracked files.
- If anything remains unstaged unexpectedly, report it.

### Verification
- Commands:
  ```bash
  git log -1 --oneline
  git status -sb --ignored
  git rev-list --left-right --count @{u}...HEAD
  ```
- Expected final sync result:
  - `git rev-list --left-right --count @{u}...HEAD` returns `0	0`

## Risks / Unknowns
- Push can still fail due to auth, network, or post-fetch remote movement. If so, stop after local commit and report exact rejection.
- This planning step creates `docs/plans/commit-pi-theme-work-to-main.md`. Current execution scope should exclude that file unless user later explicitly asks to commit dispatch plans too.
- If unstaged changes exist inside already tracked Pi files at execution time, exact staging list must be re-checked before commit.

## Self-review
- Plan stays inside requested scope: `.gitignore`, changelog check, exact staging, commit, push.
- Plan adds explicit hard-stop for branch/upstream mismatch and explicit fetch-based remote-drift gate before commit/push.
- Plan now proves staged index includes `docs/plans/add-pi-theme-target.md`, excludes `docs/plans/commit-pi-theme-work-to-main.md`, and treats `.pi-lens/` as ignored only.
- Plan includes commit-style check, no-force push path, and minimal verification before and after push.
