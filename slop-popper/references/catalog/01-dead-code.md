# 1. Dead code
Code that never runs or is never used.
- Look for: unused functions, classes, exports, variables, parameters; unreachable branches; stale feature flags; commented-out blocks; debug leftovers.
- Fix: delete it. Version control keeps history.
- **Test-only code is not dead.** If production code never calls something but tests do, or docs, a style guide, or an API list mention it, don't delete it. Flag it for review:
  - Trace callers from the app's entry points (UI handlers, CLI, jobs), not just whether anything references it. Name the code that replaced it, if any.
  - Flag the helpers that exist only to support it together with it, as one group.
  - Add one review comment above each flagged definition, as a normal comment in that language:
    `REVIEW(deslop): only tests call this; replaced by <new_function>. Helpers: <a>, <b>. Remove after owner OK.`
  - Leave the tests, any public-API listing, and the behavior unchanged. Don't add runtime deprecation warnings unless asked.
  - List the group under Needs further review with its callers, its replacement, the tests and docs that reference it, and roughly how many lines removal would cut.
  - **Removal step** (once approved): in one pass, remove the code, its helpers, its doc references, the tests that only test it, and its review comments. Tests that use it to check other behavior (for example as setup) are repointed to its replacement, not deleted, so coverage stays.
