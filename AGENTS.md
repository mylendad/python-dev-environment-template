# AGENTS.md

## Working principles

- Prefer the smallest reliable action that answers the task.
- Inspect before changing; make the smallest coherent change.
- Preserve existing project conventions unless the task requires changing them.
- Do not invent project structure, APIs, test results, or runtime behavior.
- Distinguish static inspection, test verification, runtime verification, and end-to-end verification.

## Repository exploration

Choose the tool by the question:

| Need | Tool |
|---|---|
| Exact text, identifier occurrence, config key, error | `rg` |
| File discovery | `rg --files` or `fd` |
| Search Git-tracked text | `git grep` |
| Symbol definition, references, callers/callees, structure | Serena |
| Find implementation by behavior when its name is unknown | Semble |

Rules:
- Use `rg`, not `grep`, for repository text search.
- Use `rg --files` or `fd`, not `find`, for file discovery.
- Use Serena for symbol relationships; do not use it for ordinary text search.
- Use Semble for semantic discovery; do not use it for exact identifiers.
- Do not run every tool automatically. Do not repeat equivalent searches.
- Start in the narrowest relevant directory and expand only when evidence is insufficient.
- Read targeted ranges; read a whole file only when its size or context justifies it.
- If a preferred tool is unavailable or fails, use the closest reliable alternative and state any material limitation.

Detailed search procedure: `.opencode/skills/code-search/SKILL.md`.

## External documentation

Use Context7 for external libraries, frameworks, SDKs, APIs, and tools when:
- implementing or changing code that depends on them;
- checking APIs, configuration, version-specific behavior, or recommended usage;
- debugging a problem that may come from dependency usage;
- adding a dependency.

Prefer the documentation version matching the project's installed dependency. Do not use Context7 for project-specific logic already established by repository inspection.

Context7 is expected to be configured globally in OpenCode. See `README.md` if it is not available.

## Before changing code

1. Locate the implementation and understand its relevant behavior.
2. Inspect affected symbols and callers.
3. Check related configuration, dependencies, and tests.
4. Identify the intended behavior and likely side effects.
5. Avoid unrelated refactoring.

## After changing code

1. Inspect the diff and affected usages.
2. Run the narrowest relevant tests first.
3. Run applicable linting, formatting, and type checks.
4. Run the application or workflow when the task requires runtime proof.
5. Check `git diff` and `git status --short` for unintended changes.

Never claim a test, startup, runtime workflow, or end-to-end operation succeeded unless it was actually executed and verified. Report what was checked and what was not.
