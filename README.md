# Python OpenCode project template

A small, reusable OpenCode baseline for Python repositories.

## Included

- `AGENTS.md` — short project-wide rules.
- `.opencode/skills/code-search/` — targeted repository exploration with `rg`, Serena, and Semble.
- `.opencode/skills/code-modification/` — safe implementation workflow.
- `.opencode/skills/code-verification/` — tests, static checks, runtime checks, and Git review.
- `opencode.json` — Pyright LSP, Serena, and Semble MCP configuration.

No framework, package manager, architecture, CI system, or application entry point is assumed.

## Requirements

Install these tools. `dev-env init` installs the Mise-managed ones; the commands
below assume the tools are available on `PATH` unless noted otherwise.

- `opencode` 1.x — `dev-env init` installs it through Mise as `npm:opencode-ai`,
  including the `allow_builds` setting required by its postinstall step. For a
  manual setup, install OpenCode 1.x yourself and make `opencode` available on
  `PATH` (or run it through `mise exec -- opencode ...`).
- `serena`
- `uvx`
- `pyright-langserver` (or adjust/remove the Pyright LSP entry if you use another installation)

The Serena command intentionally uses `serena` rather than a machine-specific absolute path. Install Serena in the environment used to launch OpenCode.

Semble is launched with `uvx` and pinned to `semble[mcp]==0.5.5` for repeatability. Update the pin deliberately when upgrading.

## Context7

Context7 is not duplicated in this project config because it is already configured in the user's global OpenCode config. This avoids two Context7 server definitions and avoids project-level config overriding the global authentication settings.

For a portable setup on another machine, add Context7 once to the global OpenCode config (`~/.config/opencode/opencode.json`):

```json
"mcp": {
  "context7": {
    "type": "remote",
    "url": "https://mcp.context7.com/mcp",
    "enabled": true,
    "headers": {
      "Authorization": "Bearer {env:CONTEXT7_API_KEY}"
    }
  }
}
```

Merge this entry into the existing `mcp` object; do not replace other MCP servers. Set `CONTEXT7_API_KEY` in the environment from which OpenCode is launched. If using Context7 without an API key, omit `headers` entirely.

The global config already has Context7 in the setup described when this template was prepared. Keep that one global definition; do not add another `context7` entry to this project's `opencode.json`.

## Validate after installation

From the project root:

```bash
mise exec -- opencode --version
mise exec -- opencode mcp list
mise exec -- opencode models
```

Or, if OpenCode is already on `PATH`, omit `mise exec --`.

In a project initialized by `dev-env`, a `serve` task runs OpenCode without the
TUI (for example, for a bot that uses its HTTP API):

```bash
mise run serve                    # http://127.0.0.1:4096
OPENCODE_PORT=4099 mise run serve # custom port
```

Then open the project in OpenCode and verify that:
- Serena and Semble connect;
- Context7 is available from the global config;
- Pyright starts (check the OpenCode LSP status/logs);
- the three skills are discoverable.

If a tool is not installed or does not connect, fix that environment first; do not treat the presence of a JSON entry as proof that the service works.

## Adapt per project

Add project-specific instructions only when they are genuinely needed. Keep framework-specific workflows, deployment procedures, database conventions, and CI commands in dedicated skills or project documentation instead of expanding the global baseline.
