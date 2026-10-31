# 4. Boundary violations
Code in the wrong place or reaching across layers.
- Look for: UI code doing file or network I/O directly, business logic inside handlers, hidden global state, wrong-direction imports, side effects in functions named like pure getters; hard-coded secrets, tokens, PINs, or credentials in source.
- Fix: move logic to the layer that owns it; keep validation and I/O at system edges. Never move, rotate, or delete a hard-coded secret automatically: flag it under Needs further review, and never copy its value into the report. A secret the keep list names (by name, never by value) is reported as a K item instead.
