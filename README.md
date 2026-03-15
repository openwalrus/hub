# Walrus Hub

A community registry for AI agent resources — MCP servers, skills, services, agents, prompts, and tools.

Resources are indexed as `scope/config.toml` files in this repository. Anyone can add resources by opening a PR.

## Index Format

Each resource lives at `<scope>/<config>.toml`:

```
microsoft/playwright.toml
openwalrus/memory.toml
my-org/my-tool.toml
```

`scope` is typically the author, organization, or tool namespace. One `.toml` file can declare multiple related resources under the same scope.

### config.toml structure

```toml
[package]
name = "my-package"

[mcp_servers.my-mcp]
description = "My MCP server"
command = "npx"
args = ["-y", "@my-org/mcp"]
keywords = ["mcp"]

[skills.my-skill]
description = "A downloadable skill."
repo = "https://github.com/my-org/my-skill"
path = "skills/my-skill"

[services.my-service]
description = "A managed WHS service"
kind = "hook"                           # hook | client | process
command = "my-service"                  # binary name or path
install = { command = "cargo", args = ["install", "my-service"] }
restart = "on_failure"                  # never | on_failure | always
config = {}                             # opaque JSON forwarded to the service

[agents.my-agent]
description = "An agent with a bundled prompt."
prompt = "prompts/my-agent.md"
skills = ["my-skill"]
```

Supported sections: `mcp_servers`, `skills`, `services`, `agents`.

### Services

Services are external processes managed by the walrus daemon. The `install` field specifies how to install the service binary — it is executed during `walrus hub install` and not written to `walrus.toml`.

| Field | Required | Description |
|-------|----------|-------------|
| `description` | yes | Human-readable description |
| `kind` | no | `hook` (default), `client`, or `process` |
| `command` | yes | Binary name or path to execute |
| `install` | no | `{ command, args }` — install command run during hub install |
| `restart` | no | `never` (default), `on_failure`, or `always` |
| `config` | no | Opaque JSON forwarded to the service via WHS |

## Examples

- [microsoft/playwright.toml](microsoft/playwright.toml) — MCP server and CLI skill for browser automation with Playwright.
- [openwalrus/memory.toml](openwalrus/memory.toml) — Graph memory service with entity and relation tracking.
- [openwalrus/search.toml](openwalrus/search.toml) — Meta-search aggregator (DuckDuckGo, Wikipedia).
- [openwalrus/telegram.toml](openwalrus/telegram.toml) — Telegram bot gateway for agents.
- [openwalrus/discord.toml](openwalrus/discord.toml) — Discord bot gateway for agents.

## Add a Resource

1. Fork this repo
2. Create `<scope>/<config>.toml` following the format above
3. Open a PR — the PR template will guide you through the checklist

## License

[MIT](LICENSE)
