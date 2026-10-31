# 2. Duplication
The same logic written more than once.
- Look for: copy-paste branches, near-identical functions, repeated literal values, helpers that redo what the standard library or an existing util already does.
- Fix: consolidate into one place, or use the existing utility. Don't merge things that only look alike but change for different reasons.
