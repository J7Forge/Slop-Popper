# 7. Naming slop
Names that say nothing about meaning.
- Look for: `data`, `result`, `res`, `temp`, `tmp`, `obj`, `item`, `value`, `info`, `stuff`, `val2`; class names ending in `Manager`, `Handler`, `Helper`, `Processor`, `Service` that hide what the class does; `utils` / `helpers` / `common` modules of unrelated functions; booleans not phrased as questions (`flag` instead of `is_ready`); names that disagree with what the code does.
- Fix: rename to what the thing is or does (`unpaid_invoices`, `parse_order_row`, `is_expired`). Splitting a catch-all module by topic changes file layout, so hold it for review. Leave short names where convention expects them (`i` in a small loop, `e`/`err` in a catch, `x, y` for coordinates).
