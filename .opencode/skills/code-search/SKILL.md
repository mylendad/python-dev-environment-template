---
name: code-search
description: Find code efficiently using exact text search, symbol navigation, or semantic discovery. Use when exploring a repository, locating behavior, or tracing code relationships.
---

# Code search

Use the smallest tool that can reliably answer the question. Do not use every tool for every task.

## Select the tool

| Question | First choice |
|---|---|
| Where does this exact text, identifier occurrence, filename, import, error, or config key appear? | `rg` |
| Which files exist / where is a filename? | `rg --files` or `fd` |
| Where is a symbol defined? Who calls or references it? | Serena |
| What does this symbol call / what calls it? | Serena |
| Where is behavior implemented when its name is unknown? | Semble |
| Is a discovered exact usage/configuration present? | `rg` |

Important distinction:
- “Find occurrences of `build_prompt`” → `rg`.
- “Find the definition and callers of `build_prompt`” → Serena.
- “Find where the app decides whether a downloaded file is already cached” → Semble.

## `rg` rules

Run `rg` as a shell command, not as an MCP tool.

```bash
rg -n "exact_text" src/
rg -n -C 3 "function_or_setting" .
rg -n "first_name|second_name" src/
rg -n -g "*.py" "pattern" .
rg --files
rg --files -g "*.py"
```

Use `git grep` only when the search must be limited to Git-tracked files.

Do not use `grep` when `rg` can do the job. Do not use `find` when `rg --files` or `fd` can do the job.

Start narrow. Exclude generated or irrelevant directories when useful (`.venv`, `__pycache__`, `.git`, `dist`, `build`, `node_modules`). Do not assume every repository uses the same source directory.

## Serena

Use Serena for language-aware structure:
1. Resolve the relevant symbol.
2. Inspect its definition.
3. Inspect references/callers/callees needed to answer the question.
4. Follow only relevant relationships.

Do not ask Serena to perform a simple exact-text search. Do not inspect every reference if a smaller subset answers the question.

## Semble

Use Semble when the behavior is understood but names and implementation locations are unknown.

Write one focused semantic question describing:
- the behavior or decision being made;
- the input/state involved;
- the expected outcome, if known.

Example: “Find the logic that decides whether a local file has changed since its last successful processing.”

Do not guess a long list of possible function names and search each with `rg`. After semantic discovery, use Serena for symbol relationships and `rg` for exact verification where useful.

## Fallback and stopping

- If the preferred tool is unavailable, fails, or returns irrelevant results, use the closest reliable alternative.
- Do not repeat the same failed request without changing the query or approach.
- Stop when the evidence answers the question. Expand exploration only when results conflict or leave a material gap.
- Do not claim repository-wide completeness after a targeted search.
- Report relevant files/symbols and any material uncertainty; avoid narrating every command.
