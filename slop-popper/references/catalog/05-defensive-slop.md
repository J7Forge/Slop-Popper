# 5. Defensive slop
Guards against conditions that cannot happen.
- Look for: `None` checks on values that are never `None`; type checks the type system already guarantees; re-validating data already validated at the boundary; length or empty checks before loops that handle empty input fine; fallback defaults for required values; double-checking a return value the callee guarantees.
- Fix: delete the guard. Keep checks at real boundaries (user input, files, network, external APIs) and where a failure would be silent and costly.
