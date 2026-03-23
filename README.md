<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/wordmark-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="assets/wordmark-light.svg">
  <img alt="Traffic" src="assets/wordmark-light.svg" width="280">
</picture>

<br/><br/>

**Coordination protocol for AI agents.**<br/>
Prevents file collisions when multiple agents edit the same codebase concurrently.

<br/>

[![License](https://img.shields.io/badge/Engine-Apache%202.0-blue?style=flat-square)](LICENSE)
[![Navigator](https://img.shields.io/badge/Navigator-Free%20Download-orange?style=flat-square)](https://github.com/traffic-mcp/traffic/releases)
[![Node](https://img.shields.io/badge/node-%3E%3D20-brightgreen?style=flat-square)](https://nodejs.org)

<!-- TODO: Uncomment once published to npm -->
<!-- [![npm](https://img.shields.io/npm/v/@traffic-mcp/engine?style=flat-square&label=engine)](https://www.npmjs.com/package/@traffic-mcp/engine) -->

[Quick Start](#quick-start) · [Navigator App](#navigator-app) · [How It Works](#how-it-works) · [Docs](https://trafficmcp.com)

</div>

<br/>

Traffic sits between your AI agents and the filesystem, ensuring they don't step on each other's work. When two agents try to edit the same file, Traffic detects the collision and lets you decide how to resolve it — before code gets overwritten.

<!-- TODO: Add demo GIF or screenshot -->
<!-- <div align="center">
  <br/>
  <img src="assets/demo.gif" alt="Traffic in action" width="720" />
  <br/><br/>
</div> -->

## Quick Start

```bash
# Install
npm install -g @traffic-mcp/engine

# Set up agents (auto-detects installed agents, configures MCP + hooks)
traffic setup

# Start the daemon
traffic start

# That's it — your agents are now coordinated
```

## How It Works

```
  Agent A                    Traffic Engine                    Agent B
  ───────                    ──────────────                    ───────
     │                            │                               │
     ├── begin_task ─────────────►│                               │
     │   (scopes: auth.ts)        │◄────────── begin_task ────────┤
     │                            │            (scopes: auth.ts)  │
     │                            │                               │
     │                       ┌────┴────┐                          │
     │                       │COLLISION│                          │
     │                       │DETECTED │                          │
     │                       └────┬────┘                          │
     │                            │                               │
     │                            ├── blocked ───────────────────►│
     │◄── allowed ────────────────┤                               │
     │                            │                               │
     ├── writes auth.ts           │         Agent B waits for     │
     ├── end_task ───────────────►│         resolution            │
     │                            │                               │
     │                            ├── unblocked ─────────────────►│
     │                            │                               │
```

1. **Agents register work** — each agent calls `begin_task` with the files it plans to edit
2. **Traffic tracks intents** — file scopes are reserved, preventing overlapping writes
3. **Collisions are detected** — if two agents claim the same file, Traffic blocks the newcomer
4. **You resolve** — continue, pause, or cancel either agent's task
5. **Work completes** — agents call `end_task`, intents are released on commit

## Supported Agents

| Agent | Integration | Write Protection |
|-------|------------|-----------------|
| Claude Code | MCP + PreToolUse hooks | ✅ Enforced |
| Cursor | MCP + hooks | ✅ Enforced |
| Windsurf | MCP + hooks | ✅ Enforced |
| Cline | MCP + hooks | ✅ Enforced |
| Augment Code | MCP + hooks | ✅ Enforced |
| Gemini CLI | MCP + hooks | ✅ Enforced |
| OpenCode | MCP + plugin hooks | ✅ Enforced |

## Components

| Component | Description | License |
|-----------|-------------|---------|
| **Engine** | Coordination daemon on `localhost:4747` — intent tracking, collision detection, write attribution | Apache 2.0 |
| **CLI** | `traffic setup` · `traffic start` · `traffic check-write` | Apache 2.0 |
| **MCP Server** | Streamable HTTP — agents connect via [Model Context Protocol](https://modelcontextprotocol.io) | Apache 2.0 |
| **Navigator** | Desktop dashboard — monitor agents, resolve collisions, manage coordination | Proprietary (free) |

## Navigator App

The Navigator desktop app gives you a real-time dashboard for multi-agent coordination:

- **Live task view** — see all active agents and their file reservations
- **Collision resolution** — resolve conflicts with one click
- **Task lifecycle** — pause, resume, or cancel agent tasks
- **File diffs** — inline diff previews for contested files
- **Terminal management** — embedded terminals for each agent session

<!-- TODO: Add screenshot -->
<!-- <div align="center">
  <br/>
  <img src="assets/navigator.png" alt="Traffic Navigator" width="720" />
  <br/><br/>
</div> -->

**[Download Navigator →](https://github.com/traffic-mcp/traffic/releases)**

<sub>Available for macOS and Linux. Windows coming soon.</sub>

## Documentation

Coming soon at [trafficmcp.com](https://trafficmcp.com).

## License

The Traffic **engine, CLI, and MCP server** are open source under the [Apache License 2.0](LICENSE).

The Traffic **Navigator** desktop app is proprietary software. Binaries on the [Releases](https://github.com/traffic-mcp/traffic/releases) page are free to download and use, but the source code is not covered by the Apache License. See [NOTICE](NOTICE) for details.

---

<div align="center">
  <sub>Built by <a href="https://github.com/ethanrader">Ethan Rader</a></sub>
</div>
