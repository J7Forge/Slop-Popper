## Review (`/deslop-review`, or `--review`)
A reviewer-only pass on a cleanup someone else made. The same pass must not both write and approve high-impact cleanup.
1. Do not edit files.
2. Read the plan, the diff, and the verification evidence.
3. Check against every file in `references/catalog/` and the Ground Rules in SKILL.md: leftover findings, new slop introduced, changes that alter behavior, and whether the size limit and one-category-per-pass rule were followed.
4. Give a verdict (approve, approve with follow-ups, or request changes) and list follow-ups for a separate cleanup pass.

## Approve (`/deslop-approve <IDs | all>`)
Applies items from the saved list (see Report Format in SKILL.md) after the user OKs them.
1. Accept one or more IDs or keys (`R2`, `R1 R3`, `billing.py:export_invoice`) or `all`, matched against the saved list. If an ID's key no longer matches the code, or there is no saved list, re-scan the files the keys name (ask for a scope if there are none) and show the new IDs first; never guess.
2. Apply each approved item as its own small change, using the fix in its category file. Approval lifts the held-for-review limit for that item only; every other rule still applies.
3. For test-only code, follow the removal step in `references/catalog/01-dead-code.md`.
4. Run tests and checkers after each item. If one fails, back it out and report it.
5. Report what was applied, what was backed out, and which IDs are still open.

## Reject (`/deslop-reject <IDs>`)
Closes review items the user doesn't want changed.
1. Accept IDs or keys, matched against the saved list the same way as Approve.
2. Remove that item's `REVIEW(deslop)` comment, if any; change nothing else. This is an edit, so on a copy without git that has no snapshot yet, first create the snapshot (see `references/no-git-no-tests.md`) and `last-report.md`, as for any edit.
3. Record the key as rejected in the saved list's closed list, with a short hash of that symbol's source. Don't flag it again unless the hash no longer matches, meaning the code changed; then give it the next new R ID and note "previously rejected as Rn".
4. Short report: what was rejected and which IDs are still open.

## Dry run (`--dry-run`)
Add to any editing command. Do the style lookup, scan, and plan, then stop. List each change that would be applied, grouped by pass, plus the Needs further review, Closed (from the saved list, if any), and Kept items sections of the Full report (`references/full-report.md`). Stamp `Mode: careful (default)` or `Mode: classic (--classic)` on the plan. Edit nothing, run no formatters, stage nothing, and write no files (no save point, snapshot, or `last-report.md`): the plan goes in chat only.

## Classic (`--classic`)
Add to `/deslop`, `/deslop-full`, or `/deslop-since` only (alone or with `--dry-run`). Does not change `/deslop-review`, `/deslop-approve`, or `/deslop-reject`.
1. For this run only, use classic mode under Ground Rule 1 in SKILL.md: signatures, logic or behavior, whole-file reformats, config values / IDs / constants, and renames used outside the codebase may be applied when a catalog finding needs that minimal change and the other Ground Rules still hold.
2. Still hold: swapping libraries or patterns; deleting test-only code; keep-list items; secrets; size-limit plans until the user OKs; anything that is not a cleanup. A stricter `DESLOP.md` or keep list still wins.
3. Stamp every report and `--dry-run` plan with `Mode: classic (--classic)`. Without this flag, stamp `Mode: careful (default)`.
4. Never commit or push. Classic only changes which Rule 1 items may apply without a separate OK.
