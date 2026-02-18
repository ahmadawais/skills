# code-style
- Errors first, happy path last. Confidence: 0.95
- Use guard clauses up top, main logic at the bottom. Confidence: 0.95
- Never nest deeper than 2-3 levels (ideally 1 level max); flatten with early `return`, `continue`, `break`. Confidence: 0.95
- Avoid `switch/case` and `else` — use `if` guard clauses almost always. Confidence: 0.95
- Always name the result — never bare `await Promise.all(...)` or `return Promise.all(...)`; name describes what was collected. Confidence: 0.95
- Max 1 level of nesting inside any function; if level 2 is needed, extract a helper. Confidence: 0.90
- No `result`, `data`, `nested`, `items` — use descriptive names that describe what the variable actually contains. Confidence: 0.90
