---
name: code-verification
description: Verify Python code changes with focused tests, linting, type checking, runtime checks, and Git inspection. Use after implementation or when asked whether a change works.
---

# Code verification

Verification must match the claim being made. Do not run every possible check by default.

## 1. Inspect the change

```bash
git status --short
git diff --check
git diff
```

Confirm that only intended files changed. Inspect staged changes separately with `git diff --cached` when relevant.

## 2. Choose checks

Discover the project's actual tools and commands from its configuration and documentation; do not assume a particular package manager or framework.

Typical checks, only when configured:

- Tests: project test command (for example, `pytest`).
- Lint: configured linter (for example, `ruff check .`).
- Formatting: configured formatter check (for example, `ruff format --check .`).
- Types: configured type checker (for example, `pyright` or `mypy`).

Prefer focused tests for the changed behavior, then broader checks when appropriate. Do not install new tools or dependencies without need or approval.

## 3. Runtime verification

When the task requires proving that an application or workflow runs:
1. Identify the real entry point and required environment.
2. Start it using the documented command.
3. Observe startup output and logs.
4. Exercise the requested workflow, not merely the startup path.
5. Verify the expected output or side effect.
6. Check for regressions and unintended filesystem changes.

A successful import, process launch, open browser, or HTTP status alone does not prove the complete workflow works.

## 4. Git review

Before committing:
- inspect `git diff` and `git status`;
- ensure only intended files are staged;
- do not stage unrelated user changes.

Commit or push only when requested or clearly authorized. Never claim a commit or push succeeded unless its result was observed.

## 5. Report evidence precisely

Use the strongest accurate status:

- **STATICALLY CHECKED** — code/configuration inspected; no runtime proof.
- **TESTED** — named tests/checks actually ran and passed.
- **RUNTIME VERIFIED** — application/workflow was executed and expected behavior observed.
- **END-TO-END VERIFIED** — all required stages and externally visible side effects were verified.

Report commands/checks that ran, failures, skipped checks, and limitations. Never fabricate test output or claim success from inspection alone.
