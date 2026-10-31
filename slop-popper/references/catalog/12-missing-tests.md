# 12. Missing tests
Behavior that would break silently.
- Look for: changed code with no test, tests that only check that no error was raised, tests that mock the thing under test, missing edge cases (empty, max, invalid input).
- Fix: add focused tests for preserved behavior before or alongside cleanup. In `/deslop`, list proposed tests under Needs further review instead of writing them.
