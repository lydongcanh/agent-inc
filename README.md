# Agent Inc

**Watch your AI coding agents work in a 3D office.**

Agent Inc is a desktop game built with [Godot](https://godotengine.org/). It turns your real Claude Code and Codex sessions into office workers. When an agent reads files, it heads to the bookshelf. When it runs commands, it types at a terminal. When it needs your approval, it raises a hand.

The agents don't know the game exists. Agent Inc only listens to the lifecycle **hooks** that Claude Code and Codex already provide, so it spends **zero tokens** and never slows your agents down.

> **Status: pre-alpha, design phase.** No playable build exists yet. These docs describe how the game will work. See the [roadmap](docs/roadmap.md).

## Features

| Feature | Status |
| --- | --- |
| See every running agent session as a worker at a desk, in near real time | Planned (MVP) |
| Activity animations based on what the agent is doing: reading, editing, running commands, browsing, delegating | Planned (MVP) |
| Workers raise a hand when an agent needs your attention | Planned (MVP) |
| Subagents show up as interns | Planned |
| Mini-games to play while your agents work | Planned |
| Talk back: approve requests or send prompts from inside the office (opt-in) | Research |

## How it works

```
Claude Code / Codex ──hook──▶ agent-inc-hook ──append──▶ ~/.agent-inc/sessions/*.jsonl ──watch──▶ Agent Inc (game)
```

1. You register a tiny program, `agent-inc-hook`, as an **async** hook in Claude Code and/or Codex.
2. On each hook event, the agent passes the program a small JSON payload. The program keeps only metadata (event type, tool name, session ID), appends one line to a local file, prints nothing and exits.
3. The game watches those files and animates the office.

Nothing leaves your machine. The game has no telemetry and makes no network calls. Details are in [Architecture](docs/architecture.md) and [Privacy](docs/privacy.md).

## Supported agents

| Agent | Integration | Status |
| --- | --- | --- |
| [Claude Code](https://code.claude.com/docs/en/hooks) | Hooks in `~/.claude/settings.json` | Planned (MVP) |
| [Codex CLI](https://learn.chatgpt.com/docs/hooks) | Hooks in `~/.codex/hooks.json` | Planned (MVP) |

Setup steps are in [Agent integrations](docs/agent-integrations.md).

## Install

Builds for Windows, macOS and Linux will be published on the [Releases](https://github.com/lydongcanh/agent-inc/releases) page. They will include everything you need; you won't have to install .NET or Godot.

To build from source, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Documentation

| Doc | What it covers |
| --- | --- |
| [Architecture](docs/architecture.md) | Components, data flow, event format and design principles |
| [Agent integrations](docs/agent-integrations.md) | Connecting Claude Code and Codex, plus troubleshooting |
| [Privacy](docs/privacy.md) | What data is read, what is stored, and what is never stored |
| [Roadmap](docs/roadmap.md) | Phases and what's planned next |
| [Contributing](CONTRIBUTING.md) | Dev setup, project layout and conventions |

## License

Agent Inc is **source-available** under the [PolyForm Strict License 1.0.0](LICENSE.md):

- ✅ You may use it for free for any **noncommercial** purpose, such as personal use, hobby projects or study.
- ❌ You may not redistribute it, sell it, or publish modified versions.
- For any other use, including commercial use, please [open an issue](https://github.com/lydongcanh/agent-inc/issues) to ask.

Agent Inc is an independent fan project. It is not affiliated with or endorsed by Anthropic or OpenAI. "Claude" and "Codex" are trademarks of their respective owners.
