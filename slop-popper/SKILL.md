---
name: slop-popper
description: >-
  Use when the user says deslop, anti-slop, or AI slop, or wants a
  behavior-preserving cleanup of noisy, repetitive, over-abstracted,
  over-defensive, over-commented, or weakly tested AI-generated code. `/deslop`
  runs a quick pre-commit cleanup; `/deslop-full`, `/deslop-since`,
  `/deslop-review`, `/deslop-approve`, `/deslop-reject`, and `--dry-run` cover the rest.
---
# Slop Popper

"Slop" is code that runs but carries noise a careful human would not write: duplication, dead code, needless layers, defensive clutter, vague names, noisy comments, and weak tests. Remove it from working code without changing behavior.

## Commands

| Command | What it does |
|---|---|
| `/deslop` (or plain `deslop`) | Quick pre-commit cleanup of staged changes, or uncommitted changes if none, without questions |
| `/deslop-full [path]` | Every Workflow step in full, including the written plan and before-and-after tool results, on the path, module, or file list given, or the whole repo |
| `/deslop-since <commit or tag>` | Same as `/deslop-full`, on files changed since that point plus uncommitted changes (`git diff <ref>`), to tidy a release |
| `/deslop-review` | Reviews a cleanup someone else made, no edits; gives a verdict |
| `/deslop-approve <IDs \| all>` | Applies approved items from the saved Needs further review list |
| `/deslop-reject <IDs>` | Closes items from that list without changing the code |
| `--dry-run` (on any editing command) | Plan in chat only; nothing edited or written |

`--review` is an alias for `/deslop-review`. A natural-language request with a target ("deslop this module") runs as `/deslop-full` on that target.

**Do not use for:** new features, product changes, broad redesigns, formatting-only requests (use the formatter), or code whose behavior is too unclear to protect with tests or a written verification plan.

## Reference files

Every path in this skill, including paths inside reference files, is relative to the folder that holds this file. Read a file only when its condition applies:
1. `references/no-git-no-tests.md`: first, if the target is not a git repository or has no tests. Without git, it changes the scope and replaces the save point.
2. `references/modes.md`: for `/deslop-review` (or `--review`), `/deslop-approve`, `/deslop-reject`, or `--dry-run`.
3. `references/tooling.md`: if the project has no linters or checkers configured.
4. `references/catalog/` (files named in the Slop Catalog below):
   - `/deslop`: the file for each category with findings, before its pass (category 13: before listing its items).
   - `/deslop-full`, `/deslop-since`, `/deslop-review`: every file, before planning or reviewing.
   - `/deslop-approve`: the file for each approved item's category, before applying it.
   - `references/catalog/11-comment-noise.md`: also before writing or fixing any comment or docstring.
5. `references/full-report.md`: before writing a Full report or a `--dry-run` plan.

## Ground Rules

1. **Cleanup, not rewrite.** Behavior stays the same. Leave existing logic, control flow, signatures, file layout, and formatting as written unless a catalog finding needs a minimal change. When in doubt, hold it for review instead of editing.
   - **Applied automatically:** deleting dead or duplicate code; fixing vague names (updating every caller in the same pass); removing wrappers that only pass calls through; moving code to the module it belongs in (updating imports); removing noise comments and debug output; marking deliberate catch-alls; fixing a clearly wrong comment or docstring; adding tests (except in `/deslop`, which lists them for review instead).
   - **Held for review (needs the user's OK):** changing signatures, parameters, or return types; changing logic or behavior; reformatting whole files; swapping libraries or patterns; changing config values, IDs, or constants; renaming anything used outside the codebase (a public API, CLI flag, file format, or saved setting); deleting test-only code.
2. **Lock behavior first** with the narrowest tests that cover what you will touch. If tests cannot come first, write the verification plan before editing.
3. **Follow project conventions.** Always run the style lookup first (Workflow step 1). What it finds overrides this skill's defaults for the lines being changed, but never justifies restyling untouched code.
4. **Delete before you add.** Reuse existing helpers and patterns; add no dependencies unless asked.
5. **Small, reversible diffs.** One catalog category per pass; never mix unrelated changes.
6. **Respect the scope.** Stay inside the files the command or user gave you.
7. **Size limit.** If the whole run's plan (not each pass) would touch more than about 10 files or 300 changed lines, stop after the plan, show it, and ask before editing. `/deslop-full` on a whole repository often hits this; once approved, run the plan in batches that each stay under the cap.
8. **Never commit or push to the user's repo or branch.** The user does that. Private snapshot commits in a scratch copy or a temporary git repo (see `references/no-git-no-tests.md`) are fine for locking and diffing each pass.

## Slop Catalog

Classify every finding into one category before editing. Categories 1 through 12 are also the pass order and the numbers reports use; category 13 only adds review items and is never applied. Each line below is what to scan for; the category file has the full list and the fix. Rule 1 limits every fix: anything on its held-for-review list goes under Needs further review instead.

1. `01-dead-code.md`: unused code, unreachable branches, stale feature flags, commented-out blocks. Test-only code is not dead: flag it for review, don't delete it.
2. `02-duplication.md`: copy-paste logic, near-identical functions, repeated literals, helpers that redo an existing utility.
3. `03-needless-abstraction.md`: pass-through wrappers, single-use layers, interfaces with one implementation, factories that build one thing, options no caller passes. Held for review: removing a parameter from public or external-facing code.
4. `04-boundary-violations.md`: UI code doing I/O, business logic in handlers, hidden global state, wrong-direction imports, side effects in getters, hard-coded secrets. Never move, rotate, or delete a secret automatically; flag it, and never copy its value into the report.
5. `05-defensive-slop.md`: guards that cannot fire, such as `None` checks on values never `None`, re-validation after the boundary, and defaults for required values.
6. `06-error-handling-slop.md`: swallowed errors, try/except around every call, log-and-rethrow, dropped causes, generic messages, retries on errors that are not transient, success flags instead of errors. Held for review: success flags to exceptions, unless private with every caller updated.
7. `07-naming-slop.md`: vague names (`data`, `tmp`), `Manager` or `Helper` class names, catch-all `utils` modules, booleans not phrased as questions, names that disagree with the code. Held for review: splitting a catch-all module.
8. `08-type-and-import-slop.md`: unused or wildcard imports, `Any` where the type is known, casts or `# type: ignore` hiding a mismatch, outdated typing syntax.
9. `09-logging-and-debug-slop.md`: debug prints, "entering" or "done" logs, logs in tight loops, logged secrets or payloads, repeated or same-level logs, chatty messages, demo `__main__` blocks. Held for review: visible UI text; a `__main__` or demo entry point that docs, scripts, build files, or CI mention.
10. `10-placeholder-content.md`: reachable stubs, fake sample values outside fixtures and docs, generic UI text. Held for review: reachable stubs, real-looking values, visible UI text.
11. `11-comment-noise.md`: comments that restate the code, step narration, change-log or chat residue, commented-out code, banners, boilerplate doc blocks, stale comments.
12. `12-missing-tests.md`: changed code with no test, tests that only check no error was raised or that mock the thing under test, missing edge cases.
13. `13-ui-design-defaults.md`: tiny body text, shadows everywhere, repeated eyebrow/title stacks, emoji badges, default blue/purple palettes, uniform grids, extreme gradients.

## Tooling

Run the project's existing linters and checkers before and after cleanup, and report the difference.

Keep checkers from writing caches into the tree, especially in `--dry-run`: for example `ruff check --no-cache`, `mypy --cache-dir=/dev/null` (`--cache-dir=nul` on Windows), and `python -B` or `PYTHONDONTWRITEBYTECODE=1`. Remove any cache that still appears.

Treat tool output as leads, not orders: confirm each finding (for example, code only reached through reflection or dynamic dispatch is not dead).

Show only failing lines and counts from checkers and tests, never full logs. Use a quiet or summary option where the tool has one.

## Workflow

Every editing command follows these steps; the modes note where they differ.

1. **Find style sources.** Before any edit, look for, in this order:
   - A coding-style skill in the available skill list (personal, team, or language). Read it if one fits the language.
   - Lint and formatter configs in the repo, such as `pyproject.toml`, `.editorconfig`, `.eslintrc*`, `.prettierrc*`, `tsconfig.json`, `detekt.yml`, or `.golangci.yml`.
   - Style docs: `CONTRIBUTING*`, `STYLE*`, `docs/style*`, or a style section in the README.
   - A project cleanup procedure, such as `DESLOP.md`. Follow it where it is stricter than this skill.
   - A keep list: a "do not change" or "keep" section in the coding standards or style docs, or a file like `KEEP.md` or `.deslopkeep`. Entries name functions, files, values, or patterns; secrets by name only, never by value. Never edit, move, rename, or delete a kept item; skip findings that touch one and note them in the report. Only the user changes the keep list, and it wins over every other rule, including test-only flagging. A kept item gets an ID (`K1`, `K2`) only when a catalog rule would otherwise flag it; it then shows only as a K item, never as an R item, and can't be approved with `/deslop-approve` until the user removes it from the list.
   - The code itself: the dominant patterns in nearby files (docstring format, naming, error handling).

   If sources conflict, the order is: the keep list, then explicit user instruction, then repo docs and configs, then the style skill, then this skill's defaults. Note the sources found, including the keep list, or "none found".
2. **Lock behavior.** Identify what must not change; run or add the narrowest tests. Record a baseline of lint, type, and dead-code findings, running checkers cache-free.
3. **Plan.** List findings by catalog category, the files in scope, and the pass order. Check the plan against the size limit.
4. **Run one pass per catalog category, 1 through 12, in order** (safest first). Skip passes with no findings. After any deletion, re-check in the same pass whether the deleted code's callees, helpers, and test fakes now have no callers, and handle them the same way. Re-run targeted tests after each pass.
5. **Quality gates.** No new test failures compared to the baseline (tests that already failed may keep failing, but list them); lint, type, and existing security checks pass or improve against the baseline. If a gate fails, fix it or back out that change; never force it through.
6. **Docs and changelog.** Update stale references (renamed functions, moved files) in docs the cleanup touched. If the project keeps a changelog, add a new bullet for the cleanup (under the current version or an Unreleased heading, as the project does); never edit or remove existing bullets. `REVIEW(deslop)` comments stay in the source until their item is approved or rejected; follow any packaging or release step's rule on stripping them, and say in the report that they are present.
7. **Report** (see Report Format).

## Pre-commit mode (`/deslop`)

- **Scope:** staged changes (`git diff --cached`); if nothing is staged, uncommitted changes (`git diff`); if there are neither, say so and stop. Within those files, prefer the changed lines and the code they call; `/deslop-since` does the same.
- **Save point first.** Before any edit, save `git diff --cached --binary` to `.git/deslop/<timestamp>-staged.patch` and `git diff --binary` to `.git/deslop/<timestamp>-unstaged.patch` (inside `.git`, so never committed). To undo, run `git restore --source=HEAD --staged --worktree <files>` on every file the cleanup touched or the patches list, then apply the staged patch with `git apply --index` and the unstaged patch with `git apply`. Give the save-point paths and these undo commands in the report; run the undo only when the user asks.
- Run the Workflow without asking questions. Don't write new tests; list the tests you'd add under Needs further review. Stop only if the size limit is hit, or tests fail and the fix is not obvious.
- Afterward, re-stage only the files you changed (`git add <those files>`).

## Report Format

**Needs further review:** every finding not applied, each with an R ID (`R1`, `R2`, and so on) and a stable key (`file:symbol`, for example `billing.py:export_invoice`). The key is the identity: an R ID stays with its key across runs, and closed IDs are retired, never reused. With no saved list (a fresh copy), numbering starts at R1. One line each: the file, what it is, and why it was held (held-for-review list, test-only code, over the size limit, or not sure it's safe). For test-only code, add its callers, replacement, references, and the lines removal would cut.

**Saved list.** Every editing run saves the open Needs further review items, plus a closed list of approved and rejected keys with their retired IDs, to `.git/deslop/last-report.md` (or `<tree>.deslop/last-report.md` without git). `--dry-run` and `/deslop-review` never write it. Open items appear again in each report until they're closed or the code changes. Don't flag a rejected key again unless its code changed (see Reject in `references/modes.md`).

**Short report** (`/deslop`, `/deslop-approve`, `/deslop-reject`): the save-point paths (for `/deslop`), changed files, a one-line count per category, the test result, the Needs further review list, closed items, and any kept items. Skip empty sections.

**Full report** (`/deslop-full`, `/deslop-since`): sections in `references/full-report.md`.
