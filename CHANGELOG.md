# Changelog

## v2.0 (2026-10-31)

- Split the skill into a short core `SKILL.md` and reference files under `slop-popper/references/` (command modes, the full report format, no-git and no-tests fallbacks, tooling, and one file per slop category). Agents read only the files a command needs, so common runs use fewer tokens.
- `/deslop` with no staged or uncommitted changes now says so and stops.
- `/deslop-approve` with no saved list re-scans the files the item keys name, and asks for a scope if there are none.
- `--dry-run` is documented as working on any editing command.
- Kept items get a K ID only when a catalog rule would otherwise flag them, and then show only as K items.
- Added the Windows `cmd` form of the no-git folder command.

## v1.0 (2026-10-17)

- First release.
