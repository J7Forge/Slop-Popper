# 6. Error-handling slop
Error handling that hides, duplicates, or fakes recovery.
- Look for:
  - Swallowed errors: `except: pass`, `catch {}`, or a broad catch that returns a default with no log or reason.
  - Try/except around every call instead of the few that can actually fail.
  - Log-and-rethrow at every layer, so one failure is logged many times.
  - Raising a new error that drops the original cause.
  - Generic messages like "An error occurred" or "Something went wrong" with no detail.
  - Retry loops on errors that are not transient.
  - The same try / log / return-default template around every function, whether it can fail or not.
  - Success flags instead of errors: returning `True`/`False`, `None`, or `{"success": ..., "error": ...}` dicts where the language would normally raise or throw.
- Fix: catch the narrowest error type where you can actually handle it; let the rest propagate. Log once, at the layer that handles it. Chain causes (`raise X from err` in Python, `cause` in JS/Java). Write messages that say what failed and with which input. A deliberate broad catch needs a fallback value and a short comment or lint directive explaining why. Converting success flags to exceptions changes return types, so hold it for review unless the function is private and every caller is updated in the same pass.
