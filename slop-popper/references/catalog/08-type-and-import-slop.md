# 8. Type and import slop
Typing and imports that add noise or hide mistakes.
- Look for: unused or wildcard imports; `Any` / `any` / `object` where a real type is known; casts or `# type: ignore` that hide a real mismatch; outdated typing syntax where the language version supports the modern form (for example `List[str]` instead of `list[str]`, `Optional[X]` instead of `X | None` in Python 3.10+); redundant annotations the compiler already infers.
- Fix: remove unused and wildcard imports; replace `Any` with the real type; fix the mismatch instead of silencing it; use the project's language-version idiom.
