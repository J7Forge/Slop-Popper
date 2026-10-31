# 11. Comment noise
Comments that do not tell the reader anything the code cannot.
- Look for: the Remove or rewrite list below.
- Fix: remove or rewrite those comments; keep what the Keep list names. Follow the Form and Doc comments rules when writing or fixing any comment.

**Keep**
- Why something is done when it is not obvious: business rules, workarounds, ordering or performance constraints.
- References: ticket IDs, spec links, upstream bug links.
- Warnings about side effects, thread safety, units, or required call order.
- License and copyright headers.
- `TODO` / `FIXME` / `HACK` / `NOTE` that say what is left and why, ideally with an owner or ticket.
- Lint or compiler directives (`# noqa`, `// eslint-disable-next-line`, `#pragma`, `# type: ignore`) with a short reason.

**Remove or rewrite**
- Comments that restate the code (`i += 1  # increment i`) or a constant's name (`MAX_RETRIES = 3  # maximum number of retries`). Keep a constant's comment only if it gives the unit, source, or reason for the value.
- Step narration (`# Step 1: ...`, `// Now we loop through ...`).
- Change-log or chat residue (`# Added to fix the bug`, `// Updated per your request`, `# New implementation`).
- Commented-out code.
- Section banners and dividers, unless the project already uses them consistently.
- Doc comments on trivial helpers that only repeat the name, and full Args / Returns / Raises blocks on small functions whose name and signature already say it all. Use a one-line summary instead.
- Emoji, marketing tone, or exclamation marks.
- Stale comments that no longer match the code: fix or delete.

**Form:** full sentences, matching the surrounding style; place comments above the line they explain; keep trailing comments short.

**Doc comments on public API**, in the language's convention. Private or self-explanatory helpers do not need them.
- Python: PEP 257 docstrings (triple double quotes, one-line summary ending in a period, blank line before details). Match the project's style (Google, NumPy, reST) if one is in use.
- C# / .NET: `///` XML doc comments (`<summary>`, `<param>`, `<returns>`).
- Java: Javadoc `/** ... */` with `@param`, `@return`, `@throws`.
- Kotlin: KDoc `/** ... */`.
- JavaScript / TypeScript: JSDoc / TSDoc `/** ... */`; in TypeScript, skip type tags the signature already states.
- Go: a sentence starting with the identifier's name on exported names.
- Rust: `///` for items, `//!` for modules.
