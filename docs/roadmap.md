# Roadmap

The plan is in phases. Each phase should produce something usable before the next one starts. Scope may change, and anything not in a phase yet is just an idea.

## Phase 0: Design (current)

- [x] Initialize the Godot project (Godot 4.7, .NET)
- [x] Write the vision, architecture, integration and privacy docs
- [x] Choose a license (PolyForm Strict or Internal Use, 1.0.0)
- [x] Verify real hook payloads against the event format.
- [x] Technical decisions: repo layout, .NET version, forwarder approach
- [ ] Choose an art direction (low-poly, voxel or stylized) and the asset sources

## Phase 1: Observer MVP

Goal: open the game and see your real agents working.

- [ ] Set up the code layout (move the Godot project to `game/`)
- [ ] `agent-inc-hook` forwarder with Claude Code and ChatGPT (formerly Codex) adapters
- [ ] Session file writer, with ID sanitizing and retention cleanup
- [ ] Session watcher and office state model in the game
- [ ] Debug event list in the game (no 3D yet)
- [ ] A single office room: one desk per session, with workers walking in and out
- [ ] Basic animations: idle, working (one per tool category), needs attention, done
- [ ] **Connect agents** / **Disconnect agents** screen that safely edits the hook configs
- [ ] Release builds for Windows, macOS and Linux

## Phase 2: Office life

- [ ] Subagents appear as interns
- [ ] Desks grouped by project
- [ ] Desktop notification and sound when an agent needs attention
- [ ] A session timeline panel: what each worker did, as metadata only
- [ ] Camera controls, and a focus mode that follows one worker
- [ ] Office customization: desks, decor, layout

## Phase 3: Mini-games

- [ ] A mini-game framework: start and pause any time, and get interrupted when an agent needs you
- [ ] The first mini-games, such as a desk-toy game or an arcade cabinet in the break room
- [ ] Optional rewards tied to agent activity, for example cosmetics unlocked by completed sessions

## Phase 4: Talk back (research)

These features would add opt-in two-way interaction. They need research before they can be designed:

- [ ] Approve or deny permission requests from inside the office
- [ ] Send a prompt to a running agent
- [ ] Start a new agent session from the office

Any talk-back feature must be opt-in and must leave observe-only mode unchanged. See [Architecture: Future](architecture.md#future-talking-back).

## More ideas

- More agents (anything with command hooks)
- Multi-machine offices (would need a network transport, so it is opt-in by definition)
- Stats: busiest hours, most-used tools
- Contributor agreement (needed before accepting outside code)
