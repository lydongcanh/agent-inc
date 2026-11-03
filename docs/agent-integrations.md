# Agent integrations

This guide shows how to connect Claude Code and ChatGPT (formerly Codex) to Agent Inc. Both use the same forwarder, `agent-inc-hook`. You only change your agent's hook config. Your agents don't need to know anything about the game.

> **Pre-alpha:** `agent-inc-hook` hasn't been released yet. The steps below are the planned setup. The game will also offer a one-click **Connect agents** screen that writes these configs for you, and makes a backup first.

In the examples, `<AGENT_INC_HOOK>` stands for the absolute path to the forwarder:

| Platform | Path |
| --- | --- |
| macOS / Linux | `~/.agent-inc/bin/agent-inc-hook` (use the full path, e.g. `/home/you/.agent-inc/bin/agent-inc-hook`) |
| Windows | `%USERPROFILE%\.agent-inc\bin\agent-inc-hook.exe` |

## How the forwarder behaves

These rules hold for every agent:

- It reads one JSON payload from stdin. That is how both agents deliver hook data.
- It **prints nothing** to stdout. This matters because, for some events, both agents add hook stdout to the model's context. That would cost tokens and could confuse the agent.
- It **always exits `0`**. Exit code `2` would block the agent's action, so the forwarder never uses it.
- It finishes in milliseconds. That's important for ChatGPT's `SessionEnd`, which runs synchronously with a 1-second time limit.
- If anything goes wrong (an unwritable disk, unexpected JSON), it gives up quietly. Your agent session is never affected.

## Claude Code

Docs: [Claude Code hooks reference](https://code.claude.com/docs/en/hooks)

Add the hooks to your **user** settings file, `~/.claude/settings.json`, so they apply to every project. They don't belong in a project's `.claude/settings.json`, because that file is shared with your team.

Every handler uses `"async": true`, so Claude Code doesn't wait for it.

```json
{
  "hooks": {
    "SessionStart":       [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "SessionEnd":         [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "UserPromptSubmit":   [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "PreToolUse":         [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "PostToolUse":        [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "PostToolUseFailure": [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "PermissionRequest":  [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "Notification":       [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "Stop":               [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "SubagentStart":      [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "SubagentStop":       [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }],
    "PreCompact":         [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> claude-code", "async": true }] }]
  }
}
```

Notes:

- If your settings file already has a `hooks` section, **merge** these entries into it. Don't replace it. Hooks for the same event all run in parallel, so they won't interfere with your existing hooks.
- No `matcher` is set, so the hooks fire for every tool.
- Use `/hooks` inside Claude Code to check that the hooks are registered.

## ChatGPT (formerly Codex)

Docs: [ChatGPT hooks](https://learn.chatgpt.com/docs/hooks)

Add the hooks to your user file, `~/.codex/hooks.json` (or `$CODEX_HOME/hooks.json`). You can also add the same entries as `[hooks]` tables in `~/.codex/config.toml`. Use one place or the other, not both.

```json
{
  "hooks": {
    "SessionStart":      [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "SessionEnd":        [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "timeout": 1 }] }],
    "UserPromptSubmit":  [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "PreToolUse":        [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "PostToolUse":       [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "PermissionRequest": [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "Stop":              [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "Interrupt":         [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "SubagentStart":     [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "SubagentStop":      [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }],
    "PreCompact":        [{ "hooks": [{ "type": "command", "command": "<AGENT_INC_HOOK> codex", "async": true }] }]
  }
}
```

Notes:

- **You must trust the hooks once.** ChatGPT won't run new or changed hooks until you review them. Open **Settings → Hooks** in the ChatGPT app (or run `/hooks` in the CLI) and trust the Agent Inc entries. If the forwarder command changes later, you'll need to trust it again.
- `SessionEnd` always runs synchronously in ChatGPT, with a 1-second default timeout (3 seconds at most). That's why it has no `async` flag. The forwarder is fast enough for this.
- Hooks are on by default. If you turned them off, remove `hooks = false` from the `[features]` section of `config.toml`.
- The older `notify` setting is not used. It only reports finished turns.

## Check that it works

1. Start the game, or skip this step and just inspect the files.
2. Start a new agent session and ask it something simple.
3. A new file should appear:

   ```sh
   ls ~/.agent-inc/sessions/claude-code/   # or sessions/codex/
   tail -f ~/.agent-inc/sessions/claude-code/<session_id>.jsonl
   ```

4. A worker should walk into the office.

## Troubleshooting

| Symptom | Things to check |
| --- | --- |
| No session file appears | Is the forwarder path absolute and correct? Can you run `<AGENT_INC_HOOK> --version` in a terminal? Does `/hooks` in your agent list the hooks? For ChatGPT, have you trusted them? |
| The file appears but the game shows nothing | If you set `AGENT_INC_HOME`, do the game and the agent both see the same value? Is the file being written right now (`tail -f`)? |
| A worker never leaves | The agent probably exited without `SessionEnd`, for example after a crash. The worker leaves automatically after the stale-session timeout (2 hours by default). |
| Agent prints "hook error" | Please [open an issue](https://github.com/lydongcanh/agent-inc/issues) and include your OS and agent version. Don't paste raw hook payloads: they can contain your prompts and file paths. |

## Uninstall

1. Remove the Agent Inc entries from `~/.claude/settings.json` and `~/.codex/hooks.json`. The game's **Disconnect agents** button will do this for you.
2. Delete `~/.agent-inc/`.

## Adding another agent

Any agent can be supported if it can run a command on lifecycle events and pass JSON to it. To add one:

1. Write an adapter in the forwarder that maps the agent's hook payload to the [event format](architecture.md#event-format-v1).
2. Add a `source` name, for example `my-agent`.
3. Document the setup in this file.

Open an issue first so we can agree on the event mapping.
