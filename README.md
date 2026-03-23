# Traffic

**Coordination protocol for AI agents.** Prevents file collisions when multiple agents edit the same codebase concurrently.

Traffic sits between your AI agents and the filesystem, ensuring they don't step on each other's work. When two agents try to edit the same file, Traffic detects the collision and lets you decide how to resolve it — before code gets overwritten.

## How it works

1. **Agents register work** — each agent calls `begin_task` with the files it plans to edit
2. **Traffic tracks intents** — file scopes are reserved, preventing overlapping writes
3. **Collisions are detected** — if two agents claim the same file, Traffic blocks the newcomer and notifies you
4. **You resolve** — continue, pause, or cancel either agent's task
5. **Work completes** — agents call `end_task`, intents are released on commit

## Components

| Component | Description | License |
|-----------|-------------|---------|
| **Engine** | Daemon + coordination logic. Runs on `localhost:4747`. | Apache 2.0 (open source) |
| **CLI** | `traffic setup`, `traffic start`, `traffic check-write` | Apache 2.0 (open source) |
| **MCP Server** | Streamable HTTP transport — agents connect via Model Context Protocol | Apache 2.0 (open source) |
| **Navigator** | Desktop app for monitoring agents, resolving collisions, and managing coordination | Proprietary (free to use) |

## Supported agents

Traffic works with any agent that supports MCP and file-system hooks:

- Claude Code
- Cursor
- Windsurf
- Cline
- Augment Code
- Gemini CLI
- OpenCode

## Quick start

```bash
# Install
npm install -g @traffic-mcp/engine

# Set up agents (auto-detects installed agents, configures MCP + hooks)
traffic setup

# Start the daemon
traffic start

# That's it — your agents are now coordinated
```

## Navigator app

The Navigator desktop app gives you a real-time dashboard for multi-agent coordination:

- See all active agents and their file reservations
- Resolve collisions with one click
- Pause, resume, or cancel agent tasks
- View file diffs and coordination history

**[Download Navigator →](https://github.com/traffic-mcp/traffic/releases)**

## Documentation

Coming soon at [trafficnav.dev](https://trafficnav.dev).

## License

The Traffic **engine, CLI, and MCP server** are open source under the [Apache License 2.0](LICENSE).

The Traffic **Navigator** desktop app is proprietary software. The binaries available on the [Releases](https://github.com/traffic-mcp/traffic/releases) page are free to download and use, but the source code is not included in this repository and is not covered by the Apache License. See [NOTICE](NOTICE) for details.
