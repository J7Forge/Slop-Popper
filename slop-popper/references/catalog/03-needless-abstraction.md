# 3. Needless abstraction
Structure that adds indirection without adding value.
- Look for: pass-through wrappers, single-use helper layers, interfaces or base classes with one implementation, factories that build one thing, config for values that never change; optional parameters, flags, and `**kwargs` / `...rest` options no caller passes.
- Fix: inline it and drop options nobody uses, updating callers. Keep an abstraction only when it has more than one real user or isolates a real boundary. Removing a parameter from public or external-facing code is held for review.
