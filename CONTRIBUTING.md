# Contributing

Thanks for your interest! The project is in its design phase, so the most helpful contributions right now are feedback on the [docs](docs/) and ideas in [issues](https://github.com/lydongcanh/agent-inc/issues).

> **License:** Agent Inc is source-available (PolyForm Strict or PolyForm Internal Use, see [LICENSE.md](LICENSE.md)), not open source. A contributor agreement for code contributions is coming soon. Until it's added, please open an issue before starting on a pull request.

## Prerequisites

You only need these to build from source. Players using release builds don't need them.

| Tool | Version | Notes |
| --- | --- | --- |
| [Godot Engine, **.NET** edition](https://godotengine.org/download) | 4.7 | You must use the .NET edition; the standard edition can't open C# projects. |
| [.NET SDK](https://dotnet.microsoft.com/download) | 10.0 | Builds the game and the forwarder. |
| Git | any | |

To build the forwarder ahead of time (Native AOT), you also need your platform's native toolchain: Xcode Command Line Tools on macOS, the "Desktop development with C++" workload on Windows, or `clang` on Linux.

## Getting started

```sh
git clone https://github.com/lydongcanh/agent-inc.git
cd agent-inc
```

Open the `game/` folder in Godot (.NET), then press **F5** to run.

### Testing without a real agent

You don't need to run Claude Code or ChatGPT to work on the game. Write fake events to a session file and the game will pick them up:

```sh
mkdir -p ~/.agent-inc/sessions/claude-code
echo '{"v":1,"ts":"2026-11-03T15:04:05.123Z","source":"claude-code","session":"demo-1","agent":null,"kind":"tool.start","tool":"Bash","call":"demo-call-1","category":"run","project":"demo","hook":"PreToolUse"}' \
  >> ~/.agent-inc/sessions/claude-code/demo-1.jsonl
```

A replay script for sample sessions is planned.

## Project layout

See [Architecture: Code layout](docs/architecture.md#code-layout).

## Conventions

- **C#** for all game and tool code. Follow standard .NET naming: `PascalCase` types and members, `_camelCase` private fields.
- **One feature per pull request**, with a clear description and screenshots or GIFs for visual changes.
- **Commits:** short, imperative subject lines, such as `Add session watcher`.
- **Assets:** only add assets you made or that have a license allowing use in this project. Record the source and license in `assets/CREDITS.md`.

## Privacy rules for contributors

- **Never commit real hook payloads, session files or transcripts.** They can contain prompts, code and paths. Write synthetic test data by hand.
- **No personal or machine-specific info** in committed files: no absolute paths like `/Users/<name>/...`, no emails, no tokens. Editor settings such as `.vscode/settings.json` are gitignored for this reason.
- Changes that make the forwarder keep **more** data must update [docs/privacy.md](docs/privacy.md) and be opt-in.

## Reporting bugs

Open an issue with your OS, the game version, and your agent and its version. Describe what happened, but don't attach raw session files.
