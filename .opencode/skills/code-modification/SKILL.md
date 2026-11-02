---
name: code-modification
description: Safely plan and implement changes in an existing codebase. Use before modifying application code, configuration, tests, or project structure.
---

# Code modification

## Understand before editing

1. Restate the requested behavior internally; identify what must change and what must remain unchanged.
2. Locate the implementation using `code-search` procedures.
3. Inspect the relevant symbol, callers, data flow, configuration, and tests.
4. Check dependency APIs with Context7 when the change depends on external-library behavior or version-specific details.
5. Identify compatibility risks and the smallest coherent change.

Do not modify code based only on one matching line or a semantic-search snippet.

## Implement

- Follow existing project conventions and supported Python version.
- Prefer a small, focused diff over unrelated cleanup.
- Avoid adding dependencies when the standard library or existing dependencies suffice.
- Keep interfaces and side effects explicit.
- Handle errors at the layer that can respond meaningfully; do not silently swallow exceptions.
- Do not introduce speculative abstractions or configuration.
- Do not overwrite user changes or unrelated files.
- Ask before destructive, irreversible, externally visible, or data-loss operations.

## Before finishing

1. Review the changed code in context.
2. Search for affected usages and call sites.
3. Check whether tests need to be added or updated.
4. Hand off to `code-verification` for appropriate checks.
5. Summarize files changed, behavior changed, checks run, and known limitations.

Do not claim that a change is correct merely because the edit applied cleanly.
